<script setup lang="ts">
import ProveedorFetcher from './components/ProveedorFetcher.vue'
</script>

<template>
  <main class="min-h-screen bg-gradient-to-br from-slate-50 to-slate-100">
    <div class="container mx-auto px-4 py-12 max-w-4xl">
      <div class="text-center mb-12">
        <h1 class="text-4xl font-bold text-slate-900 mb-3 text-balance">
          Gestión de Proveedores
        </h1>
        <p class="text-slate-600 text-lg">
          Consulta y administra tu lista de proveedores
        </p>
      </div>
      
      <ProveedorFetcher />
    </div>
  </main>
</template>

<style scoped>
/* Tailwind handles all styling */
</style>
