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
              To join the session please fill out the information on the right and click ‘Join’.
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
            <b-field grouped>
                  <b-input placeholder="First Name" expanded></b-input>
                  <b-input placeholder="Last Name" expanded></b-input>
            </b-field>
            <b-field>
                <b-input type="email" placeholder="Email Address"
                    value="john@">
                </b-input>
            </b-field>
            <b-field>
                <b-input placeholder="Program Meeting ID" type="number"></b-input>
            </b-field>

            <br />

            <button class="button is-fullwidth is-rounded is-outlined submit" type="submit">Join</button>
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
    </div>
</template>

<script>
export default {
  props: {
    client: Object,
  },
  computed: {
    logoURL() {
      return require('@/assets/' + this.client.logo);
    },
  },
  data() {
    return {
      formData: {
        firstName: '',
        lastName: '',
        email: '',
        programMeetingId: '',
      },
    };
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
  background-color: #bfe2fe !important;
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
.tos a {
  font-size: 14px;
}
.title {
  font-size: 42px;
  font-weight: 400;
}
</style>

