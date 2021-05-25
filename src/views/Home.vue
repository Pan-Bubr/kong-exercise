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
  max-width: 1000px;
  margin: 0 auto;

  @media (max-width: 1000px) {
    padding: 20px;
  }

  @media (max-width: 500px) {
    padding: 10px;
  }
}
</style>
