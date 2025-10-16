<script setup lang="ts">
import { ref, onMounted, defineProps } from 'vue'

type Proveedor = { id_proveedor: number; nombre: string }

const props = defineProps<{ url?: string }>()
const url = props.url ?? 'http://localhost:3000/proveedores'

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
      // If the API returned a single object, convert to array
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
  <div>
    <div v-if="loading">Cargando proveedores...</div>
    <div v-else-if="error">Error: {{ error }}</div>
    <ul v-else-if="proveedores.length">
      <li v-for="p in proveedores" :key="p.id_proveedor">{{ p.id_proveedor }} - {{ p.nombre }}</li>
    </ul>
    <div v-else>No hay proveedores.</div>
  </div>
</template>

<style scoped>
div { font-family: system-ui, -apple-system, 'Segoe UI', Roboto, Arial; }
ul { padding-left: 1.25rem; }
li { margin: 0.25rem 0; }
</style>

