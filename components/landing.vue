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
                  <b-input type="email" placeholder="Email Address" v-model="email">
                  </b-input>
              </b-field>
              <b-field :type="programMeetingType">
                  <b-input placeholder="Program Meeting ID" type="number" v-model="programMeetingId"></b-input>
              </b-field>

              <br />

              <button class="button is-fullwidth is-rounded is-outlined submit" type="submit">Join</button>
           </form>
          </div>
       </div>
      </section>
      <section class="section tos">
        <div class="columns">
          <div class="column has-text-centered"><strong><a :href="client.privacyPolicy" target="_blank">Privacy Policy</a></strong></div>
          <div class="column has-text-centered"><strong><a :href="client.importantSafetyInformation" target="_blank">Important Safety Information</a></strong></div>
          <div class="column has-text-centered"><strong><a :href="client.prescribingInformation" target="_blank">Prescribing Information</a></strong></div>
          <div class="column has-text-centered"><strong><a :href="client.termsOfUse" target="_blank">Terms & Conditions</a></strong></div>
        </div>
      </section>

      <footer class="footer">
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
    </div>
</template>

<script>
export default {
  props: {
    client: Object,
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
        const formData = {
          et_pb_contact_first_name2_1: this.firstName,
          et_pb_contact_last_name2_1: this.lastName,
          et_pb_contact_email2_1: this.email,
          et_pb_contact_program_id2_1: this.programMeetingId,
        };
        this.isLoading = true;
        this.$axios
          .post('https://runflow.built.io/run/1DGm8LpMUw?sync=true', formData)
          .then(response => {
            this.isLoading = false;
            const {
              programInfoFound,
              programFound,
              programUserFound,
              programInfo,
            } = response.data;

            const acLink = programInfo.acLink;

            if (!programFound) {
              this.errors.push(
                'Incorrect Program ID, please check your email notification and try again.'
              );
              this.programMeetingType = 'is-danger';
            } else {
              if (this.email.contains('@biogen.com')) {
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
      }

      if (!this.email) {
        this.errors.push('Please Enter Email Address');
        this.emailType = 'is-danger';
      }

      if (!this.programMeetingId) {
        this.errors.push('Please Enter Program Meeting ID');
        this.programMeetingType = 'is-danger';
      }
    },
    redirectToAC(acLink) {
      window.open(
        `${acLink}?guestName=${this.firstName} ${this.lastName},_blank`
      );
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
</style>

