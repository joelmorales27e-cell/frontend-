
<script setup lang="ts">
import ProveedorFetcher from './components/ProveedorFetcher.vue'
</script>

<template>
  <main>
    <h1>Ejemplo: Fetch de proveedor</h1>
    <!-- El componente acepta una prop `url`. Por defecto consulta `/api/proveedor` -->
    <ProveedorFetcher />
  </main>
</template>

<style scoped>
main {
  font-family: system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;
  padding: 2rem;
  max-width: 640px;
  margin: 0 auto;
}

h1 {
  margin-bottom: 1rem;
}
</style>
