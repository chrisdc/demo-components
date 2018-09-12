<template>
  <div class="form-wrapper">
    <h2 class="form-header">{{ title }}</h2>
    <form method="post" v-bind:action="action">
      <div class="form-group" v-bind:class="{invalid: $v.name.$error}">
        <label for="name">Name:</label>
        <input name="name" v-model.trim="$v.name.$model" />

        <div class="errors" v-if="$v.name.$error">
          <span class="error" v-if="!$v.name.required">Please enter your name.</span>
        </div>
      </div>

      <div class="form-group" v-bind:class="{invalid: $v.email.$error}">
        <label for="email">Email:</label>
        <input name="email" v-model="email" />

        <div class="errors" v-if="$v.email.$error">
          <span class="error" v-if="!$v.email.required">Please enter your email address.</span>
          <span class="error" v-if="!$v.email.email">Please Enter a valid email address.</span>
        </div>
      </div>

      <div class="form-group">
        <label for="company">Company:</label>
        <input name="company" v-model="company" />
      </div>

      <div class="form-group" v-bind:class="{invalid: $v.enquiry.$error}">
        <label for="enquiry">Enquiry:</label>
        <textarea name="enquiry" v-model="enquiry" rows="6"/>

        <div class="errors" v-if="$v.enquiry.$error">
          <span class="error" v-if="!$v.enquiry.required">Please enter a message.</span>
        </div>
      </div>

      <div class="form-group">
        <input type="submit" v-bind:value="[pending ? 'Sending...': 'Send']" v-on:click="submit" v-bind:disabled="pending"/>
      </div>
    </form>

    <Overlay
      title="Message Sent!"
      message="We'll be in touch as soon as possible."
      v-bind:visible="sent"
      color="#8bc34a"
    />

    <Overlay
      title="Uh Oh!"
      message="We're having trouble sending your message right now. Please try again later."
      v-bind:visible="failed"
      color="#f44336"
    />
  </div>
</template>

<script>
  //import FormInput from './FormInput.vue';
  import Overlay from './Overlay.vue';
  import { validationMixin } from 'vuelidate';
  import { required, alpha, email } from 'vuelidate/lib/validators'
  import axios from 'axios'

  export default {
    components: {
      Overlay
    },
    props: {
      title: {
        type: String,
        default: 'Contact Form'
      },
      action: {
        type: String,
        required: true
      }
    },
    methods: {
      submit: function(e) {
        e.preventDefault();
        this.$v.$touch();

        if (!this.$v.$invalid) {
          this.pending = true;
          return axios.post(this.action, {
            name: this.name,
            email: this.email,
            company: this.company,
            enquiry: this.enquiry
          }).then((response) => {
            this.pending = false;
            this.sent = true;
          }).catch((error) => {
            this.pending = false;
            this.failed = true;
          });
        }
      }
    },
    data: function() {
      return {
        name: '',
        email: '',
        company: '',
        enquiry: '',
        pending: false,
        sent: false,
        failed: false // Indicates a connection or server-side error.
      }
    },
    mixins: [validationMixin],
    validations: {
      name: {
        required,
        alpha
      },
      email: {
        required,
        email
      },
      enquiry: {
        required
      }
    }
  }
</script>

<style scoped>
  .form-wrapper * {
    box-sizing: border-box;
    font-family: 'Roboto', sans-serif;
  }

  .form-header {
    font-family: 'Roboto Slab', serif;
    font-weight: normal;
  }

  .form-wrapper {
    background: #fff;
    box-shadow: 1px 1px 6px #eee;
    margin: 0 auto;
    max-width: 400px;
    padding: 1em;
    position: relative;
  }

  .form-wrapper :first-child {
    margin-top: 0;
  }

  .form-wrapper :last-child {
    margin-bottom: 0;
  }

  /*.form-wrapper .overlay {
    background: #fff;
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    opacity: 0;
    visibility: hidden;
    z-index: 100;
  }*/

  .form-wrapper.sent .sent-message,
  .form-wrapper.failed .failed-message {
    opacity: 1;
    transition: opacity 0.4s;
    visibility: visible;
  }

  input, textarea {
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 1em;
    width: 100%;
    padding: 0.5em;
  }

  textarea {
    resize: none;
  }

  .error {
    color: #f44336;
  }

  input[type="submit"] {
    background-color: #00bcd4;
    border: 0;
    color: #fff;
    font-size: 1em;
    width: 100%;
    padding: 1em;
    transition: background-color 0.2s;
  }

  input[type="submit"]:hover {
    background-color: #0097a7;
  }

  .form-group {
    margin-bottom: 1em;
  }

  .invalid input,
  .invalid textarea {
    border: 1px solid #f44336;
  }

  input:focus,
  textarea:focus {
    box-shadow: 0 0 0 3px rgba(0, 190, 215, 0.2);
    border: 1px solid #4dd0e1;
    outline: 0;
    transition: box-shadow 0.1s;
  }

  label {
    display: block;
    font-size: 0.8125em; /* 13px */
    letter-spacing: 1px;
    text-transform: uppercase;
    margin-bottom: 0.23076923076em; /* 3px */
    font-family: 'Roboto Slab', serif;
  }
</style>
