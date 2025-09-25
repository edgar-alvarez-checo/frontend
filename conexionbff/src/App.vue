<template>
  <div class="p-6 max-w-4xl mx-auto">
    <!-- Encabezado -->
    <h2 class="text-3xl font-bold text-gray-800 mb-8 text-center">
      👥 Usuarios Registrados
    </h2>

    <!-- Estado de carga -->
    <div v-if="loading" class="flex justify-center items-center py-10">
      <span class="animate-spin rounded-full h-10 w-10 border-4 border-blue-500 border-t-transparent"></span>
    </div>

    <!-- Error -->
    <div v-else-if="error" class="text-red-600 font-semibold bg-red-100 p-4 rounded-lg text-center">
      ⚠️ {{ error }}
    </div>

    <!-- Lista de usuarios -->
    <ul v-else class="grid gap-6 sm:grid-cols-2 lg:grid-cols-3">
      <li
        v-for="user in usuarios"
        :key="user.id"
        class="bg-gradient-to-br from-white to-gray-50 p-5 rounded-3xl shadow-lg hover:shadow-xl transition-all transform hover:-translate-y-1"
      >
        <h3 class="text-xl font-semibold text-gray-700 mb-2">
          {{ user.nombre }}
        </h3>
        <p class="text-sm text-gray-500">ID: {{ user.id }}</p>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue"

const usuarios = ref([])
const loading = ref(true)
const error = ref(null)

// GET usuarios
async function cargarUsuarios() {
  try {
    const res = await fetch("http://localhost:8080/usuarios")
    if (!res.ok) throw new Error("Error al cargar usuarios")
    usuarios.value = await res.json()
  } catch (err) {
    error.value = err.message
  } finally {
    loading.value = false
  }
}

onMounted(cargarUsuarios)
</script>
