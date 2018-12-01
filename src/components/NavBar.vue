<template>
  <el-menu
    class="el-menu-demo"
    mode="horizontal"
    background-color="#545c64"
    text-color="#fff"
    active-text-color="#ffd04b"
  >
    <el-row :gutter="20">
      <el-col :span="2" :offset="18">
        <el-menu-item index="1">
          <amplify-sign-out id="header-signout"></amplify-sign-out>
        </el-menu-item>
      </el-col>
    </el-row>
  </el-menu>
</template>

<script>
export default {
  name: "NavBar"
};
</script>

<style>
#header-signout {
  line-height: 30px;
  margin: 0;
  padding: 0;
}
</style>
