<template>
  <div class="page text-center mt-5">
    <h1 class="display-1 text-danger">404</h1>
    <h3>Oops! Không thể tìm thấy trang</h3>
    <p class="lead">Trang bạn đang tìm không tồn tại hoặc đã bị di chuyển.</p>
    <router-link to="/" class="btn btn-primary btn-lg">
      <i class="fas fa-home"></i> Về Trang Chủ
    </router-link>
  </div>
</template>

<script>
export default {
  name: "NotFound",
};
</script>

<style scoped>
.page {
  max-width: 600px;
  margin: 100px auto;
}
</style>
