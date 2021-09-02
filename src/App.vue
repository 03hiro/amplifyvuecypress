<template>
  <div id="app">
    <div v-if="!signedIn">
      <amplify-authenticator></amplify-authenticator>
    </div>
    <div v-if="signedIn">
      <h3>Signed in!</h3>
      <amplify-sign-out></amplify-sign-out>
    </div>
  </div>
</template>

<script>
import { Auth } from 'aws-amplify'
import { AmplifyEventBus } from 'aws-amplify-vue'

export default {
  name: 'app',
  data () {
    return {
      signedIn: false
    }
  },
  async beforeCreate() {
    try {
      await Auth.currentAuthenticatedUser()
      this.signedIn = true
    } catch {
      this.signedIn = false
    }
    AmplifyEventBus.$on('authState', info => {
      this.signedIn = info === 'signedIn'
    }); 
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>