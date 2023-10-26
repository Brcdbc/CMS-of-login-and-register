<template>
    <dev>
        <el-result icon="warning" title="404" sub-title="抱歉，功能正在开发中，请使用本系统的的其它功能">
        </el-result>

        <el-button class="welcome-button" type="primary" @click="goBack">返回系统</el-button>

    </dev>
</template>
<script>
export default {
  methods: {
    goBack() {
      // 使用 Vue Router 的 $router.back() 方法返回到上一个页面
      this.$router.back();
    },
  },
};
</script>
<style scoped>
.welcome-button {
    width: 150px;
    height: 40px;
    font-size: 20px;
    padding: 0;
    /* 或使用 margin: 0; */
}
</style>
