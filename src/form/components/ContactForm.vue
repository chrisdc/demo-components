<template>
  <form method="post" v-bind:action="action">
    <h2>{{ title }}</h2>

    <div class="form-group">
      <label for="name">Name:</label>
      <input name="name" v-model.trim="$v.name.$model" />

      <div class="errors" v-if="$v.name.$error">
        <span class="error" v-if="!$v.name.required">Field is required</span>
      </div>
    </div>

    <div class="form-group">
      <label for="email">Email:</label>
      <input name="email" v-model="email" />

      <div class="errors" v-if="$v.email.$error">
        <span class="error" v-if="!$v.email.required">Field is required</span>
        <span class="error" v-if="!$v.email.email">Enter a valid email</span>
      </div>
    </div>

    <div class="form-group">
      <label for="company">Company:</label>
      <input name="company" v-model="company" />
    </div>

    <div class="form-group">
      <label for="enquiry">Enquiry:</label>
      <input name="enquiry" v-model="enquiry" />

      <div class="errors" v-if="$v.enquiry.$error">
        <span class="error" v-if="!$v.enquiry.required">Field is required</span>
      </div>
    </div>

    <div class="form-group">
      <input type="submit" value="Submit" v-on:click="submit"/>
    </div>
  </form>
</template>

<script>
  import FormInput from './FormInput.vue';
  import { validationMixin } from 'vuelidate';
  import { required, alpha, email } from 'vuelidate/lib/validators'
  import axios from 'axios'

  export default {
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
          this.$v.$touch()
        if (this.$v.$invalid) {
          this.status = 'error'
        } else {
          return axios.post(this.action, {
            name: this.name,
            email: this.email,
            company: this.company,
            enquiry: this.enquiry
          }).then(function (response) {
            this.submitStatus = 'sent'
          }).catch(function (error) {
            this.submitStatus = 'failed'
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
        sent: false,
        failed: false // Unable to connect to server
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

<style>
  html {
    background-color: #f8f8f8;
  }
</style>
