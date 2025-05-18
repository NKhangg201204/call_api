<template>
  <div class="flex items-center justify-center min-h-screen bg-gradient-to-br from-purple-100 to-yellow-100 px-4">
    <form class="form-card" @submit.prevent="handleLogin">
      <h2 class="form-title">
        <i class="fas fa-sign-in-alt mr-2"></i> Đăng nhập
      </h2>

      <div class="form-group">
        <label class="form-label">
          <i class="fas fa-user mr-2 text-purple-500"></i> Tên đăng nhập
        </label>
        <input v-model="user.username" type="text" placeholder="Nhập tên đăng nhập" class="form-input" />
      </div>

      <div class="form-group">
        <label class="form-label">
          <i class="fas fa-lock mr-2 text-purple-500"></i> Mật khẩu
        </label>
        <input v-model="user.password" type="password" placeholder="Nhập mật khẩu" class="form-input" />
      </div>

      <p v-if="log" class="error-msg">
        <i class="fas fa-exclamation-circle mr-1"></i>{{ log }}
      </p>

      <button type="submit" class="btn-submit">
        <i class="fas fa-arrow-right-to-bracket mr-2"></i> Đăng nhập
      </button>
      <span class="back-button" @click="goBack">
        <i class="fas fa-arrow-left"></i> Quay lại
      </span>
    </form>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue';
import VueJwtDecode from 'vue-jwt-decode';
import apiResource from '../composables/apiResource';
import { useRouter } from 'vue-router';

const { auth_login } = apiResource();
const router = useRouter();

const goBack = () => router.back();

const user = reactive({
  username: '',
  password: '',
});

const log = ref('');

const handleLogin = async () => {
  try {
    const response = await auth_login(user);
    if (response.access_token) {
      localStorage.setItem('access_token', response.access_token);
      const decodedToken = VueJwtDecode.decode(response.access_token);
      log.value = `Xin chào, ${decodedToken.email || 'người dùng'}!`;
    } else {
      log.value = 'Thông tin đăng nhập không đúng.';
    }
  } catch (error) {
    console.error('Login failed', error);
    log.value = 'Đăng nhập thất bại. Vui lòng thử lại.';
  }
};
</script>

<style scoped>
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(40px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Pulse effect for error */
@keyframes pulseRed {

  0%,
  100% {
    opacity: 1;
  }

  50% {
    opacity: 0.6;
  }
}

.back-button {
  display: inline-flex;
  align-items: center;
  color: #ffffff; 
  background: linear-gradient(90deg, #a855f7, #ec4899); 
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  font-weight: 700;
  font-size: 1.25rem; 
  cursor: pointer;
  user-select: none;
  transition: all 0.3s ease;
  margin-top: 1.5rem;
  padding: 0.5rem 1.25rem;
  border-radius: 9999px; 
  box-shadow: 0 6px 12px rgba(168, 85, 247, 0.5);
}

.back-button i {
  margin-right: 0.75rem;
  font-size: 1.3rem;
  color: white;
}

.back-button:hover {
  color: white;
  background: linear-gradient(90deg, #9333ea, #db2777);
  box-shadow: 0 10px 20px rgba(219, 39, 119, 0.7);
  transform: scale(1.05);
}



.form-card {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-radius: 2rem;
  box-shadow: 0 20px 40px rgba(147, 51, 234, 0.2);
  padding: 2.5rem;
  width: 100%;
  max-width: 420px;
  animation: fadeInUp 0.6s ease;
  border: 1px solid #e9d5ff;
}

.form-title {
  font-size: 2rem;
  font-weight: bold;
  background: linear-gradient(90deg, #a855f7, #ec4899);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
  text-align: center;
  margin-bottom: 1rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.form-label {
  color: #4b5563;
  font-weight: 600;
}

.form-input {
  padding: 0.75rem 1rem;
  border: 2px solid #ddd6fe;
  border-radius: 9999px;
  outline: none;
  transition: 0.3s ease;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
}

.form-input::placeholder {
  color: #9ca3af;
}

.form-input:hover {
  border-color: #c4b5fd;
}

.form-input:focus {
  border-color: #a78bfa;
  box-shadow: 0 0 0 4px rgba(167, 139, 250, 0.3);
}

.error-msg {
  color: #dc2626;
  font-weight: 600;
  font-size: 0.875rem;
  text-align: center;
  margin-top: 0.25rem;
  animation: pulseRed 1.5s infinite;
}

.btn-submit {
  background: linear-gradient(90deg, #a855f7, #ec4899);
  color: white;
  font-weight: 700;
  font-size: 1.1rem;
  border: none;
  border-radius: 9999px;
  padding: 0.75rem;
  width: 100%;
  margin-top: 1rem;
  transition: all 0.3s ease;
  box-shadow: 0 8px 20px rgba(236, 72, 153, 0.3);
  cursor: pointer;
}

.btn-submit:hover {
  box-shadow: 0 12px 24px rgba(236, 72, 153, 0.6);
  transform: translateY(-2px) scale(1.02);
}
</style>
