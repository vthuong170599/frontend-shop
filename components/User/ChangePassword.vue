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
           <p v-if="Object.keys(error).length > 0" class="text text-danger">
            {{ error.password }}
          </p>
          <CInput
            type="password"
            label="New Password"
            placeholder="Enter your new password"
            v-model="dataPassword.newPassword"
          />
           <p v-if="Object.keys(error).length > 0" class="text text-danger">
            {{ error.newPassword }}
          </p>
          <CInput
            type="password"
            label="Confirm Password"
            placeholder="Enter your confirm password"
            v-model="dataPassword.confirmPassword"
          />
           <p v-if="Object.keys(error).length > 0" class="text text-danger">
            {{ error.confirmPassword }}
          </p>
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
      error: {}
    };
  },
  methods: {
    changePassword() {
      this.validate();
      if(Object.keys(this.error).length>0){
        return this.error;
      }
      this.$emit("changePassword", this.dataPassword);
    },
    
    validate(){
      if(this.dataPassword.password == ""){
        this.error.password = "password khong duoc de trong";
      }else if(this.dataPassword.newPassword == "" ){
        this.error.newPassword = "new password khong duoc de trong";
      }else if(this.dataPassword.newPassword != this.dataPassword.confirmPassword){
        this.error.confirmPassword = "confirm password khong dung";
      }
    }
  },
};
</script>