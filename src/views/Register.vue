<template>
  <div class="auth-page">
    <div class="container page">
      <div class="row">
        <div class="col-md-6 offset-md-3 col-xs-12">
          <h1 class="text-xs-center">Sign up</h1>
          <p class="text-xs-center">
            <router-link :to="{name: 'login'}">Have an account?</router-link>
          </p>
          <mcv-validationErrors
            v-if="validationErrors"
            :validation-errors="validationErrors"
          />
          <form @submit.prevent="onSubmit">
            <fieldset class="form-group">
              <input
                type="text"
                class="form-control form-control-lg"
                placeholder="username"
                v-model="username"
              />
            </fieldset>

            <fieldset class="form-group">
              <input
                type="text"
                class="form-control form-control-lg"
                placeholder="Email"
                v-model="email"
              />
            </fieldset>

            <fieldset class="form-group">
              <input
                type="password"
                class="form-control form-control-lg"
                placeholder="password"
                v-model="password"
              />
            </fieldset>
            <button
              class="btn btn-lg btn-primary pull-xs-right"
              :disabled="isSubmitting"
            >
              Sign Up
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// import { defineComponent } from '@vue/composition-api'
import McvValidationErrors from '@/components/ValidationErrors'
import {actionTypes} from '@/store/modules/auth'

import {mapState} from 'vuex'

export default {
  name: 'McvRegister',
  components: {
    McvValidationErrors,
  },
  data() {
    return {
      email: '',
      password: '',
      username: '',
    }
  },
  computed: {
    ...mapState({
      isSubmitting: (state) => state.auth.isSubmitting,
      validationErrors: (state) => state.auth.validationErrors,
    }),
    // isSubmitting() {
    //   return this.$store.state.auth.isSubmitting
    // },
    // validationErrors() {
    //   return this.$store.state.auth.validationErrors
    // },
  },
  methods: {
    onSubmit() {
      console.log('submitted form')
      // this.$store.commit('registerStart') тут коммитили мутацию. Теперь вместо этого:
      this.$store
        .dispatch(actionTypes.register, {
          email: this.email,
          username: this.username,
          password: this.password,
        }) /// диспатчим экшены
        .then((user) => {
          console.log('succesfully register user', user)
          this.$router.push({name: 'home'})
        })
    },
  },
}
</script>
