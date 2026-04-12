<template>
  <div class="auth-page" :dir="dir">

    <!-- Left Panel -->
    <div class="auth-left">
      <div class="left-content">
        <router-link to="/" class="back-home">
          <i class="fas fa-arrow-left"></i> Back to Home
        </router-link>

        <div class="brand">
          <img src="../assets/ancs-logo.png" alt="ANCS" class="brand-logo">
          <h1>ANCS</h1>
          <p>Automated Network Configuration System</p>
        </div>

        <div class="features-list">
          <div class="feat-item">
            <div class="feat-icon"><i class="fas fa-robot"></i></div>
            <div>
              <h4>AI-Powered Automation</h4>
              <p>Automate complex network configurations intelligently</p>
            </div>
          </div>
          <div class="feat-item">
            <div class="feat-icon"><i class="fas fa-shield-alt"></i></div>
            <div>
              <h4>Enterprise Security</h4>
              <p>AES-encrypted credentials and secure SSH tunnels</p>
            </div>
          </div>
          <div class="feat-item">
            <div class="feat-icon"><i class="fas fa-tachometer-alt"></i></div>
            <div>
              <h4>Real-time Dashboard</h4>
              <p>Monitor and manage all your devices from one place</p>
            </div>
          </div>
        </div>
      </div>

      <div class="left-glow"></div>
    </div>

    <!-- Right Panel -->
    <div class="auth-right">
      <div class="auth-box">

        <!-- Tabs -->
        <div class="auth-tabs">
          <button :class="{ active: mode === 'login' }" @click="switchMode('login')">Sign In</button>
          <button :class="{ active: mode === 'signup' }" @click="switchMode('signup')">Sign Up</button>
          <div class="tab-indicator" :class="{ right: mode === 'signup' }"></div>
        </div>

        <!-- LOGIN -->
        <transition name="form-slide" mode="out-in">
          <div v-if="mode === 'login'" key="login" class="form-section">
            <div class="form-header">
              <h2>Welcome back</h2>
              <p>Sign in to your ANCS account</p>
            </div>

            <div class="form-group">
              <label>Username</label>
              <div class="input-wrap" :class="{ focused: focused === 'username', error: errors.username }">
                <i class="fas fa-user"></i>
                <input
                  type="text"
                  v-model="loginData.username"
                  placeholder="Enter your username"
                  @focus="focused = 'username'"
                  @blur="focused = ''"
                  @keyup.enter="handleLogin"
                >
              </div>
              <span class="err-msg" v-if="errors.username">{{ errors.username }}</span>
            </div>

            <div class="form-group">
              <label>
                Password
                <a href="#" class="forgot">Forgot password?</a>
              </label>
              <div class="input-wrap" :class="{ focused: focused === 'password', error: errors.password }">
                <i class="fas fa-lock"></i>
                <input
                  :type="showPass ? 'text' : 'password'"
                  v-model="loginData.password"
                  placeholder="Enter your password"
                  @focus="focused = 'password'"
                  @blur="focused = ''"
                  @keyup.enter="handleLogin"
                >
                <button class="eye-btn" @click="showPass = !showPass" type="button">
                  <i :class="showPass ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
                </button>
              </div>
              <span class="err-msg" v-if="errors.password">{{ errors.password }}</span>
            </div>

            <div v-if="serverError" class="server-error">
              <i class="fas fa-exclamation-triangle"></i> {{ serverError }}
            </div>

            <button class="submit-btn" @click="handleLogin" :disabled="isLoading" :class="{ loading: isLoading }">
              <span v-if="!isLoading"><i class="fas fa-sign-in-alt"></i> Sign In</span>
              <span v-else class="loader-wrap"><span class="spinner"></span> Signing in...</span>
            </button>

            <p class="switch-hint">
              Don't have an account?
              <button @click="switchMode('signup')">Create one free</button>
            </p>
          </div>

          <!-- SIGNUP -->
          <div v-else key="signup" class="form-section">
            <div class="form-header">
              <h2>Create account</h2>
              <p>Join ANCS and start automating</p>
            </div>

            <div class="form-group">
              <label>Full Name</label>
              <div class="input-wrap" :class="{ focused: focused === 'name' }">
                <i class="fas fa-user-circle"></i>
                <input type="text" v-model="signupData.name" placeholder="Your full name"
                  @focus="focused = 'name'" @blur="focused = ''">
              </div>
            </div>

            <div class="form-group">
              <label>Email Address</label>
              <div class="input-wrap" :class="{ focused: focused === 'email' }">
                <i class="fas fa-envelope"></i>
                <input type="email" v-model="signupData.email" placeholder="your@email.com"
                  @focus="focused = 'email'" @blur="focused = ''">
              </div>
            </div>

            <div class="form-group">
              <label>Password</label>
              <div class="input-wrap" :class="{ focused: focused === 'spass' }">
                <i class="fas fa-lock"></i>
                <input :type="showPass ? 'text' : 'password'" v-model="signupData.password"
                  placeholder="Min. 8 characters"
                  @focus="focused = 'spass'" @blur="focused = ''">
                <button class="eye-btn" @click="showPass = !showPass" type="button">
                  <i :class="showPass ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
                </button>
              </div>
              <!-- Password strength -->
              <div class="strength-bar" v-if="signupData.password">
                <div class="strength-fill" :class="passwordStrength.class" :style="{ width: passwordStrength.width }"></div>
              </div>
              <span class="strength-label" v-if="signupData.password" :class="passwordStrength.class">
                {{ passwordStrength.label }}
              </span>
            </div>

            <div v-if="serverError" class="server-error">
              <i class="fas fa-exclamation-triangle"></i> {{ serverError }}
            </div>
            <div v-if="successMsg" class="server-success">
              <i class="fas fa-check-circle"></i> {{ successMsg }}
            </div>

            <button class="submit-btn signup-btn" @click="handleSignup" :disabled="isLoading" :class="{ loading: isLoading }">
              <span v-if="!isLoading"><i class="fas fa-rocket"></i> Create Account</span>
              <span v-else class="loader-wrap"><span class="spinner"></span> Creating...</span>
            </button>

            <p class="switch-hint">
              Already have an account?
              <button @click="switchMode('login')">Sign in</button>
            </p>
          </div>
        </transition>

      </div>
    </div>

  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'
