<template>
  <div>
    <h2 class="mt-3">Here You can sign in to Your profile</h2>
    <ValidationObserver ref="login_observer" v-slot="{ handleSubmit }">
      <b-form @submit.prevent="sendToBack">
        <ValidationProvider rules="required|email" name="Email" v-slot="{ valid, errors }">
          <b-form-group>
            <b-form-input
              class="mt-3"
              type="email"
              v-model="form.email"
              :state="errors[0] ? false : (valid ? true : null)"
              placeholder="Enter email"
            ></b-form-input>
            <b-form-invalid-feedback id="inputLiveFeedback">{{ errors[0] }}</b-form-invalid-feedback>
          </b-form-group>
        </ValidationProvider>
        <ValidationProvider
          rules="required|regex: /^(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$%^&*-]).*$/"
          name="regex"
          vid="password"
          v-slot="{ valid, errors }"
        >
          <b-form-group>
            <b-form-input
              type="password"
              v-model="form.password"
              :state="errors[0] ? false : (valid ? true : null)"
              placeholder="Enter password"
            ></b-form-input>
            <b-form-invalid-feedback id="inputLiveFeedback">{{ errors[0] }}</b-form-invalid-feedback>
          </b-form-group>
        </ValidationProvider>

        <b-button type="submit" variant="primary">Login</b-button>
      </b-form>
    </ValidationObserver>
  </div>
</template>

<script>

import axios from "axios";
import { ValidationObserver, ValidationProvider } from "vee-validate";

  export default {
    name: "BootstrapForm",
    components: {
      ValidationObserver,
      ValidationProvider
    },
    data: () => ({
      form:{
        email: "",
        password: "",
      },

    }),
    methods: {
      sendToBack() {
        this.$store.dispatch('LOGIN_USER', this.form)
          .then(res => {
            if(res){
              this.$router.push({name: 'Profile'})
            }
          })
          .catch(error => {
            console.log(error)
          })
      },

    }
  };

</script>

<style scoped>

</style>
