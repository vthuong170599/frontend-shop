<template>
  <CContainer class="min-vh-100 d-flex align-items-center">
    <CRow class="w-100 justify-content-center">
      <CCol md="6" sm="8">
        <CCard class="mx-4 mb-0">
          <CCardBody class="p-4">
            <CForm>
              <h1>Register</h1>
              <p class="text-muted">Create your account</p>
              <CInput
                placeholder="Username"
                autocomplete="username"
                v-model="formRegister.name"
              >
                <template #prepend-content>
                  <CIcon :content="$options.freeSet.cilUser" />
                </template>
              </CInput>
              <p v-if="Object.keys(errors).length > 0" class="text text-danger">
                {{ errors.name }}
              </p>
              <CInput
                placeholder="Email"
                autocomplete="email"
                prepend="@"
                v-model="formRegister.email"
              />
              <p v-if="Object.keys(errors).length > 0" class="text text-danger">
                {{ errors.email }}
              </p>
              <CInput
                placeholder="Password"
                type="password"
                autocomplete="new-password"
                v-model="formRegister.password"
              >
                <template #prepend-content>
                  <CIcon :content="$options.freeSet.cilLockLocked" />
                </template>
              </CInput>
              <p v-if="Object.keys(errors).length > 0" class="text text-danger">
                {{ errors.password }}
              </p>
              <CInput
                placeholder="Repeat password"
                type="password"
                class="mb-4"
                v-model="formRegister.confirmPassword"
              >
                <template #prepend-content>
                  <CIcon :content="$options.freeSet.cilLockLocked" />
                </template>
              </CInput>
              <p v-if="Object.keys(errors).length > 0" class="text text-danger">
                {{ errors.password_confirmation }}
              </p>
              <CButton color="success" block @click="register">
                Create Account
              </CButton>
            </CForm>
          </CCardBody>
        </CCard>
      </CCol>
    </CRow>
  </CContainer>
</template>
<script>
import { freeSet } from "@coreui/icons";
import axios from "axios";
import { URL } from "../constant/constant";
export default {
  auth: false,
  freeSet,
  data() {
    return {
      formRegister: {
        name: "",
        email: "",
        password: "",
        confirmPassword: "",
      },
      errors: {},
    };
  },
  methods: {
    /**
     * register
     */
    register() {
      this.validate();
      if (Object.keys(this.errors).length > 0) {
        return this.errors;
      } else {
        axios.post(URL + "auth/signup", this.formRegister).then((res) => {
          this.$router.push("/login");
        });
      }
    },

    /**
     * check validate
     */
    validate() {
      this.errors = {};
      if (this.formRegister.name == "") {
        this.errors.name = "Name không được trống";
      }
      if (this.formRegister.password == "") {
        this.errors.password = "Password không được trống";
      }
      if (this.formRegister.password !== this.formRegister.password_confirmation) {
        this.errors.password_confirmation = "Password không giống";
      }
      if (this.formRegister.email == "") {
        this.errors.email = "Email không được trống";
      }
    },
  },
};
</script>