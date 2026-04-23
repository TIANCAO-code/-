<template>
  <div class="auth-container">
    <div class="auth-card">
      <div class="auth-body">
        <div v-if="isLogin" class="form-group">
          <input type="text" placeholder="账号 / 邮箱" class="auth-input" />
          <input type="password" placeholder="密码" class="auth-input" />
          <button class="btn btn-auth" @click="submitAuth">登录账号</button>
        </div>
        <div v-else class="form-group">
          <input type="text" placeholder="设置用户名" class="auth-input" />
          <input type="email" placeholder="邮箱" class="auth-input" />
          <input type="password" placeholder="设置密码" class="auth-input" />
          <button class="btn btn-auth" @click="submitAuth">注册</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, defineEmits } from 'vue';
const isLogin = ref(true);
const emit = defineEmits(['login-success']);
const submitAuth = () => {
    emit('login-success');
}
</script>

<style scoped>
.auth-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 400px;
  padding: 20px;
}

.auth-card {
  width: 350px;
  background: #ffffff;
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(99, 192, 137, 0.15);
  overflow: hidden;
  border: 1px solid #eaf6ef;
}

.auth-tabs {
  display: flex;
  position: relative;
  background: #f0f9f3;
  cursor: pointer;
}

.tab-item {
  flex: 1;
  text-align: center;
  padding: 15px 0;
  font-size: 16px;
  color: #8dae9a;
  z-index: 2;
  transition: color 0.3s;
}

.tab-item.active {
  color: #3b7b54;
  font-weight: 600;
}

.tab-slider {
  position: absolute;
  top: 0;
  width: 50%;
  height: 100%;
  background: #ffffff;
  transition: 0.3s cubic-bezier(0.18, 0.89, 0.32, 1.28);
  z-index: 1;
  border-radius: 20px 20px 0 0;
}

.auth-body {
  padding: 30px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.auth-input {
  border: 1px solid #e0eee5;
  padding: 12px 15px;
  border-radius: 10px;
  background: #fbfdfc;
  outline: none;
  transition: all 0.3s;
}

.auth-input:focus {
  border-color: #63c089;
  background: #fff;
  box-shadow: 0 0 8px rgba(99, 192, 137, 0.1);
}

.btn-auth {
  margin-top: 10px;
  background: #63c089;
  color: white;
  padding: 12px;
  border-radius: 10px;
  font-weight: 600;
  border: none;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(99, 192, 137, 0.2);
}

.btn-auth:hover {
  background: #4ea872;
}
</style>