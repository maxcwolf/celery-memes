<template>
  <div id="app">
    <!-- <img src="./assets/celeryman-warning-nsfw.png"> -->
    <div v-if="!signedIn">
      <HeaderTitle msg="CELERY MEMES"/>
      <amplify-authenticator></amplify-authenticator>
    </div>
    <div v-if="signedIn">
      <nav-bar></nav-bar>
      <HeaderTitle msg="CELERY MEMES"/>
      <div class="container">
        <button-bar @showModal="showModal" @showCelery="showModal(true)"/>
        <amplify-s3-album path="images/"></amplify-s3-album>
        <upload-image
          @closeModal="closeModal"
          v-show="dialogVisible"
          :config="photoPickerConfig"
          :dialogVisible="dialogVisible"
        ></upload-image>
        <write-celery
          @closeCelery="closeModal(true)"
          v-show="celeryVisible"
          :signedIn="signedIn"
          :celeryVisible="celeryVisible"
        ></write-celery>
      </div>
    </div>
  </div>
</template>

<script>
import HeaderTitle from "./components/HeaderTitle";
import UploadImage from "./components/UploadImage";
import NavBar from "./components/NavBar";
import ButtonBar from "./components/ButtonBar";
import WriteCelery from "./components/WriteCelery";
import { AmplifyEventBus } from "aws-amplify-vue";
import { Auth } from "aws-amplify";

const photoPickerConfig = {
  header: "",
  path: "images/"
};

export default {
  name: "app",
  components: {
    HeaderTitle,
    UploadImage,
    NavBar,
    ButtonBar,
    WriteCelery
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
      dialogVisible: false,
      celeryVisible: false,
      photoPickerConfig,
      signedIn: false,
      albumShown: false
    };
  },
  methods: {
    showModal(celery) {
      celery ? (this.celeryVisible = true) : (this.dialogVisible = true);
    },
    closeModal(celery) {
      celery ? (this.celeryVisible = false) : (this.dialogVisible = false);
    },
    showAlbum() {
      console.log(this.dialogVisible);
      this.showModal();
      if (this.albumShown) {
        this.albumShown = false;
      } else {
        this.albumShown = true;
      }
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
