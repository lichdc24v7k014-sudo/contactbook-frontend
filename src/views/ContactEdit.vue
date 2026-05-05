<template>
  <div class="page" v-if="contact">
    <h4><i class="fas fa-edit"></i> Hiệu chỉnh Liên hệ</h4>

    <ContactForm
      :contact="contact"
      @submit:contact="updateContact"
      @delete:contact="deleteContact"
    />

    <p class="mt-3">{{ message }}</p>
  </div>
  <div v-else class="text-center mt-5">
    <p>Đang tải...</p>
  </div>
</template>

<script>
import ContactForm from "../components/ContactForm.vue";
import ContactService from "../services/contact.service";

export default {
  components: {
    ContactForm,
  },
  props: {
    id: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      contact: null,
      message: "",
    };
  },
  methods: {
    async getContact(id) {
      try {
        this.contact = await ContactService.get(id);
      } catch (error) {
        console.error(error);
        this.message = "Không tìm thấy liên hệ";
      }
    },
    async updateContact(data) {
      try {
        await ContactService.update(this.contact._id, data);
        alert("Cập nhật liên hệ thành công!");
        this.$router.push({ name: "contactbook" });
      } catch (error) {
        console.error(error);
        alert("Cập nhật thất bại!");
      }
    },
    async deleteContact() {
      if (confirm("Bạn có chắc muốn xóa liên hệ này?")) {
        try {
          await ContactService.delete(this.contact._id);
          alert("Xóa liên hệ thành công!");
          this.$router.push({ name: "contactbook" });
        } catch (error) {
          console.error(error);
        }
      }
    },
  },
  created() {
    this.getContact(this.id);
  },
};
</script>

<style scoped>
.page {
  max-width: 600px;
  margin: auto;
}
</style>
