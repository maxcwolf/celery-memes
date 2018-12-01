<template>
  <div>
    <el-dialog
      title="Celery Man Fan Fiction"
      :visible.sync="this.celeryVisible"
      width="80%"
      :before-close="closeCelery"
    >
      <img src="@/assets/hat-wobble.png" width="20px" height="20px">
      <span>Write Some Celery Man Fan Fiction</span>
      <img src="@/assets/hat-wobble.png" width="20px" height="20px">

      <el-form ref="form">
        <div class="form">
          <input class="input" v-model="name" placeholder="Celery Name">
          <input class="input" v-model="description" placeholder="Celery Description">
          <button class="button" v-on:click="createCelery">Write Celery</button>
        </div>
        <div v-for="item in celeryFics" :key="item.key" class="list-item">
          <p class="name">{{ item.name }}</p>
          <p class="description">{{ item.description }}</p>
        </div>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
const GetCelery = `
  query {
    getCelery {
      items {
        id name description
      }
    }
  }
`;
const CreateCelery = `
  mutation($name: String! $description: String) {
    createCelery(input: {
      name: $name description: $description
    }) {
      id name description
    }
  }
`;
export default {
  name: "WriteCelery",
  props: {
    signedIn: Boolean,
    celeryVisible: Boolean
  },
  data() {
    return {
      name: "",
      description: "",
      celeryFics: []
    };
  },
  async beforeCreate() {
    const {
      data: {
        getCelery: { items }
      }
    } = await API.graphql(graphqlOperation(GetCelery));
    this.celeryFics = items;
    try {
      const user = await Auth.currentAuthenticatedUser();
      this.signedIn = true;
    } catch (err) {
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
  methods: {
    closeCelery() {
      this.$emit("closeCelery");
    },
    async createCelery() {
      if (this.name === "" || this.description === "") {
        return;
      }
      const celeryFic = { name: this.name, description: this.description };
      const celeryFics = [...this.celeryFics, celeryFic];
      this.celeryFics = celeryFics;
      this.name = "";
      this.description = "";
      try {
        await API.graphql(graphqlOperation(CreateCelery, celeryFic));
        console.log("new Celery fan fic successfully created!");
      } catch (err) {
        console.log("error adding item: ", err);
      }
    }
  }
};
</script>

<style>
</style>
