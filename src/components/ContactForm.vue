<template>
  <Form
    @submit="submitContact"
    :validation-schema="contactFormSchema"
    class="contact-form"
  >
    <div class="form-group">
      <label for="name">Tên <span class="text-danger">*</span></label>
      <Field
        name="name"
        type="text"
        class="form-control"
        v-model="contactLocal.name"
      />
      <ErrorMessage name="name" class="error-feedback" />
    </div>

    <div class="form-group">
      <label for="email">E-mail</label>
      <Field
        name="email"
        type="email"
        class="form-control"
        v-model="contactLocal.email"
      />
      <ErrorMessage name="email" class="error-feedback" />
    </div>

    <div class="form-group">
      <label for="address">Địa chỉ</label>
      <Field
        name="address"
        type="text"
        class="form-control"
        v-model="contactLocal.address"
      />
      <ErrorMessage name="address" class="error-feedback" />
    </div>

    <div class="form-group">
      <label for="phone">Điện thoại</label>
      <Field
        name="phone"
        type="tel"
        class="form-control"
        v-model="contactLocal.phone"
      />
      <ErrorMessage name="phone" class="error-feedback" />
    </div>

    <!-- Checkbox -->
    <div class="form-group form-check">
      <Field
        name="favorite"
        type="checkbox"
        class="form-check-input"
        v-model="contactLocal.favorite"
      />
      <label class="form-check-label"
        ><strong>⭐ Liên hệ yêu thích</strong></label
      >
    </div>

    <!-- Radio - Nhóm liên hệ -->
    <div class="form-group">
      <label>Nhóm liên hệ:</label><br />
      <div class="form-check form-check-inline">
        <Field
          name="group"
          type="radio"
          class="form-check-input"
          value="family"
          v-model="contactLocal.group"
        />
        <label class="form-check-label">Gia đình</label>
      </div>
      <div class="form-check form-check-inline">
        <Field
          name="group"
          type="radio"
          class="form-check-input"
          value="friend"
          v-model="contactLocal.group"
        />
        <label class="form-check-label">Bạn bè</label>
      </div>
      <div class="form-check form-check-inline">
        <Field
          name="group"
          type="radio"
          class="form-check-input"
          value="work"
          v-model="contactLocal.group"
        />
        <label class="form-check-label">Công việc</label>
      </div>
    </div>

    <div class="form-group mt-4">
      <button type="submit" class="btn btn-primary me-2">Lưu</button>
      <button
        v-if="contactLocal._id"
        type="button"
        class="btn btn-danger me-2"
        @click="deleteContact"
      >
        Xóa
      </button>
      <button type="button" class="btn btn-secondary" @click="cancel">
        Thoát
      </button>
    </div>
  </Form>
</template>

<script>
import { Form, Field, ErrorMessage } from "vee-validate";
import * as yup from "yup";

export default {
  components: { Form, Field, ErrorMessage },
  emits: ["submit:contact", "delete:contact"],
  props: {
    contact: { type: Object, required: true },
  },
  data() {
    const contactFormSchema = yup.object().shape({
      name: yup.string().required("Tên phải có giá trị.").min(2).max(50),
      email: yup.string().email("Email không hợp lệ.").max(50),
      address: yup.string().max(100),
      phone: yup
        .string()
        .matches(
          /((09|03|07|08|05)+([0-9]{8})\b)/g,
          "Số điện thoại không hợp lệ.",
        ),
      group: yup.string(),
    });

    return {
      contactLocal: {
        ...this.contact,
        group: this.contact.group || "friend",
      },
      contactFormSchema,
    };
  },
  methods: {
    submitContact() {
      this.$emit("submit:contact", this.contactLocal);
    },
    deleteContact() {
      this.$emit("delete:contact", this.contactLocal._id);
    },
    cancel() {
      if (confirm("Bạn có thay đổi chưa lưu. Thoát thật không?")) {
        this.$router.push({ name: "contactbook" });
      }
    },
  },
};
</script>

<style scoped>
.error-feedback {
  color: red;
  font-size: 0.85em;
}
</style>
