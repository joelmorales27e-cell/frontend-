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
  <div class="max-w-xl mx-auto mt-10 p-6 bg-gradient-to-br from-white to-gray-100 rounded-2xl shadow-xl border border-gray-200">
    <h2 class="text-2xl font-extrabold text-gray-800 mb-6 text-center">📋 Proveedores</h2>

    <!-- Formulario -->
    <form @submit.prevent="agregarProveedor" class="flex gap-3 mb-6">
      <input
        v-model="nuevoNombre"
        type="text"
        placeholder="Nombre del proveedor"
        class="flex-1 border border-gray-300 rounded-lg px-4 py-2 focus:ring-2 focus:ring-blue-400 focus:outline-none transition"
      />
      <button
        type="submit"
        class="bg-blue-600 text-white px-5 py-2 rounded-lg hover:bg-blue-700 transition shadow-md"
      >
        Agregar
      </button>
    </form>

    <!-- Estado -->
    <div v-if="loading" class="text-center text-gray-500 animate-pulse">Cargando proveedores...</div>
    <div v-else-if="error" class="text-red-600 text-center font-semibold">Error: {{ error }}</div>

    <!-- Lista -->
    <ul v-else-if="proveedores.length" class="space-y-2">
      <li
        v-for="p in proveedores"
        :key="p.id_proveedor"
        class="flex justify-between items-center p-3 bg-white rounded-lg shadow hover:shadow-md transition"
      >
        <span class="font-medium text-gray-700">{{ p.nombre }}</span>
        <span class="text-gray-400 text-sm">ID: {{ p.id_proveedor }}</span>
      </li>
    </ul>

    <div v-else class="text-center text-gray-500 font-medium mt-4">No hay proveedores.</div>
  </div>
</template>

<style scoped>
/* Pequeños ajustes para animaciones y tipografía */
body {
  font-family: 'Inter', system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
}
</style>
