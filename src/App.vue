<template>
  <div id="app">
    <div class="banner"></div>
    <!-- <img src="./assets/celeryman-warning-nsfw.png"> -->
    <HelloWorld msg="CELERY MEMES"/>
    <div v-if="!signedIn">
      <amplify-authenticator></amplify-authenticator>
    </div>
    <div v-if="signedIn">
      <amplify-sign-out class="signout"></amplify-sign-out>
      <div class="container">
        <el-button type="primary" @click="showAlbum()">Add New Celery Meme</el-button>
        <amplify-s3-album path="images/"></amplify-s3-album>
        <amplify-photo-picker v-if="albumShown" v-bind:photoPickerConfig="photoPickerConfig"></amplify-photo-picker>
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import { AmplifyEventBus } from "aws-amplify-vue";
import { Auth } from "aws-amplify";

const photoPickerConfig = {
  path: "images/"
};

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
      photoPickerConfig,
      signedIn: false,
      albumShown: true
    };
  },
  methods: {
    showAlbum() {
      albumShown ? (albumShown = false) : (albumShown = true);
    }
  }
};
</script>

<style>
body {
  margin: 0;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.container {
  padding: 40px;
}
.signout {
  background-color: #ededed;
  margin: 0;
  padding: 11px 0px 1px;
}
.banner {
  width: 100%;
  background-image: url(./assets/tayne-triple.jpg);
  height: 250px;
  background-color: purple;
  background-position: top;
}
</style>
