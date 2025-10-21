<script setup lang="ts">
import { ref, onMounted, defineProps } from 'vue'

type Proveedor = { id_proveedor: number; nombre: string }

const props = defineProps<{ url?: string }>()
const url = props.url ?? 'https://backend-1-0pas.onrender.com/proveedores'

const proveedores = ref<Proveedor[]>([])
const loading = ref(true)
const error = ref<string | null>(null)
const nuevoNombre = ref('')

// 📦 Obtener proveedores
async function fetchProveedores() {
  loading.value = true
  error.value = null
  proveedores.value = []

  try {
    const res = await fetch(url)
    if (!res.ok) throw new Error(`HTTP ${res.status}`)
    const data = await res.json()

    if (Array.isArray(data)) {
      proveedores.value = data
    } else if (data && typeof data === 'object') {
      proveedores.value = [data]
    }
  } catch (err: unknown) {
    error.value = err instanceof Error ? err.message : String(err)
  } finally {
    loading.value = false
  }
}

// 🧾 Agregar proveedor
async function agregarProveedor() {
  const nombre = nuevoNombre.value.trim()
  if (!nombre) return alert('Por favor, escribe un nombre.')

  try {
    const res = await fetch(url, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ nombre })
    })

    if (!res.ok) throw new Error('Error al agregar proveedor')

    const creado = await res.json()
    proveedores.value.push(creado)
    nuevoNombre.value = ''
  } catch (err: unknown) {
    error.value = err instanceof Error ? err.message : String(err)
  }
}

onMounted(fetchProveedores)
</script>

<template>
  <div class="p-6 bg-white rounded-xl shadow-md">
    <h2 class="text-xl font-bold mb-4">Lista de proveedores</h2>

    <!-- Formulario -->
    <form @submit.prevent="agregarProveedor" class="flex gap-2 mb-4">
      <input
        v-model="nuevoNombre"
        type="text"
        placeholder="Nombre del proveedor"
        class="flex-1 border border-gray-300 rounded-lg p-2"
      />
      <button
        type="submit"
        class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition"
      >
        Agregar
      </button>
    </form>

    <!-- Estado -->
    <div v-if="loading">Cargando proveedores...</div>
    <div v-else-if="error" class="text-red-600">Error: {{ error }}</div>

    <!-- Lista -->
    <ul v-else-if="proveedores.length" class="list-disc pl-5">
      <li v-for="p in proveedores" :key="p.id_proveedor">
        {{ p.id_proveedor }} - {{ p.nombre }}
      </li>
    </ul>
    <div v-else>No hay proveedores.</div>
  </div>
</template>

<style scoped>
div {
  font-family: system-ui, -apple-system, 'Segoe UI', Roboto, Arial;
}
</style>