import { useAuthStore } from '../stores/auth'

const router    = useRouter()
const authStore = useAuthStore()
const mode = ref('login')
const focused = ref('')
const showPass = ref(false)
const isLoading = ref(false)
const serverError = ref('')
const successMsg = ref('')
const errors = ref({})
const dir = ref('ltr')

const loginData = ref({ username: '', password: '' })
const signupData = ref({ name: '', email: '', password: '' })

const switchMode = (m) => {
  mode.value = m
  serverError.value = ''
  successMsg.value = ''
  errors.value = {}
  showPass.value = false
}

const passwordStrength = computed(() => {
  const p = signupData.value.password
  if (!p) return {}
  if (p.length < 6) return { class: 'weak', width: '25%', label: 'Weak' }
  if (p.length < 10) return { class: 'fair', width: '50%', label: 'Fair' }
  if (p.match(/[A-Z]/) && p.match(/[0-9]/)) return { class: 'strong', width: '100%', label: 'Strong' }
  return { class: 'good', width: '75%', label: 'Good' }
})

const validateLogin = () => {
  errors.value = {}
  if (!loginData.value.username) errors.value.username = 'Username is required'
  if (!loginData.value.password) errors.value.password = 'Password is required'
  return Object.keys(errors.value).length === 0
}

const handleLogin = async () => {
  if (!validateLogin()) return
  isLoading.value = true
  serverError.value = ''
  try {
    const res = await fetch('http://127.0.0.1:8000/api/login/', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      
      body: JSON.stringify({ username: loginData.value.username.trim(), password: loginData.value.password.trim() })
    })
    const data = await res.json()
    if (!res.ok) throw new Error(data.error || data.detail || 'Invalid credentials')
    // ✅ store يحفظ token + username + role
    authStore.loginSuccess(data.access, data.username, data.role)

    // ✅ توجيه حسب الـ role
    if (data.role === 'admin') {
      router.push('/dashboard')
    } else {
      router.push('/')
    }
  } catch (err) {
    serverError.value = err.message || 'Login failed. Please try again.'
  } finally {
    isLoading.value = false
  }
}

