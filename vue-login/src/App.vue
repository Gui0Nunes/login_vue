<template>
  <div class="container d-flex justify-content-center align-items-center min-vh-100">
    <div class="card shadow p-4" style="width: 100%; max-width: 400px;">
      <h3 class="text-center mb-4">Login</h3>
      <form @submit.prevent="handleLogin">
        <div class="mb-3">
          <label for="email" class="form-label">Email</label>
          <input
            v-model="email"
            type="email"
            id="email"
            class="form-control"
            :class="{ 'is-invalid': emailError }"
            required
          />
          <div class="invalid-feedback">Email inválido.</div>
        </div>

        <div class="mb-3">
          <label for="password" class="form-label">Senha</label>
          <input
            v-model="password"
            type="password"
            id="password"
            class="form-control"
            :class="{ 'is-invalid': passwordError }"
            required
          />
          <div class="invalid-feedback">Senha é obrigatória.</div>
        </div>

        <button type="submit" class="btn btn-primary w-100" :disabled="loading">
          <span v-if="loading" class="spinner-border spinner-border-sm me-2" role="status" aria-hidden="true"></span>
          Entrar
        </button>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import api from './api'

const email = ref('')
const password = ref('')
const loading = ref(false)

const emailError = ref(false)
const passwordError = ref(false)

async function handleLogin() {
  emailError.value = !email.value.includes('@')
  passwordError.value = password.value.length === 0
  if (emailError.value || passwordError.value) return

  loading.value = true

  try {
    const response = await api.post('/login', {
      email: email.value,
      password: password.value,
    })

    const { message, token, user } = response.data
    alert(`Login OK: ${user.name} (token: ${token})`)
  } catch (error: any) {
    alert('Erro ao fazer login. Verifique suas credenciais.')
    console.error(error)
  } finally {
    loading.value = false
  }
}
</script>
