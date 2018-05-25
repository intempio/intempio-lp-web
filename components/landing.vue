<template>
    <div>
      <section class="section">
        <div class="columns">
          <div class="column is-half is-vertical-center" style="padding-left:40px; padding-right: 40px">
              <h1 class="title" v-html="client.title"></h1>
          </div>

          <div class="column is-half" style="padding-left:40px; padding-right: 40px">
            <figure class="image is-350x150 is-pulled-right" style="max-width: 300px">
              <img :src="logoURL">
            </figure>
        </div>
        </div>
      </section>
      <section class="section">
       <div class="columns">
          <div class="column is-half" style="padding-left:40px; padding-right: 40px">
            <p class=content>
              To join the session please fill out the information on the right and click 'Join'.
              The email address and program ID must match the ones you received in your registration email.
            </p>

            <p class="content">
              When prompted please enter a direct dial phone number (does not work with extensions)
              and the system will call you for meeting audio. Note: Please see Biogen’s privacy policy
              below for more information.
            </p>

            <p class="content">
              If you need technical help please click on the Chat icon for assistance.
            </p>
          </div>
          <div class="column is-half" style="padding-left:40px; padding-right: 40px">

            <div class="content">
              <div class="error-message" v-for="(error, index) in errors" :key="index">{{error}}</div>
            </div>

           <form v-on:submit.prevent="submitForm">
              <b-field grouped :type="nameType">
                  <b-input placeholder="First Name" expanded v-model="firstName"></b-input>
                  <b-input placeholder="Last Name" expanded v-model="lastName"></b-input>
              </b-field>
              <b-field :type="emailType">
                  <b-input type="email" placeholder="Email Address" v-model="email" v-bind:class="{ 'error-input': this.emailType == 'is-danger' }">
                  </b-input>
              </b-field>
              <b-field :type="programMeetingType">
                  <b-input placeholder="Program ID" type="number" v-model="programMeetingId"></b-input>
              </b-field>

              <br />

              <button class="button is-fullwidth is-rounded is-outlined submit" type="submit">Join</button>
           </form>
          </div>
       </div>
      </section>
      <section class="section tos">
        <div class="columns" v-if="this.brand !== 'Biogen'">
          <div class="column has-text-centered">
            <strong v-if="client.privacyPolicy"><a :href="client.privacyPolicy" target="_blank">Privacy Policy</a></strong>
          </div>

          <div class="column has-text-centered">
            <strong v-if="client.importantSafetyInformation"><a :href="client.importantSafetyInformation" target="_blank">Important Safety Information</a></strong>
          </div>
          <div class="column has-text-centered">
            <strong v-if="client.prescribingInformation"><a :href="client.prescribingInformation" target="_blank">Prescribing Information</a></strong>
          </div>

          <div class="column has-text-centered">
            <strong v-if="client.termsOfUse"><a :href="client.termsOfUse" target="_blank">Terms & Conditions</a></strong>
          </div>
        </div>

        <div class="columns" v-else>
          <div class="column has-text-centered">
            <strong v-if="client.privacyPolicy"><a :href="client.privacyPolicy" target="_blank">Privacy Policy</a></strong>
          </div>

          <div class="column has-text-centered">
            <strong v-if="client.termsOfUse"><a :href="client.termsOfUse" target="_blank">Terms & Conditions</a></strong>
          </div>
        </div>
      </section>

      <footer class="footer" style="background: #bfe2fe">
       <div class="columns">
        <div class="column">
          <p>
             2018 Biogen. All rights reserved.
            </p>
        </div>
        <div class="column">
          <p class="is-pulled-right">{{client.footerRightText}}</p>
        </div>
       </div>
      </footer>
      <b-loading :is-full-page="true" :active.sync="isLoading"></b-loading>

      <div id="chat-text1"></div>
    </div>
</template>

<script>
import CLIENTS from '@/utils/clients';

