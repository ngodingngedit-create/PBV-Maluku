<script setup>
import { ref, watch, onMounted, onUnmounted } from 'vue'
import logoImg from '../assets/images/M (1).png'

const props = defineProps({
  activePage: {
    type: String,
    default: 'home'
  },
  forceWhite: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['sidebar-toggle', 'navigate'])

const isMobileMenuOpen = ref(false)
const isScrolledPastHero = ref(false)
const localActiveTab = ref(props.activePage)

watch(isMobileMenuOpen, (newVal) => {
  emit('sidebar-toggle', newVal)
})

watch(() => props.activePage, (newVal) => {
  if (newVal === 'blog-detail') {
    localActiveTab.value = 'blog'
  } else {
    localActiveTab.value = newVal
  }
})

const handleScroll = () => {
  const heroEl = document.getElementById('hero')
  if (heroEl) {
    const heroBottom = heroEl.getBoundingClientRect().bottom
    isScrolledPastHero.value = heroBottom <= 70
  } else {
    isScrolledPastHero.value = window.scrollY > 100
  }

  // Active section tracking on scroll when on home page
  if (props.activePage === 'home') {
    const resultsEl = document.querySelector('.results-section')
    const blogEl = document.querySelector('.blog-section')
    const scrollPos = window.scrollY + window.innerHeight / 3

    if (blogEl && scrollPos >= blogEl.offsetTop) {
      localActiveTab.value = 'blog'
    } else if (resultsEl && scrollPos >= resultsEl.offsetTop) {
      localActiveTab.value = 'pertandingan'
    } else {
      localActiveTab.value = 'home'
    }
  }
}

const navigateTo = (page, selector = null) => {
  emit('navigate', page)
  
  if (page === 'tickets') {
    localActiveTab.value = 'tickets'
  } else if (page === 'blog-detail') {
    localActiveTab.value = 'blog'
  } else if (selector === '.blog-section') {
    localActiveTab.value = 'blog'
  } else if (selector === '.results-section') {
    localActiveTab.value = 'pertandingan'
  } else if (page === 'home' && !selector) {
    localActiveTab.value = 'home'
  }

  if (selector) {
    setTimeout(() => {
      const el = document.querySelector(selector)
      if (el) {
        el.scrollIntoView({ behavior: 'smooth' })
      }
    }, 150)
  } else {
    window.scrollTo({ top: 0, behavior: 'smooth' })
  }
  isMobileMenuOpen.value = false
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
  <!-- TOP HEADER -->
  <header class="site-header" :class="{ 'scrolled-white': isScrolledPastHero || props.forceWhite }">
    <div class="header-container">
      <!-- Logo -->
      <div class="brand-logo-container">
        <a href="#" @click.prevent="navigateTo('home')" class="logo-link">
          <img :src="logoImg" alt="PBV MALUKU Logo" class="logo-image-only" />
        </a>
      </div>

      <!-- Desktop Navigation -->
      <nav class="desktop-nav">
        <a href="#" @click.prevent="navigateTo('home')" class="nav-link" :class="{ active: activePage === 'home' }">Beranda</a>
        <a href="#" @click.prevent="navigateTo('tickets', '#info')" class="nav-link" :class="{ active: activePage === 'tickets' }">Informasi</a>
        <a href="#" @click.prevent="navigateTo('home', '.results-section')" class="nav-link">Jadwal Pertandingan</a>
        <a href="#" @click.prevent="navigateTo('home', '.footer-section')" class="nav-link">Kontak</a>
      </nav>

      <!-- Right side: CTA (desktop) + Hamburger (mobile) -->
      <div class="header-action-group">
        <button class="btn-header-primary desktop-cta" @click="navigateTo('tickets', '#info')">
          Daftar Sekarang
        </button>

        <!-- Mobile Hamburger Button -->
        <button class="mobile-toggle" @click="isMobileMenuOpen = !isMobileMenuOpen" title="Menu">
          <svg fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path v-if="!isMobileMenuOpen" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
            <path v-else stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>
    </div>

    <!-- Mobile Drawer Overlay -->
    <transition name="fade">
      <div v-if="isMobileMenuOpen" class="mobile-drawer-overlay" @click="isMobileMenuOpen = false"></div>
    </transition>

    <!-- Mobile Sidebar Drawer -->
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
          <a href="#" class="mobile-link" :class="{ active: activePage === 'home' }" @click.prevent="navigateTo('home')">Beranda</a>
          <a href="#" class="mobile-link" :class="{ active: activePage === 'tickets' }" @click.prevent="navigateTo('tickets', '#info')">Informasi</a>
          <a href="#" class="mobile-link" @click.prevent="navigateTo('home', '.results-section')">Jadwal Pertandingan</a>
          <a href="#" class="mobile-link" @click.prevent="navigateTo('home', '.footer-section')">Kontak</a>
        </div>

        <div class="sidebar-footer">
          <button class="btn-header-primary mobile-btn" @click="navigateTo('tickets', '#info')">Daftar Sekarang</button>
        </div>
      </div>
    </transition>
  </header>

  <!-- MOBILE BOTTOM TAB BAR — hidden when sidebar is open -->
  <nav class="mobile-bottom-nav" :class="{ 'bottom-nav-hidden': isMobileMenuOpen }">
    <!-- Beranda -->
    <button class="bottom-tab" :class="{ active: localActiveTab === 'home' }" @click="navigateTo('home')">
      <span class="tab-indicator"></span>
      <svg class="tab-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path>
        <polyline points="9 22 9 12 15 12 15 22"></polyline>
      </svg>
      <span class="tab-label">Beranda</span>
    </button>

    <!-- Blog — scroll to blog section on homepage -->
    <button class="bottom-tab" :class="{ active: localActiveTab === 'blog' }" @click="navigateTo('home', '.blog-section')">
      <span class="tab-indicator"></span>
      <svg class="tab-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
        <polyline points="14 2 14 8 20 8"></polyline>
        <line x1="16" y1="13" x2="8" y2="13"></line>
        <line x1="16" y1="17" x2="8" y2="17"></line>
        <polyline points="10 9 9 9 8 9"></polyline>
      </svg>
      <span class="tab-label">Blog</span>
    </button>

    <!-- Pertandingan -->
    <button class="bottom-tab" :class="{ active: localActiveTab === 'pertandingan' }" @click="navigateTo('home', '.results-section')">
      <span class="tab-indicator"></span>
      <svg class="tab-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <path d="M6 9H4.5a2.5 2.5 0 0 1 0-5H6"></path>
        <path d="M18 9h1.5a2.5 2.5 0 0 0 0-5H18"></path>
        <path d="M4 22h16"></path>
        <path d="M10 14.66V17c0 .55-.47.98-.97 1.21C7.85 18.75 7 20.24 7 22"></path>
        <path d="M14 14.66V17c0 .55.47.98.97 1.21C16.15 18.75 17 20.24 17 22"></path>
        <path d="M18 2H6v7a6 6 0 0 0 12 0V2Z"></path>
      </svg>
      <span class="tab-label">Pertandingan</span>
    </button>

    <!-- Daftar -->
    <button class="bottom-tab" :class="{ active: localActiveTab === 'tickets' }" @click="navigateTo('tickets')">
      <span class="tab-indicator"></span>
      <svg class="tab-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path>
        <circle cx="9" cy="7" r="4"></circle>
        <line x1="19" y1="8" x2="19" y2="14"></line>
        <line x1="22" y1="11" x2="16" y2="11"></line>
      </svg>
      <span class="tab-label">Daftar</span>
    </button>
  </nav>
</template>

<style scoped>
/* ─── TOP HEADER ──────────────────────────────────────────── */
.site-header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1000;
  background-color: rgba(15, 44, 89, 0.45);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
  box-shadow: none;
  transition: background-color 0.3s ease, border-color 0.3s ease, box-shadow 0.3s ease;
}

.site-header.scrolled-white {
  background-color: rgba(255, 255, 255, 0.96);
  border-bottom: 1px solid #e2e8f0;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.06);
}

.site-header.scrolled-white .nav-link { color: #475569; }
.site-header.scrolled-white .nav-link:hover,
.site-header.scrolled-white .nav-link.active { color: #0f172a; }
.site-header.scrolled-white .nav-link.active { color: #0f172a !important; }
.site-header.scrolled-white .nav-link::after { background-color: #2563eb; }
.site-header.scrolled-white .mobile-toggle { color: #0f172a; }

.header-container {
  max-width: 1540px;
  margin: 0 auto;
  padding: 0.85rem 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.brand-logo-container { display: flex; align-items: center; }

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

.desktop-nav { display: flex; gap: 2.2rem; }

.nav-link {
  color: #cbd5e1;
  text-decoration: none;
  font-size: 0.92rem;
  font-weight: 600;
  position: relative;
  padding-bottom: 0.35rem;
  transition: color 0.2s ease;
}

.nav-link::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 2px;
  background-color: #ffffff;
  transition: width 0.2s ease;
}

.nav-link:hover::after,
.nav-link.active::after { width: 100%; }

.nav-link:hover { color: #ffffff; }
.nav-link.active { color: #ffffff; }

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
  padding: 0.2rem;
  line-height: 0;
}

.mobile-toggle svg {
  width: 26px;
  height: 26px;
}

/* ─── MOBILE SIDEBAR DRAWER ───────────────────────────────── */
.mobile-drawer-overlay {
  position: fixed;
  inset: 0;
  background-color: rgba(15, 23, 42, 0.55);
  backdrop-filter: blur(3px);
  z-index: 1050;
}

.mobile-sidebar-drawer {
  position: fixed;
  top: 0;
  right: 0;
  width: 270px;
  height: 100vh;
  background-color: #ffffff;
  z-index: 1100;
  display: flex;
  flex-direction: column;
  box-shadow: -8px 0 28px rgba(0, 0, 0, 0.12);
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
  height: 36px;
  object-fit: contain;
}

.btn-close-sidebar {
  background: none;
  border: none;
  color: #64748b;
  cursor: pointer;
  padding: 0.2rem;
  display: flex;
  align-items: center;
}

.btn-close-sidebar svg { width: 22px; height: 22px; }

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
  font-size: 0.95rem;
  font-weight: 600;
  transition: color 0.2s ease;
}

.mobile-link:hover,
.mobile-link.active { color: #2563eb; }

.sidebar-footer {
  padding-top: 1rem;
  border-top: 1px solid #f1f5f9;
}

.mobile-btn {
  width: 100%;
  text-align: center;
  padding: 0.75rem 1rem;
}

/* ─── MOBILE BOTTOM TAB BAR ───────────────────────────────── */
.mobile-bottom-nav { display: none; }

/* ─── ANIMATIONS ──────────────────────────────────────────── */
.fade-enter-active, .fade-leave-active { transition: opacity 0.3s ease; }
.fade-enter-from, .fade-leave-to { opacity: 0; }

.slide-sidebar-enter-active,
.slide-sidebar-leave-active { transition: transform 0.3s cubic-bezier(0.16, 1, 0.3, 1); }
.slide-sidebar-enter-from,
.slide-sidebar-leave-to { transform: translateX(100%); }

/* ─── RESPONSIVE ──────────────────────────────────────────── */
@media (max-width: 860px) {
  /* Desktop nav hidden, show hamburger */
  .desktop-nav { display: none; }
  .desktop-cta { display: none; }
  .mobile-toggle { display: block; }

  .header-container {
    padding: 0.65rem 1rem;
    justify-content: space-between;
  }
  .logo-image-only { height: 38px; }

  /* Show bottom tab bar */
  .mobile-bottom-nav {
    display: flex;
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    z-index: 1001;
    background-color: #ffffff;
    border-top: 1px solid #e2e8f0;
    box-shadow: 0 -2px 16px rgba(0, 0, 0, 0.07);
    border-radius: 16px 16px 0 0;
    padding-bottom: env(safe-area-inset-bottom, 0px);
    transition: transform 0.35s cubic-bezier(0.4, 0, 0.2, 1), opacity 0.3s ease;
    transform: translateY(0);
    opacity: 1;
  }

  /* Hidden state when sidebar is open */
  .mobile-bottom-nav.bottom-nav-hidden {
    transform: translateY(100%);
    opacity: 0;
    pointer-events: none;
  }

  .bottom-tab {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    padding: 0 0.2rem 0.65rem;
    background: none;
    border: none;
    cursor: pointer;
    position: relative;
    gap: 0.22rem;
    color: #94a3b8;
    transition: color 0.25s ease;
    -webkit-tap-highlight-color: transparent;
    outline: none;
  }

  /* Active indicator line at top of tab */
  .tab-indicator {
    width: 36px;
    height: 2.5px;
    border-radius: 0 0 3px 3px;
    background-color: #2563eb;
    margin-bottom: 0.38rem;
    transform: scaleX(0);
    transform-origin: center;
    transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
    flex-shrink: 0;
  }

  .bottom-tab.active .tab-indicator { transform: scaleX(1); }

  .tab-icon {
    width: 21px;
    height: 21px;
    flex-shrink: 0;
    transition: color 0.25s ease;
  }

  .tab-label {
    font-size: 0.6rem;
    font-weight: 600;
    letter-spacing: 0.15px;
    line-height: 1;
    transition: color 0.25s ease;
  }

  .bottom-tab.active { color: #2563eb; }
}
</style>
