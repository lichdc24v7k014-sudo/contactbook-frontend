<template>
  <form @submit.prevent="submitContact" :validation-schema="contactFormSchema">
    <div class="form-group">
      <label>Tên <span class="text-danger">*</span></label>
      <Field
        name="name"
        type="text"
        class="form-control"
        v-model="contactLocal.name"
      />
      <ErrorMessage name="name" class="error-feedback" />
    </div>

    <div class="form-group">
      <label>E-mail</label>
      <Field
        name="email"
        type="email"
        class="form-control"
        v-model="contactLocal.email"
      />
      <ErrorMessage name="email" class="error-feedback" />
    </div>

    <div class="form-group">
      <label>Địa chỉ</label>
      <Field
        name="address"
        type="text"
        class="form-control"
        v-model="contactLocal.address"
      />
      <ErrorMessage name="address" class="error-feedback" />
    </div>

    <div class="form-group">
      <label>Điện thoại</label>
      <Field
        name="phone"
        type="tel"
        class="form-control"
        v-model="contactLocal.phone"
      />
      <ErrorMessage name="phone" class="error-feedback" />
    </div>

    <div class="form-group form-check mt-3">
      <input
        name="favorite"
        type="checkbox"
        class="form-check-input"
        v-model="contactLocal.favorite"
      />
      <label class="form-check-label">
        <strong>Liên hệ yêu thích</strong>
      </label>
    </div>

    <div class="form-group mt-4">
      <button type="submit" class="btn btn-primary me-2">Lưu</button>
      <button type="button" class="btn btn-secondary" @click="cancel">
        Thoát
      </button>
    </div>
  </form>
</template>

<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";

export default {
  components: { Form, Field, ErrorMessage },
  emits: ["submit:contact"],
  props: {
    contact: { type: Object, required: true },
  },
  data() {
    const contactFormSchema = yup.object().shape({
      name: yup.string().required("Tên phải có giá trị.").min(2).max(50),
      email: yup.string().email("E-mail không đúng.").max(50),
      address: yup.string().max(100),
      phone: yup
        .string()
        .matches(
          /((09|03|07|08|05)+([0-9]{8})\b)/,
          "Số điện thoại không hợp lệ.",
        ),
    });

    return {
      contactLocal: { ...this.contact },
      contactFormSchema,
    };
  },
  methods: {
    submitContact() {
      this.$emit("submit:contact", this.contactLocal);
    },
    cancel() {
      if (confirm("Bạn có chắc muốn thoát?")) {
        this.$router.push({ name: "contactbook" });
      }
    },
  },
};
</script>

<style scoped>
.error-feedback {
  color: red;
  font-size: 0.85rem;
}
</style>
