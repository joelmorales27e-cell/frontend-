<script setup lang="ts">
import { ref, onMounted, defineProps } from 'vue'

type Proveedor = { id_proveedor: number; nombre: string }

const props = defineProps<{ url?: string }>()
const url = props.url ?? 'https://backend-1-0pas.onrender.com/proveedores'

const proveedores = ref<Proveedor[]>([])
const loading = ref(true)
const error = ref<string | null>(null)

async function fetchProveedores() {
  loading.value = true
  error.value = null
  proveedores.value = []

  try {
    const res = await fetch(url)
    if (!res.ok) throw new Error(`HTTP ${res.status}`)
    const data = await res.json()

    if (Array.isArray(data)) {
      proveedores.value = data as Proveedor[]
    } else if (data && typeof data === 'object') {
      proveedores.value = [data as Proveedor]
    } else {
      proveedores.value = []
    }
  } catch (err: unknown) {
    error.value = err instanceof Error ? err.message : String(err)
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  void fetchProveedores()
})
</script>

<template>
  <div class="bg-white rounded-2xl shadow-lg border border-slate-200 overflow-hidden">
    <!-- Header -->
    <div class="bg-gradient-to-r from-blue-600 to-blue-700 px-6 py-4 flex items-center justify-between">
      <h2 class="text-xl font-semibold text-white">Lista de Proveedores</h2>
      <button 
        @click="fetchProveedores"
        class="px-4 py-2 bg-white/20 hover:bg-white/30 text-white rounded-lg transition-colors duration-200 text-sm font-medium"
      >
        Actualizar
      </button>
    </div>

    <!-- Loading State -->
    <div v-if="loading" class="px-6 py-12 text-center">
      <div class="inline-block animate-spin rounded-full h-12 w-12 border-4 border-blue-200 border-t-blue-600 mb-4"></div>
      <p class="text-slate-600 font-medium">Cargando proveedores...</p>
    </div>

    <!-- Error State -->
    <div v-else-if="error" class="px-6 py-12">
      <div class="bg-red-50 border border-red-200 rounded-xl p-6 text-center">
        <div class="text-red-600 text-4xl mb-3">⚠️</div>
        <h3 class="text-red-900 font-semibold text-lg mb-2">Error al cargar</h3>
        <p class="text-red-700">{{ error }}</p>
        <button 
          @click="fetchProveedores"
          class="mt-4 px-4 py-2 bg-red-600 hover:bg-red-700 text-white rounded-lg transition-colors duration-200 text-sm font-medium"
        >
          Reintentar
        </button>
      </div>
    </div>

    <!-- Proveedores List -->
    <div v-else-if="proveedores.length" class="divide-y divide-slate-100">
      <div 
        v-for="p in proveedores" 
        :key="p.id_proveedor"
        class="px-6 py-4 hover:bg-slate-50 transition-colors duration-150 flex items-center gap-4"
      >
        <div class="flex-shrink-0 w-12 h-12 bg-gradient-to-br from-blue-500 to-blue-600 rounded-xl flex items-center justify-center text-white font-bold shadow-md">
          {{ p.id_proveedor }}
        </div>
        <div class="flex-1">
          <h3 class="text-slate-900 font-semibold text-lg">{{ p.nombre }}</h3>
          <p class="text-slate-500 text-sm">ID: {{ p.id_proveedor }}</p>
        </div>
        <div class="flex-shrink-0">
          <button class="px-3 py-1.5 text-blue-600 hover:bg-blue-50 rounded-lg transition-colors duration-150 text-sm font-medium">
            Ver detalles
          </button>
        </div>
      </div>
    </div>

    <!-- Empty State -->
    <div v-else class="px-6 py-12 text-center">
      <div class="text-slate-300 text-6xl mb-4">📦</div>
      <h3 class="text-slate-700 font-semibold text-lg mb-2">No hay proveedores</h3>
      <p class="text-slate-500">No se encontraron proveedores en el sistema</p>
    </div>
  </div>
</template>

<style scoped>
/* Tailwind handles all styling */
</style>
