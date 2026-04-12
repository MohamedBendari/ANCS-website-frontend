<template>
  <div id="app">

    <!-- ===== NAVBAR ===== -->
    <nav v-if="showLayout" class="navbar" :class="{ 'scrolled': isScrolled }">
      <div class="nav-container">

        <!-- Logo -->
        <router-link to="/" class="logo" @click="closeMenu">
          <img src="./assets/ancs-logo.png" alt="ANCS Logo">
          <span>ANCS</span>
        </router-link>

        <!-- Desktop Links -->
        <div class="nav-links">
          <router-link to="/">Home</router-link>
          <router-link to="/about">About</router-link>
          <router-link to="/features">Features</router-link>
          <router-link to="/team">Team</router-link>
          <router-link to="/contact">Contact</router-link>
        </div>

        <!-- Desktop Actions -->
        <div class="nav-actions">
          <template v-if="!isLoggedIn">
            <button class="btn-login" @click="openAuth('login')">Login</button>
            <button class="btn-signup" @click="openAuth('signup')">Sign Up</button>
            <router-link to="/download" class="btn-download">
              <i class="fas fa-download"></i> Download
            </router-link>
          </template>
          <template v-else>
            <template v-if="authStore.isAdmin">
              <router-link to="/dashboard" class="btn-dashboard">
                <i class="fas fa-tachometer-alt"></i> Dashboard
              </router-link>
            </template>
            <template v-else>
              <div class="user-avatar-nav" :title="authStore.username">
                {{ authStore.username?.charAt(0)?.toUpperCase() }}
              </div>
            </template>
            <router-link to="/download" class="btn-download">
              <i class="fas fa-download"></i> Download
            </router-link>
            <button class="btn-logout" @click="logout">Logout</button>
          </template>
        </div>

        <!-- Mobile Toggle -->
        <button class="mobile-toggle" @click="toggleMenu" :class="{ 'active': isMenuOpen }" aria-label="Toggle menu">
          <span></span>
          <span></span>
          <span></span>
        </button>

      </div>

      <!-- Mobile Menu -->
      <div class="mobile-menu" :class="{ 'open': isMenuOpen }">
        <router-link to="/" @click="closeMenu">Home</router-link>
        <router-link to="/about" @click="closeMenu">About</router-link>
        <router-link to="/features" @click="closeMenu">Features</router-link>
        <router-link to="/team" @click="closeMenu">Team</router-link>
        <router-link to="/contact" @click="closeMenu">Contact</router-link>
        <div class="mobile-actions">
          <template v-if="!isLoggedIn">
            <button @click="openAuth('login'); closeMenu()">Login</button>
            <button @click="openAuth('signup'); closeMenu()">Sign Up</button>
          </template>
          <template v-else>
            <template v-if="authStore.isAdmin">
              <router-link to="/dashboard" @click="closeMenu">Dashboard</router-link>
            </template>
            <template v-else>
              <div class="mobile-user-info">
                <div class="mobile-avatar">{{ authStore.username?.charAt(0)?.toUpperCase() }}</div>
                <span>{{ authStore.username }}</span>
              </div>
            </template>
            <button @click="logout; closeMenu()">Logout</button>
          </template>
          <router-link to="/download" class="mobile-download" @click="closeMenu">Download</router-link>
        </div>
      </div>
    </nav>

    <!-- ===== AUTH MODAL ===== -->
    <Transition name="modal-fade">
      <div v-if="showLayout && showAuthModal" class="modal-overlay" @click.self="closeAuth">
        <div class="auth-modal">

          <button class="close-btn" @click="closeAuth" aria-label="Close">
            <i class="fas fa-times"></i>
          </button>

          <div class="modal-tabs">
            <button :class="{ active: isLogin }" @click="isLogin = true; resetStatus()">Login</button>
            <button :class="{ active: !isLogin }" @click="isLogin = false; resetStatus()">Sign Up</button>
          </div>

          <!-- Login Form -->
          <div v-if="isLogin" class="modal-form">
            <div class="form-header">
              <h2>Welcome Back</h2>
              <p>Access your network automation tools</p>
            </div>
            <div class="form-group">
              <label>Username</label>
              <div class="input-wrap">
                <i class="fas fa-user"></i>
                <input type="text" v-model="loginData.username" placeholder="Enter your username" required>
              </div>
            </div>
            <div class="form-group">
              <label>Password</label>
              <div class="input-wrap">
                <i class="fas fa-lock"></i>
                <input :type="showPassword ? 'text' : 'password'" v-model="loginData.password" placeholder="Enter your password" required>
                <button class="toggle-pass" @click="showPassword = !showPassword" type="button">
                  <i :class="showPassword ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
                </button>
              </div>
            </div>
            <div v-if="authStatus" class="status-msg" :class="authStatus.type">
              <i :class="authStatus.type === 'error' ? 'fas fa-exclamation-circle' : 'fas fa-check-circle'"></i>
              {{ authStatus.message }}
            </div>
            <button class="submit-btn" @click="handleLogin" :disabled="isLoading">
              <span v-if="!isLoading">Login</span>
              <span v-else><i class="fas fa-spinner fa-spin"></i> Logging in...</span>
            </button>
            <p class="switch-text">Don't have an account? <button @click="isLogin = false; resetStatus()">Sign Up</button></p>
          </div>

          <!-- Signup Form -->
          <div v-else class="modal-form">
            <div class="form-header">
              <h2>Create Account</h2>
              <p>Start your automation journey with ANCS</p>
            </div>
            <div class="form-group">
              <label>Full Name</label>
              <div class="input-wrap">
                <i class="fas fa-user"></i>
                <input type="text" v-model="signupData.name" placeholder="Your full name" required>
              </div>
            </div>
            <div class="form-group">
              <label>Email Address</label>
              <div class="input-wrap">
                <i class="fas fa-envelope"></i>
                <input type="email" v-model="signupData.email" placeholder="your@email.com" required>
              </div>
            </div>
            <div class="form-group">
              <label>Password</label>
              <div class="input-wrap">
                <i class="fas fa-lock"></i>
                <input :type="showPassword ? 'text' : 'password'" v-model="signupData.password" placeholder="Create a strong password" required>
                <button class="toggle-pass" @click="showPassword = !showPassword" type="button">
                  <i :class="showPassword ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
                </button>
              </div>
            </div>
            <div v-if="authStatus" class="status-msg" :class="authStatus.type">
              <i :class="authStatus.type === 'error' ? 'fas fa-exclamation-circle' : 'fas fa-check-circle'"></i>
              {{ authStatus.message }}
            </div>
            <button class="submit-btn signup" @click="handleSignup" :disabled="isLoading">
              <span v-if="!isLoading">Join Now</span>
              <span v-else><i class="fas fa-spinner fa-spin"></i> Creating account...</span>
            </button>
            <p class="switch-text">Already have an account? <button @click="isLogin = true; resetStatus()">Login</button></p>
          </div>

        </div>
      </div>
    </Transition>

    <!-- ===== PAGE CONTENT ===== -->
    <router-view v-slot="{ Component }">
      <transition name="fade">
        <component :is="Component" :key="$route.fullPath" @open-auth="openAuth" />
      </transition>
    </router-view>

    <!-- ===== FOOTER ===== -->
    <footer v-if="showLayout" class="footer">
      <div class="footer-container">
        <div class="footer-brand">
          <img src="./assets/ancs-logo.png" alt="ANCS Logo">
          <span>ANCS</span>
          <p>Automated Network Configuration System — automating the future of networking.</p>
        </div>
        <div class="footer-links">
          <div class="footer-col">
            <h4>Product</h4>
            <router-link to="/features">Features</router-link>
            <router-link to="/download">Download</router-link>
            <router-link to="/about">About</router-link>
          </div>
          <div class="footer-col">
            <h4>Company</h4>
            <router-link to="/team">Team</router-link>
            <router-link to="/contact">Contact</router-link>
          </div>
          <div class="footer-col">
            <h4>Connect</h4>
            <a href="https://github.com/MohamedBendari" target="_blank"><i class="fab fa-github"></i> GitHub</a>
            <a href="mailto:mohamedsharkawy078@gmail.com"><i class="fas fa-envelope"></i> Email</a>
          </div>
        </div>
      </div>
      <div class="footer-bottom">
        <p>© 2026 ANCS. All rights reserved. Built with <i class="fas fa-heart"></i> by the ANCS Team.</p>
      </div>
    </footer>

  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import { useAuthStore } from './stores/auth'