const handleSignup = async () => {
  if (!signupData.value.name || !signupData.value.email || !signupData.value.password) {
    serverError.value = 'Please fill in all fields.'
    return
  }
  isLoading.value = true
  serverError.value = ''
  try {
    const res = await fetch('http://127.0.0.1:8000/api/register/', {

      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      
      body: JSON.stringify({
        username: signupData.value.name.trim(),
        email:    signupData.value.email,
        password: signupData.value.password
      })
    })
    const data = await res.json()
    if (!res.ok) throw new Error(data.username?.[0] || data.email?.[0] || 'Signup failed')
    successMsg.value = 'Account created! You can now login.'
    setTimeout(() => switchMode('login'), 1800)
  } catch (err) {
    serverError.value = err.message || 'Signup failed. Please try again.'
  } finally {
    isLoading.value = false
  }
}
</script>

<style scoped>
.auth-page {
  min-height: 100vh;
  display: flex;
  background: #0b1c2c;
  padding-top: 0;
}

/* LEFT PANEL */
.auth-left {
  width: 45%;
  background: linear-gradient(145deg, #0d2137, #071525);
  padding: 60px 50px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: relative;
  overflow: hidden;
  border-right: 1px solid rgba(66,165,245,0.1);
}
.left-glow {
  position: absolute;
  width: 400px; height: 400px;
  background: radial-gradient(circle, rgba(66,165,245,0.12) 0%, transparent 70%);
  bottom: -100px; left: -100px;
  pointer-events: none;
}
.back-home {
  display: inline-flex; align-items: center; gap: 8px;
  color: rgba(255,255,255,0.5); text-decoration: none;
  font-size: 14px; font-weight: 500;
  transition: color 0.25s; margin-bottom: 60px;
}
.back-home:hover { color: #42a5f5; }
.brand { margin-bottom: 60px; }
.brand-logo { width: 56px; border-radius: 14px; margin-bottom: 16px; }
.brand h1 {
  font-size: 40px; font-weight: 900; letter-spacing: -1px;
  background: linear-gradient(135deg, #42a5f5, #0096c7);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
  margin-bottom: 8px;
}
.brand > p { font-size: 15px; color: rgba(255,255,255,0.45); }

.features-list { display: flex; flex-direction: column; gap: 28px; }
.feat-item { display: flex; align-items: flex-start; gap: 18px; }
.feat-icon {
  width: 44px; height: 44px; flex-shrink: 0;
  background: rgba(66,165,245,0.1);
  border: 1px solid rgba(66,165,245,0.2);
  border-radius: 12px;
  display: flex; align-items: center; justify-content: center;
  color: #42a5f5; font-size: 18px;
}
.feat-item h4 { font-size: 15px; font-weight: 700; color: #fff; margin-bottom: 4px; }
.feat-item p { font-size: 13px; color: rgba(255,255,255,0.45); line-height: 1.5; }

/* RIGHT PANEL */
.auth-right {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 60px 40px;
  background: #0b1c2c;
}
.auth-box {
  width: 100%;
  max-width: 420px;
}

/* TABS */
.auth-tabs {
  position: relative;
  display: flex;
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.08);
  border-radius: 16px;
  padding: 6px;
  margin-bottom: 36px;
  overflow: hidden;
}
.auth-tabs button {
  flex: 1; padding: 12px; border: none; background: none;
  color: rgba(255,255,255,0.45); font-size: 14.5px; font-weight: 600;
  cursor: pointer; border-radius: 11px; transition: color 0.3s;
  font-family: inherit; position: relative; z-index: 1;
}
.auth-tabs button.active { color: #fff; }
.tab-indicator {
  position: absolute;
  top: 6px; left: 6px;
  width: calc(50% - 6px); height: calc(100% - 12px);
  background: linear-gradient(135deg, #0077b6, #0096c7);
  border-radius: 11px;
  transition: transform 0.35s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 4px 15px rgba(0,119,182,0.35);
}
.tab-indicator.right { transform: translateX(100%); }

/* FORM */
.form-header { margin-bottom: 32px; }
.form-header h2 { font-size: 28px; font-weight: 800; color: #fff; margin-bottom: 6px; }
.form-header p { font-size: 14px; color: rgba(255,255,255,0.45); }

.form-group { margin-bottom: 20px; }
.form-group label {
  display: flex; justify-content: space-between; align-items: center;
  font-size: 13px; font-weight: 600; color: rgba(255,255,255,0.65);
  margin-bottom: 8px;
}
.forgot { color: #42a5f5; text-decoration: none; font-weight: 500; font-size: 12.5px; }
.forgot:hover { text-decoration: underline; }

.input-wrap {
  position: relative; display: flex; align-items: center;
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 12px; transition: all 0.25s;
}
.input-wrap.focused {
  border-color: #42a5f5;
  background: rgba(66,165,245,0.05);
  box-shadow: 0 0 0 3px rgba(66,165,245,0.1);
}
.input-wrap.error { border-color: #ef4444; background: rgba(239,68,68,0.04); }
.input-wrap > i:first-child {
  position: absolute; left: 15px;
  color: rgba(255,255,255,0.25); font-size: 14px; pointer-events: none;
}
.input-wrap input {
  width: 100%; padding: 14px 44px 14px 42px;
  background: none; border: none; color: #fff;
  font-size: 14.5px; font-family: inherit; outline: none;
}
.input-wrap input::placeholder { color: rgba(255,255,255,0.2); }
.eye-btn {
  position: absolute; right: 14px; background: none; border: none;
  color: rgba(255,255,255,0.25); cursor: pointer; font-size: 14px;
  padding: 6px; transition: color 0.2s;
}
.eye-btn:hover { color: rgba(255,255,255,0.6); }

.err-msg { display: block; font-size: 12px; color: #fca5a5; margin-top: 6px; }

.server-error {
  display: flex; align-items: center; gap: 8px;
  background: rgba(239,68,68,0.08); border: 1px solid rgba(239,68,68,0.2);
  border-radius: 10px; padding: 12px 16px;
  color: #fca5a5; font-size: 13px; margin-bottom: 20px;
}
.server-success {
  display: flex; align-items: center; gap: 8px;
  background: rgba(34,197,94,0.08); border: 1px solid rgba(34,197,94,0.2);
  border-radius: 10px; padding: 12px 16px;
  color: #86efac; font-size: 13px; margin-bottom: 20px;
}

/* Password strength */
.strength-bar {
  height: 3px; background: rgba(255,255,255,0.08);
  border-radius: 2px; margin-top: 8px; overflow: hidden;
}
.strength-fill { height: 100%; border-radius: 2px; transition: all 0.4s ease; }
.strength-fill.weak { background: #ef4444; }
.strength-fill.fair { background: #f59e0b; }
.strength-fill.good { background: #3b82f6; }
.strength-fill.strong { background: #22c55e; }
.strength-label { font-size: 11px; font-weight: 600; margin-top: 4px; display: block; }
.strength-label.weak { color: #ef4444; }
.strength-label.fair { color: #f59e0b; }
.strength-label.good { color: #3b82f6; }
.strength-label.strong { color: #22c55e; }

/* Submit */
.submit-btn {
  width: 100%; padding: 15px;
  background: linear-gradient(135deg, #42a5f5, #0077b6);
  border: none; border-radius: 12px; color: #fff;
  font-size: 15px; font-weight: 700; cursor: pointer;
  transition: all 0.3s; font-family: inherit;
  margin-bottom: 24px; margin-top: 8px;
  display: flex; align-items: center; justify-content: center; gap: 8px;
}
.submit-btn:hover:not(:disabled) { transform: translateY(-2px); box-shadow: 0 10px 30px rgba(0,119,182,0.4); }
.submit-btn:disabled { opacity: 0.6; cursor: not-allowed; transform: none; }
.submit-btn.signup-btn { background: linear-gradient(135deg, #0077b6, #005b8e); }
.loader-wrap { display: flex; align-items: center; gap: 10px; }
.spinner {
  width: 16px; height: 16px;
  border: 2px solid rgba(255,255,255,0.3);
  border-top-color: #fff; border-radius: 50%;
  animation: spin 0.7s linear infinite; display: inline-block;
}
@keyframes spin { to { transform: rotate(360deg); } }

.switch-hint { text-align: center; font-size: 13.5px; color: rgba(255,255,255,0.4); }
.switch-hint button {
  background: none; border: none; color: #42a5f5;
  font-size: 13.5px; font-weight: 600; cursor: pointer;
  font-family: inherit; margin-left: 4px;
}
.switch-hint button:hover { text-decoration: underline; }

/* Transitions */
.form-slide-enter-active, .form-slide-leave-active { transition: all 0.25s ease; }
.form-slide-enter-from { opacity: 0; transform: translateX(20px); }
.form-slide-leave-to { opacity: 0; transform: translateX(-20px); }

/* Responsive */
@media (max-width: 900px) {
  .auth-left { display: none; }
  .auth-right { padding: 40px 24px; }
}
</style>
