<template>
  <CDropdown
    in-nav
    class="c-header-nav-items"
    placement="bottom-end"
    add-menu-classes="pt-0"
  >
    <template #toggler>
      <CHeaderNavLink>
        <div class="c-avatar">
          <CIcon :content="$options.freeSet.cilUser" />
        </div>
      </CHeaderNavLink>
    </template>
    <CDropdownHeader tag="div" class="text-center" color="light">
      <strong>Account</strong>
    </CDropdownHeader>
    <CDropdownItem>
      <CIcon :content="$options.freeSet.cilBell" /> 
      <nuxt-link to="/user/update">Updates</nuxt-link>
    </CDropdownItem>
    <CDropdownItem>
      <CIcon :content="$options.freeSet.cilUser" />
      <nuxt-link to="/user"> Profile</nuxt-link></CDropdownItem
    >
    <CDropdownItem>
      <CIcon :content="$options.freeSet.cilLockLocked" />
      <CButton @click="logout"> Logout</CButton>
    </CDropdownItem>
  </CDropdown>
</template>

<script>
import { freeSet } from "@coreui/icons";
export default {
  name: "TheHeaderDropdownAccnt",
  freeSet,
  data() {
    return {
      itemsCount: 42,
    };
  },
  methods: {
    logout() {
      this.$auth.logout();
      this.$router.push("/login");
    },
  },
};
</script>

<style scoped>
.c-icon {
  margin-right: 0.3rem;
}
</style>
