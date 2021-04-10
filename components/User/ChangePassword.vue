<template>
  <CCard>
    <CCardHeader>
      <h3>Change Password</h3>
    </CCardHeader>
    <CCardBody>
      <CRow>
        <CCol sm="12">
          <CInput
            type="password"
            label="Old Password"
            placeholder="Enter your old password"
            v-model="dataPassword.password"
          />
          <CInput
            type="password"
            label="New Password"
            placeholder="Enter your new password"
            v-model="dataPassword.newPassword"
          />
          <CInput
            type="password"
            label="Confirm Password"
            placeholder="Enter your confirm password"
            v-model="dataPassword.confirmPassword"
          />
          <CButton color="primary" @click="changePassword"
            >Change password</CButton
          >
        </CCol>
      </CRow>
    </CCardBody>
  </CCard>
</template>
<script>
export default {
  data() {
    return {
      dataPassword: {
        password: "",
        newPassword: "",
        confirmPassword: "",
      },
    };
  },
  methods: {
    changePassword() {
      this.$emit("changePassword", this.dataPassword);
    },
  },
};
</script>