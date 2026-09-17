<template>
  <div class="add-contact-page">
    <div class="add-contact-card">
      <h2 class="page-heading">Thêm Liên hệ</h2>

      <div class="form-shell">
        <div class="form-title">ContactForm</div>

        <Form @submit="submitContact" :validation-schema="contactSchema">
          <div class="field-group">
            <label for="name">Tên</label>
            <Field id="name" name="name" type="text" v-model="contact.name" class="form-control" />
            <ErrorMessage name="name" class="error-feedback" />
          </div>

          <div class="field-group">
            <label for="email">E-mail</label>
            <Field id="email" name="email" type="email" v-model="contact.email" class="form-control" />
            <ErrorMessage name="email" class="error-feedback" />
          </div>

          <div class="field-group">
            <label for="address">Địa chỉ</label>
            <Field id="address" name="address" type="text" v-model="contact.address" class="form-control" />
            <ErrorMessage name="address" class="error-feedback" />
          </div>

          <div class="field-group">
            <label for="phone">Điện thoại</label>
            <Field id="phone" name="phone" type="tel" v-model="contact.phone" class="form-control" />
            <ErrorMessage name="phone" class="error-feedback" />
          </div>

          <div class="favorite-row">
            <Field name="favorite" type="checkbox" v-model="contact.favorite" class="form-check-input" />
            <label class="favorite-label">Liên hệ yêu thích</label>
          </div>

          <div class="actions">
            <button type="submit" class="btn-save">
              <i class="fas fa-save"></i>
              Lưu
            </button>
          </div>
        </Form>
      </div>
    </div>
  </div>
</template>

<script>
import { Form, Field, ErrorMessage } from "vee-validate";
import * as yup from "yup";
import ContactService from "@/services/contact.service";

export default {
  components: { Form, Field, ErrorMessage },

  data() {
    const contactSchema = yup.object().shape({
      name: yup
        .string()
        .required("Tên phải có giá trị.")
        .min(2, "Tên phải ít nhất 2 ký tự.")
        .max(50, "Tên tối đa 50 ký tự."),
      email: yup.string().email("E-mail không đúng.").max(50, "E-mail tối đa 50 ký tự."),
      address: yup.string().max(100, "Địa chỉ tối đa 100 ký tự."),
      phone: yup
        .string()
        .matches(/((09|03|07|08|05)+([0-9]{8})\b)/g, "Số điện thoại không hợp lệ."),
      favorite: yup.boolean(),
    });

    return {
      contact: {
        name: "",
        email: "",
        address: "",
        phone: "",
        favorite: false,
      },
      contactSchema,
    };
  },

  methods: {
    async submitContact() {
      try {
        await ContactService.create(this.contact);
        alert("Liên hệ được lưu thành công.");
        this.$router.push({ name: "contactbook" });
      } catch (error) {
        console.error(error);
        alert("Không thể lưu liên hệ. Vui lòng kiểm tra lại dữ liệu.");
      }
    },
  },
};
</script>

<style scoped>
.add-contact-page {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: 80vh;
  background: #f3f3f3;
}

.add-contact-card {
  width: 100%;
  max-width: 720px;
  padding-top: 12px;
}

.page-heading {
  text-align: center;
  font-size: 2.25rem;
  font-weight: 700;
  color: #1f1f1f;
  margin-bottom: 12px;
}

.form-shell {
  border: 3px solid #f00;
  background: #f5f5f5;
  padding: 8px 14px 18px;
  box-sizing: border-box;
}

.form-title {
  text-align: center;
  font-size: 2.3rem;
  font-weight: 700;
  color: #f13d3d;
  line-height: 1.2;
  margin: 12px 0 8px;
}

.field-group {
  margin: 10px auto;
  max-width: 640px;
}

.field-group label {
  display: block;
  font-size: 1.2rem;
  font-weight: 500;
  margin-bottom: 8px;
  color: #1c1c1c;
}

.form-control {
  display: block;
  width: 100%;
  height: 42px;
  border: 1px solid #d0d0d0;
  border-radius: 4px;
  padding: 0 12px;
  font-size: 1.1rem;
  background: #fff;
}

.form-control:focus {
  outline: none;
  border-color: #3b82f6;
  box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.15);
}

.favorite-row {
  display: flex;
  align-items: center;
  max-width: 640px;
  margin: 18px auto 14px;
  gap: 10px;
}

.form-check-input {
  width: 16px;
  height: 16px;
}

.favorite-label {
  font-size: 1.2rem;
  font-weight: 600;
  color: #1d1d1d;
  margin: 0;
}

.error-feedback {
  display: block;
  margin-top: 6px;
  color: red;
  font-size: 0.92rem;
}

.actions {
  max-width: 640px;
  margin: 12px auto 0;
}

.btn-save {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  min-width: 140px;
  min-height: 42px;
  border: none;
  background: #1e88e5;
  color: #fff;
  font-size: 1.1rem;
  font-weight: 600;
  border-radius: 4px;
  cursor: pointer;
  padding: 8px 18px;
}

.btn-save:hover {
  background: #1976d2;
}
</style>
