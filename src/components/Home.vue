<template>
  <button @click="goToLogin" class="btn-login">
    <i class="fas fa-arrow-right-to-bracket mr-2"></i> Đăng nhập
  </button>
</template>

<script setup>
import { useRouter } from 'vue-router'

const router = useRouter()

function goToLogin() {
  router.push('/login')
}
</script>

<style scoped>
.btn-login {
  background: linear-gradient(90deg, #a855f7, #ec4899);
  color: white;
  font-weight: 700;
  font-size: 1.1rem;
  border: none;
  border-radius: 9999px; /* bo tròn hết */
  padding: 0.75rem 1.5rem;
  box-shadow: 0 8px 20px rgba(236, 72, 153, 0.3);
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  transition: all 0.3s ease;
}

.btn-login:hover {
  box-shadow: 0 12px 24px rgba(236, 72, 153, 0.6);
  transform: translateY(-2px) scale(1.02);
}

.btn-login i {
  /* căn icon bên trái */
  font-size: 1.2rem;
}
</style>
