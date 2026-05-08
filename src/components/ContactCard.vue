<script>
export default {
  props: {
    contact: {
      type: Object,
      required: true,
    },
  },
};
</script>

<template>
  <div class="card">
    <div class="card-body">
      <h5 class="card-title">{{ contact.name }}</h5>

      <p class="card-text">
        <strong>Email:</strong> {{ contact.email || "(Chưa có)" }}<br />
        <strong>Địa chỉ:</strong> {{ contact.address || "(Chưa có)" }}<br />
        <strong>Điện thoại:</strong> {{ contact.phone || "(Chưa có)" }}<br />

        <!-- Nhóm liên hệ -->
        <strong>Nhóm liên hệ: </strong>
        <span v-if="contact.group === 'family'">Gia đình</span>
        <span v-else-if="contact.group === 'friend'">Bạn bè</span>
        <span v-else-if="contact.group === 'work'">Công việc</span>
        <span v-else class="text-muted">(Chưa chọn)</span><br />

        <!-- Liên hệ yêu thích -->
        <strong>Liên hệ yêu thích:</strong>
        <span v-if="contact.favorite" class="text-warning">
          <i class="fas fa-star"></i> Có
        </span>
        <span v-else class="text-muted">
          <i class="far fa-star"></i> Không
        </span>
      </p>
    </div>
  </div>
</template>

<style scoped>
.card {
  border-left: 5px solid #007bff;
}
.card-title {
  color: #2c3e50;
  font-weight: bold;
}
</style>
