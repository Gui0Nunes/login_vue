<template>
  <div class="container mt-5">
    <h2>Bem-vindo, você está logado!</h2>
    <button class="btn btn-danger mt-3" @click="logout">Logout</button>
  </div>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router'
const router = useRouter()

function logout() {
  localStorage.removeItem('token')
  router.push('/')
}
</script>
    