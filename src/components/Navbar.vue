<script setup>
import { ref, watch, onMounted, onUnmounted } from 'vue'
import logoImg from '../assets/images/M (1).png'

const emit = defineEmits(['sidebar-toggle'])

const isMobileMenuOpen = ref(false)
const isScrolledPastHero = ref(false)

watch(isMobileMenuOpen, (newVal) => {
  emit('sidebar-toggle', newVal)
})

const handleScroll = () => {
  const heroEl = document.getElementById('hero')
  if (heroEl) {
    const heroBottom = heroEl.getBoundingClientRect().bottom
    isScrolledPastHero.value = heroBottom <= 70
  } else {
    isScrolledPastHero.value = window.scrollY > 400
  }
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll, { passive: true })
  handleScroll()
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<template>
  <header class="site-header" :class="{ 'scrolled-white': isScrolledPastHero }">
    <div class="header-container">
      <!-- Logo Image Import -->
      <div class="brand-logo-container">
        <a href="#hero" class="logo-link">
          <img :src="logoImg" alt="PBV MALUKU Logo" class="logo-image-only" />
        </a>
      </div>

      <!-- Desktop Navigation -->
      <nav class="desktop-nav">
        <a href="#hero" class="nav-link active">Home</a>
        <a href="#info" class="nav-link">Informasi</a>
        <a href="#jadwal" class="nav-link">Jadwal Pertandingan</a>
        <a href="#kontak" class="nav-link">Kontak</a>
      </nav>

      <!-- Header Action Button (Warna Biru Tanpa Shadow Berlebih) -->
      <div class="header-action-group">
        <button class="btn-header-primary">
          Daftar Sekarang
        </button>
        
        <!-- Mobile Hamburger Button -->
        <button class="mobile-toggle" @click="isMobileMenuOpen = !isMobileMenuOpen" title="Toggle Navigation">
          <svg fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path v-if="!isMobileMenuOpen" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
            <path v-else stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>
    </div>

    <!-- Mobile Drawer Overlay & Sidebar -->
    <transition name="fade">
      <div v-if="isMobileMenuOpen" class="mobile-drawer-overlay" @click="isMobileMenuOpen = false"></div>
    </transition>

    <transition name="slide-sidebar">
      <div v-if="isMobileMenuOpen" class="mobile-sidebar-drawer">
        <div class="sidebar-header">
          <img :src="logoImg" alt="PBV MALUKU Logo" class="sidebar-logo" />
          <button class="btn-close-sidebar" @click="isMobileMenuOpen = false">
            <svg fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
        </div>

        <div class="sidebar-menu-links">
          <a href="#hero" class="mobile-link" @click="isMobileMenuOpen = false">Home</a>
          <a href="#info" class="mobile-link" @click="isMobileMenuOpen = false">Informasi</a>
          <a href="#jadwal" class="mobile-link" @click="isMobileMenuOpen = false">Jadwal Pertandingan</a>
          <a href="#kontak" class="mobile-link" @click="isMobileMenuOpen = false">Kontak</a>
        </div>

        <div class="sidebar-footer">
          <button class="btn-header-primary mobile-btn" @click="isMobileMenuOpen = false">Daftar Sekarang</button>
        </div>
      </div>
    </transition>
  </header>
</template>

<style scoped>
.site-header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1000;
  background-color: rgba(15, 23, 42, 0.94);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.08);
  transition: background-color 0.3s ease, border-color 0.3s ease, box-shadow 0.3s ease;
}

.site-header.scrolled-white {
  background-color: rgba(255, 255, 255, 0.96);
  border-bottom: 1px solid #e2e8f0;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.06);
}

.site-header.scrolled-white .nav-link {
  color: #475569;
}

.site-header.scrolled-white .nav-link:hover,
.site-header.scrolled-white .nav-link.active {
  color: #0f172a;
}

.site-header.scrolled-white .mobile-toggle {
  color: #0f172a;
}

.header-container {
  max-width: 1540px;
  margin: 0 auto;
  padding: 0.85rem 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.brand-logo-container {
  display: flex;
  align-items: center;
}

.logo-link {
  display: flex;
  align-items: center;
  text-decoration: none;
}

.logo-image-only {
  height: 48px;
  width: auto;
  max-width: 220px;
  object-fit: contain;
  display: block;
}

.desktop-nav {
  display: flex;
  gap: 2.2rem;
}

.nav-link {
  color: #cbd5e1;
  text-decoration: none;
  font-size: 0.92rem;
  font-weight: 600;
  transition: color 0.2s ease;
}

.nav-link:hover,
.nav-link.active {
  color: #ffffff;
}

.header-action-group {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.btn-header-primary {
  background-color: #2563eb;
  color: #ffffff;
  border: none;
  padding: 0.6rem 1.35rem;
  border-radius: 8px;
  font-size: 0.88rem;
  font-weight: 700;
  cursor: pointer;
  box-shadow: none;
  transition: background-color 0.2s ease, transform 0.2s ease;
}

.btn-header-primary:hover {
  background-color: #1d4ed8;
  transform: translateY(-1px);
}

.mobile-toggle {
  display: none;
  background: none;
  border: none;
  color: #ffffff;
  cursor: pointer;
  padding: 0.25rem;
}

.mobile-toggle svg {
  width: 28px;
  height: 28px;
}

/* MOBILE SIDEBAR DRAWER STYLING */
.mobile-drawer-overlay {
  position: fixed;
  inset: 0;
  background-color: rgba(15, 23, 42, 0.6);
  backdrop-filter: blur(4px);
  z-index: 1050;
}

.mobile-sidebar-drawer {
  position: fixed;
  top: 0;
  right: 0;
  width: 280px;
  height: 100vh;
  background-color: #ffffff;
  z-index: 1100;
  display: flex;
  flex-direction: column;
  box-shadow: -10px 0 30px rgba(0, 0, 0, 0.15);
  padding: 1.25rem 1.5rem;
}

.sidebar-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-bottom: 1.25rem;
  border-bottom: 1px solid #f1f5f9;
}

.sidebar-logo {
  height: 38px;
  object-fit: contain;
}

.btn-close-sidebar {
  background: none;
  border: none;
  color: #64748b;
  cursor: pointer;
  padding: 0.25rem;
  display: flex;
  align-items: center;
  justify-content: center;
}

.btn-close-sidebar svg {
  width: 24px;
  height: 24px;
}

.sidebar-menu-links {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
  padding: 1.75rem 0;
  flex-grow: 1;
}

.mobile-link {
  color: #1e293b;
  text-decoration: none;
  font-size: 1rem;
  font-weight: 600;
  transition: color 0.2s ease;
}

.mobile-link:hover {
  color: #2563eb;
}

.sidebar-footer {
  padding-top: 1rem;
  border-top: 1px solid #f1f5f9;
}

.mobile-btn {
  width: 100%;
  text-align: center;
  padding: 0.75rem 1rem;
}

/* Animations */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.slide-sidebar-enter-active,
.slide-sidebar-leave-active {
  transition: transform 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}
.slide-sidebar-enter-from,
.slide-sidebar-leave-to {
  transform: translateX(100%);
}

@media (max-width: 860px) {
  .desktop-nav {
    display: none;
  }
  .btn-header-primary:not(.mobile-btn) {
    display: none;
  }
  .mobile-toggle {
    display: block;
  }
  .header-container {
    padding: 0.75rem 1rem;
  }
  .logo-image-only {
    height: 40px;
  }
}
</style>
