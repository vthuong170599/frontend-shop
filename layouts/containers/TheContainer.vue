<template>
  <div class="c-app">
    <TheSidebar v-if="$nuxt.$auth.loggedIn"/>
    <div class="c-wrapper">
      <TheHeader v-if="$nuxt.$auth.loggedIn"/>
      <div class="c-body">
        <main class="c-main">
          <CContainer fluid>
            <transition name="fade">
              <nuxt />
            </transition>
          </CContainer>
        </main>
      </div>
      <TheFooter v-if="$nuxt.$auth.loggedIn"/>
    </div>
  </div>
</template>

<script>
import TheSidebar from './TheSidebar'
import TheHeader from './TheHeader'
import TheFooter from './TheFooter'

export default {
  name: 'TheContainer',
  components: {
    TheSidebar,
    TheHeader,
    TheFooter
  }
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