const router    = useRouter()
const route     = useRoute()
const authStore = useAuthStore()

// ── Layout visibility ──
const showLayout = computed(() => !route.meta.hideLayout)

// ── Navbar ──
const isMenuOpen = ref(false)
const isScrolled = ref(false)
const toggleMenu = () => { isMenuOpen.value = !isMenuOpen.value }
const closeMenu  = () => { isMenuOpen.value = false }
const handleScroll = () => { isScrolled.value = window.scrollY > 50 }
onMounted(()  => window.addEventListener('scroll', handleScroll))
onUnmounted(() => window.removeEventListener('scroll', handleScroll))

// ── Auth state ──
const isLoggedIn = computed(() => authStore.isLoggedIn)

const showAuthModal = ref(false)
const isLogin       = ref(true)
const showPassword  = ref(false)
const isLoading     = ref(false)
const authStatus    = ref(null)
const loginData     = ref({ username: '', password: '' })
const signupData    = ref({ name: '', email: '', password: '' })

const openAuth = (type) => {
  isLogin.value = type === 'login'
  showAuthModal.value = true
  document.body.style.overflow = 'hidden'
  resetStatus()
}
const closeAuth = () => {
  showAuthModal.value = false
  document.body.style.overflow = 'auto'
  showPassword.value = false
}
const resetStatus = () => { authStatus.value = null }

