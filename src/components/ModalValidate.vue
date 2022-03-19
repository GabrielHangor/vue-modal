<template>
  <Modal title="Modal with validation" @close="closeModal">
    <div slot="body">
      <form @submit.prevent="onSubmit">
        <div class="form-item" :class="{ errorInput: $v.name.$error }">
          <label>Name:</label>
          <p class="errorText" v-if="!$v.name.required">Field is required!</p>
          <p class="errorText" v-if="!$v.name.minLength">
            Name should contain at least
            {{ $v.name.$params.minLength.min }} symbols!
          </p>
          <input
            v-model="name"
            :class="{ error: $v.name.$error }"
            @change="$v.name.$touch()"
          />
        </div>

        <div class="form-item" :class="{ errorInput: $v.email.$error }">
          <label>Email:</label>
          <p class="errorText" v-if="!$v.email.required">Field is required!</p>
          <p class="errorText" v-if="!$v.email.email">
            Please use the correct email format!
          </p>
          <input
            v-model="email"
            :class="{ error: $v.email.$error }"
            @change="$v.email.$touch()"
          />
        </div>

        <div class="form-item" :class="{ errorInput: $v.password.$error }">
          <label>Password:</label>
          <p class="errorText" v-if="!$v.password.required">
            Field is required!
          </p>
          <p class="errorText" v-if="!$v.password.minLength">
            Password should contain at least
            {{ $v.password.$params.minLength.min }} symbols!
          </p>
          <input
            v-model="password"
            :class="{ error: $v.password.$error }"
            @change="$v.password.$touch()"
          />
        </div>

        <div
          class="form-item"
          :class="{ errorInput: $v.confirmedPassword.$error }"
        >
          <label>Password:</label>
          <p class="errorText" v-if="!$v.confirmedPassword.required">
            Field is required!
          </p>
          <p class="errorText" v-if="!$v.confirmedPassword.sameAs">
            Passwords should match!
          </p>
          <input
            v-model="confirmedPassword"
            :class="{ error: $v.confirmedPassword.$error }"
            @change="$v.confirmedPassword.$touch()"
          />
        </div>

        <button class="btn btnPrimary">Submit</button>
      </form>
    </div>
  </Modal>
</template>

<script>
import { required, minLength, email, sameAs } from "vuelidate/lib/validators";
import Modal from "@/components/UI/Modal";

export default {
  name: "ModalValidate.vue",
  components: { Modal },

  data() {
    return { name: "", email: "", password: "", confirmedPassword: "" };
  },
  methods: {
    closeModal() {
      this.name = "";
      this.email = "";
      this.password = "";
      this.confirmedPassword = "";
      this.$v.$reset();
      this.$emit("close");
    },

    onSubmit() {
      this.$v.$touch();

      if (!this.$v.$invalid) {
        const user = {
          name: this.name,
          email: this.email,
          password: this.password,
        };
        console.log(user);

        this.closeModal();
      }
    },
  },
  validations: {
    name: { required, minLength: minLength(4) },
    email: { required, email },
    password: { required, minLength: minLength(6) },
    confirmedPassword: { required, sameAs: sameAs('password') },
  },
};
</script>

<style lang="scss" scoped>
.form-item .errorText {
  display: none;
  margin-bottom: 0.5rem;
  font-size: 0.9rem;
  color: red;
  font-weight: bold;
}

.form-item {
  &.errorInput .errorText {
    display: block;
  }
}

input.error {
  border-color: red;
}

form button {
  &:disabled {
    background: grey;
  }
}
</style>
