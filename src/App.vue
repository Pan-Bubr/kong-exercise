<template>
  <div id="app">
    <div class="nav">
      <router-link
        to="/"
        class="nav-item nav-item-wide"
      >
        <img
          class="logo"
          height="15px"
          :src="`${publicPath}assets/Konnect.svg`"
        >
      </router-link>
      <img
        class="nav-item"
        height="20px"
        width="20px"
        :src="`${publicPath}assets/Notifications.svg`"
      >
      <img
        class="nav-item"
        height="20px"
        width="20px"
        :src="`${publicPath}assets/Help.svg`"
      >
      <img
        class="user nav-item"
        height="32px"
        width="32px"
        :src="`${publicPath}assets/User.jpg`"
      >
    </div>
    <router-view />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'App',
  data () {
    return {
      publicPath: process.env.BASE_URL
    }
  }
})
</script>

<style lang="scss">
#app {
  font-family: Roboto, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.nav {
  padding: 14px 24px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 24px;

  &-item {
    display: block;

    &-wide {
      margin-right: auto;
    }
  }

  .logo {
    display: block;
  }

  .user {
    border-radius: 50%;
  }
}
</style>
