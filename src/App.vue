<script setup lang="ts">
import ProveedorFetcher from './components/ProveedorFetcher.vue'
</script>

<template>
  <main class="min-h-screen bg-gray-50 flex flex-col items-center py-10 px-4">
    <h1 class="text-3xl font-extrabold text-gray-800 mb-8 text-center">
      📦 Ejemplo: Fetch de Proveedores
    </h1>

    <!-- Contenedor del componente -->
    <ProveedorFetcher />
  </main>
</template>

<style scoped>
/* Tipografía y espaciado */
main {
  font-family: 'Inter', system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
}

h1 {
  line-height: 1.3;
}

/* Pequeños ajustes para el componente hijo si es necesario */
</style>
