<template>
  <div class="hello">
    <h1>
      <img src="@/assets/hat-wobble.png" width="35px" height="35px">
      {{ msg }}
      <img src="@/assets/hat-wobble.png" width="35px" height="35px">
    </h1>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
