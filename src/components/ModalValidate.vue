<template>
  <modal title="Modal with form + Validate" @close="$emit('close')">
    <!-- body -->
    <div slot="body">
      <form @submit.prevent="onSubmit">
        <!-- name -->
        <div class="form-item" :class="{ errorInput: $v.name.$error }">
          <label>Name:</label>
          <p class="errorText" v-if="!$v.name.required">Filed is required!</p>
          <p class="errorText" v-if="!$v.name.minLength">
            Name must have at least {{ $v.name.$params.minLength.min }} !
          </p>
          <input
            v-model="name"
            :class="{ error: $v.name.$error }"
            @change="$v.name.$touch()"
          />
        </div>

        <!-- email -->
        <div class="form-item" :class="{ errorInput: $v.email.$error }">
          <label>Email:</label>
          <p class="errorText" v-if="!$v.email.required">Filed is required!</p>
          <p class="errorText" v-if="!$v.email.email">Email is not correct!</p>
          <input
            v-model="email"
            :class="{ error: $v.email.$error }"
            @change="$v.email.$touch()"
          />
        </div>

        <!-- password -->
        <div class="form-item" :class="{ errorInput: $v.password.$error }">
          <label>Password:</label>
          <p class="errorText" v-if="!$v.password.required">
            Filed is required!
          </p>
          <p class="errorText" v-if="!$v.password.minLength">
            Password must have at least
            {{ $v.password.$params.minLength.min }} !
          </p>
          <input type="password"
            v-model="password"
            :class="{ error: $v.password.$error }"
            @change="$v.password.$touch()"
          />
        </div>

                <!-- confirm password -->
        <div class="form-item" :class="{ errorInput: $v.confirmPassword.$error }">
          <label>Confirm password:</label>
          <p class="errorText" v-if="!$v.confirmPassword.required">
            Filed is required!
          </p>
          <p class="errorText" v-if="!$v.confirmPassword.sameAs">
            Password mismatch
          </p>
          <input type="password"
            v-model="confirmPassword"
            :class="{ error: $v.confirmPassword.$error }"
            @change="$v.confirmPassword.$touch()"
          />
        </div>

        <!-- button -->
        <button class="btn btnPrimary">Submit!</button>
      </form>
    </div>
  </modal>
</template>

<script>
import { required, minLength, email, sameAs } from "vuelidate/lib/validators";
import modal from "@/components/UI/Modal.vue";

export default {
  components: { modal },
  data() {
    return {
      name: "",
      email: "",
      password: "",
      confirmPassword: ""
    };
  },
  validations: {
    name: {
      required,
      minLength: minLength(4),
    },
    email: {
      required,
      email,
    },
    password: {
      required,
      minLength: minLength(6),
    },
    confirmPassword:{
      required,
      sameAs: sameAs ('password')

    }
  },
  methods: {
    onSubmit() {
      this.$v.$touch(); //запускаем проверку всей формы
      if (!this.$v.$invalid) {
        // если форма валидная, то в консоль выводится обьект user
        const user = {
          name: this.name,
          email: this.email,
        };
        console.log(user);

        // DONE!
        this.name = "";
        this.email = "";
        this.password = "";
        this.confirmPassword = "";
        this.$v.$reset();
        this.$emit("close");
      }
    },
  },
};
</script>

<style lang="scss">
.form-item .errorText {
  display: none;
  margin-bottom: 8px;
  font-size: 13.4px;
  color: #de4040;
}
.form-item {
  &.errorInput .errorText {
    display: block;
  }
}
input.error {
  border-color: #de4040;
}
</style>
