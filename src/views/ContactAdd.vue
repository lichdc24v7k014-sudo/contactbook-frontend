<template>
  <div class="page">
    <h4><i class="fas fa-plus"></i> Thêm Liên hệ mới</h4>

    <ContactForm :contact="contact" @submit:contact="addContact" />

    <p class="mt-3 text-center">{{ message }}</p>
  </div>
</template>

<script>
import ContactForm from "../components/ContactForm.vue";
import ContactService from "../services/contact.service";

export default {
  components: {
    ContactForm,
  },
  data() {
    return {
      contact: {
        name: "",
        email: "",
        address: "",
        phone: "",
        favorite: false,
      },
      message: "",
    };
  },
  methods: {
    async addContact(data) {
      try {
        await ContactService.create(data);
        alert("✅ Thêm liên hệ mới thành công!");
        this.$router.push({ name: "contactbook" });
      } catch (error) {
        console.error(error);
        alert("❌ Có lỗi khi thêm liên hệ!");
      }
    },
  },
};
</script>

<style scoped>
.page {
  max-width: 600px;
  margin: auto;
}
</style>