const handleLogin = async () => {
  if (!loginData.value.username || !loginData.value.password) {
    authStatus.value = { type: 'error', message: 'Please fill in all fields.' }
    return
  }
  isLoading.value = true
  resetStatus()
  try {
    const res = await fetch('http://127.0.0.1:8000/api/login/', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        username: loginData.value.username.trim(),
        password: loginData.value.password.trim()
      })
    })
    const data = await res.json()
    if (!res.ok) throw new Error(data.error || data.detail || 'Invalid credentials')
    authStore.loginSuccess(data.access, data.username, data.role)
    closeAuth()
    if (data.role === 'admin') {
      router.push('/dashboard')
    }
  } catch (err) {
    authStatus.value = { type: 'error', message: err.message || 'Login failed.' }
  } finally {
    isLoading.value = false
  }
}

const handleSignup = async () => {
  if (!signupData.value.name || !signupData.value.email || !signupData.value.password) {
    authStatus.value = { type: 'error', message: 'Please fill in all fields.' }
    return
  }
  isLoading.value = true
  resetStatus()
  try {
    const res = await fetch('http://127.0.0.1:8000/api/register/', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        username: signupData.value.name.trim(),
        email:    signupData.value.email.trim(),
        password: signupData.value.password
      })
    })
    const data = await res.json()
    if (!res.ok) throw new Error(data.username?.[0] || data.email?.[0] || 'Signup failed')
    authStatus.value = { type: 'success', message: 'Account created! Please login.' }
    setTimeout(() => { isLogin.value = true; resetStatus() }, 1500)
  } catch (err) {
    authStatus.value = { type: 'error', message: err.message || 'Signup failed.' }
  } finally {
    isLoading.value = false
  }
}

const logout = () => {
  authStore.logout()
  router.push('/')
}
</script>

<style>
/* base.css handles reset, fonts, body, #app */

