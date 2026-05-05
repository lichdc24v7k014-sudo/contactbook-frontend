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
        <strong>Email:</strong> {{ contact.email }}<br />
        <strong>Địa chỉ:</strong> {{ contact.address }}<br />
        <strong>Điện thoại:</strong> {{ contact.phone }}<br />
        <strong>Liên hệ yêu thích:</strong>
        <i v-if="contact.favorite" class="fas fa-check text-success"></i>
        <i v-else class="fas fa-times text-danger"></i>
      </p>
    </div>
  </div>
</template>
