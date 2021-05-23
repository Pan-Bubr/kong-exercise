<template>
  <div class="container">
    <Catalog />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import Catalog from '../components/Catalog.vue'

export default Vue.extend({
  name: 'Home',
  components: {
    Catalog
  }
})
</script>

<style lang="scss">
.container {
  max-width: 942px;
  margin: 0 auto;
}
</style>