/* NAVBAR */
.navbar {
  width: 100%;
  position: fixed;
  top: 0; left: 0;
  z-index: 1000;
  background: rgba(11, 28, 44, 0.7);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-bottom: 1px solid rgba(255,255,255,0.06);
  transition: all 0.4s ease;
}
.navbar.scrolled {
  background: rgba(11, 28, 44, 0.97);
  box-shadow: 0 4px 30px rgba(0,0,0,0.4);
  border-bottom-color: rgba(66, 165, 245, 0.15);
}
.nav-container {
  max-width: 1400px;
  margin: 0 auto;
  height: 72px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 40px;
}
.logo { display: flex; align-items: center; gap: 10px; text-decoration: none; flex-shrink: 0; }
.logo img { width: 38px; height: 38px; border-radius: 10px; object-fit: contain; }
.logo span {
  font-size: 22px; font-weight: 800; letter-spacing: -0.5px;
  background: linear-gradient(135deg, #42a5f5, #0096c7);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
}
.nav-links { display: flex; align-items: center; gap: 4px; }
.nav-links a {
  color: rgba(255,255,255,0.75); text-decoration: none;
  font-size: 14.5px; font-weight: 500;
  padding: 6px 14px; border-radius: 8px; transition: all 0.25s ease;
}
.nav-links a:hover { color: #fff; background: rgba(66, 165, 245, 0.12); }
.nav-links a.router-link-exact-active { color: #42a5f5; background: rgba(66, 165, 245, 0.1); }

.nav-actions { display: flex; align-items: center; gap: 10px; flex-shrink: 0; }
.btn-login {
  background: none; border: none; color: rgba(255,255,255,0.85);
  font-size: 14px; font-weight: 500; cursor: pointer;
  padding: 8px 16px; border-radius: 8px; transition: all 0.25s; font-family: inherit;
}
.btn-login:hover { color: #fff; background: rgba(255,255,255,0.07); }
.btn-signup {
  background: rgba(66, 165, 245, 0.1); border: 1px solid rgba(66, 165, 245, 0.4);
  color: #42a5f5; font-size: 14px; font-weight: 600; cursor: pointer;
  padding: 8px 18px; border-radius: 8px; transition: all 0.25s; font-family: inherit;
}
.btn-signup:hover { background: #42a5f5; color: #fff; box-shadow: 0 0 20px rgba(66,165,245,0.3); }
.btn-download {
  background: linear-gradient(135deg, #0077b6, #0096c7);
  color: #fff; text-decoration: none; font-size: 14px; font-weight: 600;
  padding: 9px 20px; border-radius: 8px; transition: all 0.3s;
  display: flex; align-items: center; gap: 7px;
}
.btn-download:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(0,119,182,0.4); }
.btn-dashboard {
  background: rgba(66, 165, 245, 0.1); border: 1px solid rgba(66, 165, 245, 0.3);
  color: #42a5f5; text-decoration: none; font-size: 14px; font-weight: 600;
  padding: 8px 16px; border-radius: 8px; transition: all 0.25s;
  display: flex; align-items: center; gap: 7px;
}
.btn-dashboard:hover { background: #42a5f5; color: #fff; }
.btn-logout {
  background: rgba(239, 68, 68, 0.1); border: 1px solid rgba(239, 68, 68, 0.3);
  color: #ef4444; font-size: 14px; font-weight: 500; cursor: pointer;
  padding: 8px 16px; border-radius: 8px; transition: all 0.25s; font-family: inherit;
}
.btn-logout:hover { background: #ef4444; color: #fff; }

.user-avatar-nav {
  width: 36px; height: 36px; border-radius: 50%;
  background: linear-gradient(135deg, #42a5f5, #0077b6);
  color: #fff; font-size: 15px; font-weight: 700;
  display: flex; align-items: center; justify-content: center;
  cursor: default; border: 2px solid rgba(66,165,245,0.4);
  flex-shrink: 0;
}
.mobile-user-info {
  display: flex; align-items: center; gap: 12px;
  padding: 12px 16px; border-radius: 10px;
  background: rgba(66,165,245,0.08); border: 1px solid rgba(66,165,245,0.15);
}
.mobile-avatar {
  width: 34px; height: 34px; border-radius: 50%;
  background: linear-gradient(135deg, #42a5f5, #0077b6);
  color: #fff; font-size: 14px; font-weight: 700;
  display: flex; align-items: center; justify-content: center; flex-shrink: 0;
}
.mobile-user-info span { font-size: 15px; color: #fff; font-weight: 500; }

.mobile-toggle {
  display: none; flex-direction: column; justify-content: center;
  gap: 5px; background: none; border: none; cursor: pointer;
  padding: 6px; width: 38px; height: 38px;
}
.mobile-toggle span {
  width: 22px; height: 2px; background: rgba(255,255,255,0.85);
  border-radius: 2px; transition: all 0.3s ease; transform-origin: center;
}
.mobile-toggle.active span:nth-child(1) { transform: rotate(45deg) translate(5px, 5px); }
.mobile-toggle.active span:nth-child(2) { opacity: 0; transform: scaleX(0); }
.mobile-toggle.active span:nth-child(3) { transform: rotate(-45deg) translate(5px, -5px); }

.mobile-menu {
  display: none; flex-direction: column;
  padding: 16px 24px 24px; gap: 4px;
  border-top: 1px solid rgba(255,255,255,0.06);
  background: rgba(11, 28, 44, 0.98);
  transform: translateY(-10px); opacity: 0;
  pointer-events: none; transition: all 0.3s ease;
}
.mobile-menu.open { transform: translateY(0); opacity: 1; pointer-events: all; }
.mobile-menu a {
  color: rgba(255,255,255,0.8); text-decoration: none;
  font-size: 16px; font-weight: 500;
  padding: 12px 16px; border-radius: 10px; transition: all 0.2s;
}
.mobile-menu a:hover, .mobile-menu a.router-link-active { color: #fff; background: rgba(66, 165, 245, 0.1); }
.mobile-actions { display: flex; flex-direction: column; gap: 8px; margin-top: 12px; padding-top: 16px; border-top: 1px solid rgba(255,255,255,0.06); }
.mobile-actions button {
  background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1);
  color: white; font-size: 15px; font-weight: 500;
  padding: 12px; border-radius: 10px; cursor: pointer; font-family: inherit; transition: all 0.25s;
}
.mobile-actions button:hover { background: rgba(66, 165, 245, 0.15); border-color: #42a5f5; color: #42a5f5; }
.mobile-download {
  background: linear-gradient(135deg, #0077b6, #0096c7) !important;
  color: #fff !important; text-align: center;
  padding: 12px !important; border-radius: 10px; font-weight: 600 !important;
}

/* AUTH MODAL */
.modal-overlay {
  position: fixed; inset: 0;
  background: rgba(0,0,0,0.8); backdrop-filter: blur(8px);
  display: flex; align-items: center; justify-content: center;
  z-index: 2000; padding: 20px;
}
.auth-modal {
  background: linear-gradient(145deg, #16293d, #0f1f30);
  width: 100%; max-width: 440px;
  border-radius: 28px; padding: 40px; position: relative;
  border: 1px solid rgba(66, 165, 245, 0.15);
  box-shadow: 0 25px 60px rgba(0,0,0,0.6), 0 0 0 1px rgba(255,255,255,0.04);
}
.close-btn {
  position: absolute; top: 18px; right: 18px;
  width: 36px; height: 36px;
  background: rgba(255,255,255,0.07); border: 1px solid rgba(255,255,255,0.1);
  border-radius: 10px; color: rgba(255,255,255,0.7); cursor: pointer;
  font-size: 14px; transition: all 0.2s;
  display: flex; align-items: center; justify-content: center;
}
.close-btn:hover { background: rgba(239, 68, 68, 0.2); border-color: #ef4444; color: #ef4444; }
.modal-tabs {
  display: flex; background: rgba(0,0,0,0.25);
  border-radius: 14px; padding: 5px; margin-bottom: 32px;
  border: 1px solid rgba(255,255,255,0.06);
}
.modal-tabs button {
  flex: 1; padding: 11px; border: none; background: none;
  color: rgba(255,255,255,0.5); font-size: 14px; font-weight: 600;
  cursor: pointer; border-radius: 10px; transition: all 0.25s; font-family: inherit;
}
.modal-tabs button.active { background: #42a5f5; color: #fff; box-shadow: 0 4px 15px rgba(66, 165, 245, 0.3); }
.form-header { margin-bottom: 28px; }
.form-header h2 { font-size: 26px; font-weight: 800; color: #fff; margin-bottom: 6px; }
.form-header p { font-size: 14px; color: rgba(255,255,255,0.5); }
.form-group { margin-bottom: 18px; }
.form-group label { display: block; font-size: 13px; font-weight: 600; color: rgba(255,255,255,0.7); margin-bottom: 8px; letter-spacing: 0.3px; }
.input-wrap { position: relative; display: flex; align-items: center; }
.input-wrap > i:first-child { position: absolute; left: 15px; color: rgba(255,255,255,0.3); font-size: 14px; pointer-events: none; }
.input-wrap input {
  width: 100%; padding: 13px 44px 13px 40px;
  background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1);
  border-radius: 12px; color: #fff; font-size: 14.5px;
  transition: all 0.25s; font-family: inherit;
}
.input-wrap input::placeholder { color: rgba(255,255,255,0.25); }
.input-wrap input:focus {
  outline: none; border-color: #42a5f5;
  background: rgba(66, 165, 245, 0.05);
  box-shadow: 0 0 0 3px rgba(66, 165, 245, 0.12);
}
.toggle-pass { position: absolute; right: 14px; background: none; border: none; color: rgba(255,255,255,0.3); cursor: pointer; font-size: 14px; padding: 4px; transition: color 0.2s; }
.toggle-pass:hover { color: rgba(255,255,255,0.7); }
.status-msg { display: flex; align-items: center; gap: 8px; font-size: 13px; padding: 12px 16px; border-radius: 10px; margin-bottom: 16px; }
.status-msg.error { background: rgba(239, 68, 68, 0.1); border: 1px solid rgba(239, 68, 68, 0.25); color: #fca5a5; }
.status-msg.success { background: rgba(34, 197, 94, 0.1); border: 1px solid rgba(34, 197, 94, 0.25); color: #86efac; }
.submit-btn {
  width: 100%; padding: 14px;
  background: linear-gradient(135deg, #42a5f5, #0077b6);
  border: none; border-radius: 12px; color: #fff;
  font-size: 15px; font-weight: 700; cursor: pointer;
  transition: all 0.3s; font-family: inherit; letter-spacing: 0.3px; margin-bottom: 20px;
}
.submit-btn:hover:not(:disabled) { transform: translateY(-2px); box-shadow: 0 8px 25px rgba(0, 119, 182, 0.4); }
.submit-btn:disabled { opacity: 0.6; cursor: not-allowed; }
.submit-btn.signup { background: linear-gradient(135deg, #0077b6, #005f8f); }
.switch-text { text-align: center; font-size: 13px; color: rgba(255,255,255,0.45); }
.switch-text button { background: none; border: none; color: #42a5f5; cursor: pointer; font-weight: 600; font-size: 13px; font-family: inherit; margin-left: 4px; }
.switch-text button:hover { text-decoration: underline; }

/* FOOTER */
.footer { background: #060f18; border-top: 1px solid rgba(255,255,255,0.06); padding: 60px 40px 0; margin-top: auto; }
.footer-container { max-width: 1400px; margin: 0 auto; display: flex; justify-content: space-between; gap: 60px; flex-wrap: wrap; padding-bottom: 50px; }
.footer-brand { display: flex; flex-direction: column; gap: 10px; max-width: 260px; }
.footer-brand img { width: 42px; border-radius: 10px; }
.footer-brand span { font-size: 22px; font-weight: 800; color: #42a5f5; }
.footer-brand p { font-size: 14px; color: rgba(255,255,255,0.45); line-height: 1.6; }
.footer-links { display: flex; gap: 60px; flex-wrap: wrap; }
.footer-col { display: flex; flex-direction: column; gap: 12px; }
.footer-col h4 { font-size: 12px; font-weight: 700; text-transform: uppercase; letter-spacing: 1.5px; color: rgba(255,255,255,0.4); margin-bottom: 4px; }
.footer-col a { color: rgba(255,255,255,0.65); text-decoration: none; font-size: 14px; transition: color 0.25s; display: flex; align-items: center; gap: 8px; }
.footer-col a:hover { color: #42a5f5; }
.footer-bottom { border-top: 1px solid rgba(255,255,255,0.06); padding: 20px 0; text-align: center; max-width: 1400px; margin: 0 auto; }
.footer-bottom p { font-size: 13px; color: rgba(255,255,255,0.3); }
.footer-bottom .fa-heart { color: #ef4444; font-size: 11px; }

/* TRANSITIONS */
.fade-enter-active, .fade-leave-active { transition: opacity 0.3s ease; }
.fade-enter-from, .fade-leave-to { opacity: 0; }
.modal-fade-enter-active, .modal-fade-leave-active { transition: all 0.3s ease; }
.modal-fade-enter-from, .modal-fade-leave-to { opacity: 0; transform: scale(0.96); }

/* RESPONSIVE */
@media (max-width: 900px) {
  .nav-links, .nav-actions { display: none; }
  .mobile-toggle { display: flex; }
  .mobile-menu { display: flex; }
}
@media (max-width: 600px) {
  .nav-container { padding: 0 20px; }
  .auth-modal { padding: 30px 24px; }
  .footer { padding: 40px 24px 0; }
  .footer-container { flex-direction: column; gap: 32px; }
  .footer-links { gap: 32px; }
}
</style>
