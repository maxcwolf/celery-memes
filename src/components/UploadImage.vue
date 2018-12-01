<template>
  <div>
    <el-dialog
      title="Upload Fresh Celery"
      :visible.sync="this.dialogVisible"
      width="60%"
      :before-close="closeModal"
    >
      <span>Bring on the 4d3d3d3!</span>
      <amplify-photo-picker v-bind:photoPickerConfig="this.config"></amplify-photo-picker>
      <span slot="footer" class="dialog-footer"></span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  props: { config: Object, dialogVisible: Boolean },
  data() {
    return {};
  },
  methods: {
    closeModal() {
      this.$emit("closeModal");
    }
  }
};
</script>

<style>
</style>
