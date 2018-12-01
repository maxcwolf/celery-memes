<template>
  <el-row>
    <el-col :span="8">
      <el-button type="warning" @click="addCelery">Write Fan Fiction</el-button>
    </el-col>
    <el-col :span="8">
      <el-button type="primary" @click="addMeme">
        Add New Celery Meme
        <i class="el-icon-upload el-icon-right"></i>
      </el-button>
    </el-col>
    <el-col :span="8">
      <el-button type="danger">Generate Nude Tayne?</el-button>
    </el-col>
  </el-row>
</template>

<script>
export default {
  name: "ButtonBar",
  methods: {
    addMeme() {
      this.$emit("showModal");
    },
    addCelery() {
      this.$emit("showCelery");
    }
  }
};
</script>

<style>
</style>
