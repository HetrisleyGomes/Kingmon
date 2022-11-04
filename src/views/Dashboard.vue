<template>
  <div style="overflow: hidden;">
    <Preparar/>
  </div>
</template>
  
<script>
import Preparar from '../components/Preparar.vue'
  export default {
    name: 'Dashboard',
    components: {
      Preparar
    }
  }
</script>

<style scoped>
div {
  overflow: hidden;
}

</style>