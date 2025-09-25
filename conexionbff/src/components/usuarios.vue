<template>
  <div class="p-4">
    <h2 class="text-xl font-bold mb-4">Usuario desde API</h2>

    <div v-if="loading">Cargando...</div>
    <div v-else-if="error" class="text-red-500">{{ error }}</div>
    
    <div v-else class="p-3 bg-gray-100 rounded-lg shadow">
      <p><strong>ID:</strong> {{ usuario.id }}</p>
      <p><strong>Nombre:</strong> {{ usuario.nombre }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue"

const usuario = ref(null)
const loading = ref(true)
const error = ref(null)

onMounted(async () => {
  try {
    const res = await fetch("http://localhost:8080/usuarios")
    if (!res.ok) throw new Error("Error al cargar el usuario")
    usuario.value = await res.json()
  } catch (err) {
    error.value = err.message
  } finally {
    loading.value = false
  }
})
</script>