export default {
  props: {
    onFormSubmitUrl: String,
    pagetypeInfo: Object,
    pageUrl: String,
  },
  created() {
    const { brand, page, acLink, title } = this.pagetypeInfo;
    window.history.replaceState({}, document.title, `/${brand}/${page}`);
    this.brand = brand;
    if (brand === 'Spinraza') {
      this.client = CLIENTS[brand];
    } else {
      this.client.title = title;
    }
  },
  computed: {
    logoURL() {
      return this.client.logo === ''
        ? ''
        : require('@/assets/' + this.client.logo);
    },
  },
  data() {
    return {
      errors: [],
      firstName: '',
      lastName: '',
      email: '',
      programMeetingId: '',
      isLoading: false,
      nameType: '',
      emailType: '',
      programMeetingType: '',
      brand: '',
      client: {
        logo: '',
        title: '',
        footerRighText: '',
        privacyPolicy: '',
        termsOfUse: '',
        importantSafetyInformation: '',
        prescribingInformation: '',
      },
    };
  },
  head() {
    return {
      title: `${
        this.pageUrl.indexOf('eod') !== -1 ? 'EOD' : 'Webcast'
      } | Intempio`,
      bodyAttrs: { class: `${this.brand === 'Biogen' ? 'ms-eod' : 'sma-eod'}` },
    };
  },
  methods: {
    submitForm(e) {
      this.errors = [];
      this.nameType = '';
      this.emailType = '';
      this.programMeetingType = '';

      if (
        this.firstName &&
        this.lastName &&
        this.programMeetingId &&
        this.email
      ) {
        let formData = {};
        if (this.pageUrl.indexOf('eod') !== -1) {
          formData = {
            et_pb_contact_first_name_1: this.firstName,
            et_pb_contact_last_name_1: this.lastName,
            et_pb_contact_email_1: this.email.toLowerCase(),
            et_pb_contact_program_id_1: this.programMeetingId.toString(),
          };
        } else {
          formData = {
            et_pb_contact_first_name2_1: this.firstName,
            et_pb_contact_last_name2_1: this.lastName,
            et_pb_contact_email2_1: this.email.toLowerCase(),
            et_pb_contact_program_id2_1: this.programMeetingId.toString(),
          };
        }
        this.isLoading = true;
        this.$axios.post(this.onFormSubmitUrl, formData).then(response => {
          this.isLoading = false;
          const {
            programInfoFound,
            programFound,
            programUserFound,
            programInfo,
          } = response.data;

          if (!programFound) {
            this.errors.push(
              'Incorrect Program ID, please check your email notification and try again.'
            );
            this.programMeetingType = 'is-danger';
          } else {
            const acLink = programInfo.acLink;

            if (this.email.indexOf('@biogen.com') !== -1) {
              this.redirectToAC(acLink);
            } else {
              if (!programUserFound) {
                this.errors.push(
                  'This email address is not registered for this event.  The email address must be entered exactly as it was upon registration.'
                );
                this.emailType = 'is-danger';
              } else {
                this.redirectToAC(acLink);
              }
            }
          }
        });
      }

      if (!this.firstName || !this.lastName) {
        this.errors.push('Please Enter Name');
        this.nameType = 'is-danger';
      } else if (!this.email) {
        this.errors.push('Please Enter Email Address');
        this.emailType = 'is-danger';
      } else if (!this.programMeetingId) {
        this.errors.push('Please Enter Program ID');
        this.programMeetingType = 'is-danger';
      }
    },
    redirectToAC(acLink) {
      window.location.href = `${acLink}?guestName=${this.firstName} ${
        this.lastName
      }`;
    },
  },
};
</script>


<style>
html,
body,
p,
a {
  color: #000;
  font-size: 17px;
}
.footer {
  padding: 3rem;
}

.is-vertical-center {
  display: flex;
  align-items: center;
}
.input,
.taginput .taginput-container.is-focusable,
.textarea {
  background-color: #eeeeee;
  box-shadow: none;
  padding: 16px;
  font-size: 18px;
  border-radius: 0;
  border-color: transparent;
  height: 53px;
}
.input:focus,
.taginput .taginput-container.is-focusable:focus,
.input.is-focused,
.taginput .is-focused.taginput-container.is-focusable,
.input:active,
.taginput .taginput-container.is-focusable:active,
.input.is-active,
.taginput .is-active.taginput-container.is-focusable,
.textarea:focus,
.textarea.is-focused,
.textarea:active,
.textarea.is-active {
  border-color: transparent;
  box-shadow: none;
}
button.submit,
.button.is-rounded {
  border-color: black;
  border-radius: 10px;
  height: 45px;
  color: #000;
  font-size: 18px;
}

button.submit:hover {
  background-color: #bfe2fe !important;
}
.tos a {
  font-size: 14px;
}
.title {
  font-size: 42px;
  font-weight: 400;
}
.error-message {
  color: red;
  font-weight: bold;
  font-size: 14px;
}
.button:focus:not(:active),
.button.is-focused:not(:active) {
  box-shadow: none;
}

.siScale.siInvite {
  height: 220px !important;
  min-height: 220px !important;
  bottom: 30px !important;
}
body.sma-eod .silc-btn-button {
  background-image: url('http://intemp.io/wp-content/uploads/2018/04/wrench_icon.png') !important;
}

body.sma-eod .icon-text {
  visibility: visible;
  display: inline-block;
}
.icon-text {
  visibility: hidden;
  display: none;
}
body.sma-eod #chat-text1:before {
  display: block;
  content: 'Technical Difficulties?';
  width: 110px;
  text-align: center;
}
body.ms-eod #chat-text1:before {
  display: block;
  content: 'Need Help?';
}
#chat-text1 {
  position: fixed;
  right: 12px;
  bottom: 82px;
  color: rgb(0, 174, 239);
  font-weight: bold;
  text-transform: uppercase;
  font-size: 15px;
}

.error-input input {
  background: #fbd4d4;
}
</style>

