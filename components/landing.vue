<template>
    <div class="section">
      <section class="section">
        <div class="columns is-mobile">
          <div class="column is-half">
              <h1 class="title" v-html="title"></h1>
          </div>

          <div class="column is-half">
            <figure class="image is-350x150 is-pulled-right">
              <img src="http://via.placeholder.com/350x100">
            </figure>
        </div>
        </div>
      </section>
      <section class="section">
       <div class="columns is-mobile">
          <div class="column is-half">
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
          <div class="column is-half">
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

            <button class="button is-fullwidth" type="submit">Join</button>
          </div>
       </div>
      </section>
      <section class="section">
        <div class="columns">
          <div class="column"><a href="#">Privacy Policy</a></div>
          <div class="column"><a href="#">Important Safety Information</a></div>
          <div class="column"><a href="#">Prescribing Information</a></div>
          <div class="column"><a href="#">Terms & Conditions</a></div>
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
          <p class="is-pulled-right">SPZ-US-1315 4/18</p>
        </div>
       </div>
      </footer>
    </div>
</template>

<script>
export default {
  props: {
    title: String,
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
a {
  color: #000;
}
.footer {
  padding: 3rem;
}
</style>

