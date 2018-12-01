<template>
  <el-row>
    <el-col :span="8">
      <el-button type="warning">Kick Up 4d3d3d3</el-button>
    </el-col>
    <el-col :span="8">
      <el-button type="primary" @click="addCelery">
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
    addCelery() {
      this.$emit("showModal");
    }
  }
};
</script>

<style>
</style>
