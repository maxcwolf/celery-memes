<template>
  <div id="app">
    <img src="./assets/logo.png" />
    <HelloWorld msg="Welcome to My Shitty Vue-Amplify Test App" />
    <div v-if="!signedIn"><amplify-authenticator></amplify-authenticator></div>
    <div v-if="signedIn"><amplify-sign-out></amplify-sign-out></div>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import { AmplifyEventBus } from "aws-amplify-vue";
import { Auth } from "aws-amplify";
export default {
  name: "app",
  components: {
    HelloWorld
  },
  async beforeCreate() {
    try {
      const user = await Auth.currentAuthenticatedUser();
      this.signedIn = true;
    } catch {
      this.signedIn = false;
    }
    AmplifyEventBus.$on("authState", info => {
      if (info === "signedIn") {
        this.signedIn = true;
      } else {
        this.signedIn = false;
      }
    });
  },
  data() {
    return {
      signedIn: false,
      fuckLinter: user
    };
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
