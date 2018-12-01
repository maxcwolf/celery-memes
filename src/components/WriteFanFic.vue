<template>
  <el-form ref="form">
    <div class="form">
      <input class="input" v-model="name" placeholder="Coffee Shop Name">
      <input class="input" v-model="description" placeholder="Coffee Shop Description">
      <button class="button" v-on:click="createCoffeeShop">Create Coffee Shop</button>
    </div>
    <div v-for="item in coffeeShops" :key="item.key" class="list-item">
      <p class="name">{{ item.name }}</p>
      <p class="description">{{ item.description }}</p>
    </div>
  </el-form>
</template>

<script>
const ListCoffeeShops = `
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
  name: "app",
  data() {
    return {
      name: "",
      description: "",
      coffeeShops: [],
      signedIn: false
    };
  },
  async beforeCreate() {
    const {
      data: {
        getCelery: { items }
      }
    } = await API.graphql(graphqlOperation(ListCoffeeShops));
    this.coffeeShops = items;
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
    async createCoffeeShop() {
      if (this.name === "" || this.description === "") {
        return;
      }
      const coffeeShop = { name: this.name, description: this.description };
      const coffeeShops = [...this.coffeeShops, coffeeShop];
      this.coffeeShops = coffeeShops;
      this.name = "";
      this.description = "";
      try {
        await API.graphql(graphqlOperation(CreateCelery, coffeeShop));
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
