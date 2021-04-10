<template>
 <CCard>
    <CCardHeader>
      <h3>Profile {{user.name}}</h3>
    </CCardHeader>
    <CCardBody>
        <CRow>
          <CCol sm="1"><h5>Name</h5></CCol>
          <CCol sm="6"><h5>{{user.name}}</h5></CCol>
        </CRow>
        <CRow>
          <CCol sm="1"><h5>Email</h5></CCol>
          <CCol sm="6"><h5>{{user.email}}</h5></CCol>
        </CRow>
    </CCardBody>
  </CCard>
</template>
<script>
export default {
  props: {
    user: {
      type: Object,
      default: () => {},
    },
  },
};
</script>