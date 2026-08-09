<template>
  <div class="login-container">
    <form class="login-form" @submit.prevent="handleLogin">
      <h1>Entrar</h1>

      <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>

      <div class="field">
        <label for="email">Email</label>
        <input
          id="email"
          v-model="email"
          type="email"
          placeholder="seu@email.com"
          required
          autocomplete="email"
        />
      </div>

      <div class="field">
        <label for="password">Senha</label>
        <input
          id="password"
          v-model="password"
          type="password"
          placeholder="••••••••"
          required
          autocomplete="current-password"
        />
      </div>

      <button type="submit" :disabled="loading">
        {{ loading ? 'Entrando...' : 'Entrar' }}
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { useAuthStore } from '@/stores/auth';

const router = useRouter();
const authStore = useAuthStore();

const email = ref('');
const password = ref('');
const loading = ref(false);
const errorMessage = ref('');

async function handleLogin() {
  loading.value = true;
  errorMessage.value = '';
  try {
    await authStore.login(email.value, password.value);
    router.push('/');
  } catch (err) {
    errorMessage.value =
      err.response?.data?.detail ??
      'Erro ao entrar. Verifique suas credenciais.';
  } finally {
    loading.value = false;
  }
}
</script>

<style scoped>
.login-container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: calc(100vh - 88px);
  padding: 24px 16px 32px;
}

.login-form {
  width: 100%;
  max-width: 420px;
  background: #ffffff;
  border: 1px solid #e6e9ee;
  border-radius: 18px;
  box-shadow: 0 18px 45px rgba(32, 41, 56, 0.08);
  padding: 32px;
}

.login-form h1 {
  font-size: 1.75rem;
  margin-bottom: 24px;
  color: #1f2937;
}

.field {
  display: grid;
  gap: 8px;
  margin-bottom: 18px;
}

.field label {
  font-size: 0.95rem;
  color: #475569;
}

.field input {
  width: 100%;
  padding: 14px 16px;
  border: 2px solid #d6d8df;
  border-radius: 12px;
  font-size: 1rem;
  outline: none;
  transition: border-color 0.2s, box-shadow 0.2s;
}

.field input:focus {
  border-color: #4a90d9;
  box-shadow: 0 0 0 4px rgba(74, 144, 217, 0.12);
}

button {
  width: 100%;
  margin-top: 8px;
  padding: 14px 16px;
  border: none;
  border-radius: 12px;
  background-color: #4a90d9;
  color: white;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.2s, transform 0.2s;
}

button:hover:not(:disabled) {
  background-color: #357abd;
  transform: translateY(-1px);
}

button:disabled {
  opacity: 0.65;
  cursor: not-allowed;
}

.error-message {
  margin-bottom: 18px;
  padding: 12px 14px;
  background: #fee2e2;
  border: 1px solid #fca5a5;
  border-radius: 12px;
  color: #991b1b;
  font-size: 0.95rem;
}

@media (max-width: 480px) {
  .login-form {
    padding: 24px;
  }
}
</style>