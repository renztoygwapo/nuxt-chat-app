<template>
  <div class="h-screen">
    <div class="container mx-auto h-full flex justify-center items-center">
      <div class="w-1/3">
        <h1 class="font-hairline mb-6 text-center">Login to chat app</h1>
        <form v-on:submit.prevent="onSubmit">
          <div class="border-teal p-8 border-t-12 bg-white mb-6 rounded-lg shadow-lg">
            <div class="error-box text-red text-center mb-4" v-if="errors.error" v-text="errors.error"></div>

            <div class="mb-4">
              <label class="font-bold text-grey-darker block mb-2">Username</label>
              <input type="text" name="username" v-validate="'required'" v-model="username" :class="['block appearance-none w-full bg-white border border-grey-light hover:border-grey px-2 py-2 rounded shadow', errors.has('username') ? 'border-red mb-3' : '']" placeholder="Your Username">
              <p v-if="errors.has('username')" class="text-red text-xs italic" v-text="errors.first('username')"></p>
            </div>

            <div class="mb-4">
              <label class="font-bold text-grey-darker block mb-2">Password</label>
              <input type="password" name="password" v-validate="'required'" v-model="password" :class="['block appearance-none w-full bg-white border border-grey-light hover:border-grey px-2 py-2 rounded shadow', errors.has('password') ? 'border-red mb-3' : '']" placeholder="Your Password">
              <p v-if="errors.has('password')" class="text-red text-xs italic" v-text="errors.first('password')">Please choose a password.</p>
            </div>

            <div class="flex items-center justify-between">
              <button type="submit" class="bg-teal-dark hover:bg-teal text-white font-bold py-2 px-4 rounded">
                Login
              </button>
            </div>

          </div>
          <div class="text-center">
            <p class="text-grey-dark text-sm">Don't have an account?
              <router-link to="/register" class="no-underline text-blue font-bold">Create an Account.</router-link>
            </p>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'login',
  middleware: 'no-auth',
  data () {
    return {
      username: null,
      password: null
    }
  },
  methods: {
    async onSubmit () {
      this.$validator.validateAll().then((result) => {
        if (result) {
          this.$auth.login({
            data: {
              username: this.username,
              password: this.password
            }
          }).then(() => {
            this.$router.replace({ path: '/' })
          }).catch ( ({ response }) => {

          // if (response.status === 422) {
          //   this.errors = response.data
          // }

          // if (response.status === 401) {
          //   this.$set(this.errors, 'error', response.data.message)
          // }
          })
        }
      })
    }
  }
}
</script>
