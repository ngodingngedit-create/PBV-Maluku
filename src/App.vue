<script setup>
import { ref, watch } from 'vue'
import Navbar from './components/Navbar.vue'
import HomePage from './components/HomePage.vue'
import HeroSection from './components/HeroSection.vue'
import PersonalInfoForm from './components/PersonalInfoForm.vue'
import InvoicePage from './components/InvoicePage.vue'
import BlogDetailPage from './components/BlogDetailPage.vue'

// Import Hero Image Asset
import heroImg from './assets/images/hero.png'

// Current Page state: 'home', 'tickets', 'invoice', or 'blog-detail'
const currentPage = ref('home')
const selectedArticle = ref(null)

// Navigation helper
const handleNavigate = (page, payload = null) => {
  currentPage.value = page
  if (payload) {
    selectedArticle.value = payload
  }
}

// Scroll to top on page navigation
watch(currentPage, () => {
  window.scrollTo({ top: 0, behavior: 'instant' })
})

// Sample Ticket Data
const ticketTypes = ref([
  { id: 'vVIP', name: 'VIP REGULAR PASS', price: 150000, qty: 1 }
])

const submissionResult = ref(null)

const handleFormSuccess = (payload) => {
  submissionResult.value = payload
  // Navigate directly to dedicated Invoice Page (no popup modal)
  currentPage.value = 'invoice'
}

const handleSidebarToggle = (isOpen) => {
  if (isOpen) {
    document.body.classList.add('sidebar-open')
  } else {
    document.body.classList.remove('sidebar-open')
  }
}
</script>

<template>
  <div class="app-root">
    <!-- Header Navigation — hidden on invoice page -->
    <Navbar
      v-if="currentPage !== 'invoice'"
      :activePage="currentPage"
      :forceWhite="currentPage === 'blog-detail'"
      @navigate="handleNavigate"
      @sidebar-toggle="handleSidebarToggle"
    />

    <main :class="currentPage !== 'invoice' ? 'main-content-padded' : 'main-invoice-padded'">
      <!-- Conditional Page Rendering -->
      <template v-if="currentPage === 'home'">
        <HomePage @navigate="handleNavigate" />
      </template>
      
      <template v-else-if="currentPage === 'tickets'">
        <!-- Hero Section -->
        <section id="hero">
          <HeroSection
            eventTitle="PBV MALUKU"
            :heroImage="heroImg"
            eventDate="5 Jun 2026"
            eventTime="19:30 - 23:30 WIT"
            eventLocation="GOR Ghabata, Ambon, Maluku"
            organizerName="PBV MALUKU OFFICIAL"
          />
        </section>

        <!-- Personal Information Form Section -->
        <section id="info">
          <PersonalInfoForm
            :ticketTypes="ticketTypes"
            :serviceFee="10000"
            @submit-success="handleFormSuccess"
          />
        </section>
      </template>

      <!-- DEDICATED INVOICE PAGE (NO POPUP) -->
      <template v-else-if="currentPage === 'invoice'">
        <InvoicePage
          :data="submissionResult"
          @navigate="handleNavigate"
        />
      </template>

      <!-- DEDICATED BLOG DETAIL PAGE -->
      <template v-else-if="currentPage === 'blog-detail'">
        <BlogDetailPage
          :article="selectedArticle"
          @navigate="handleNavigate"
        />
      </template>
    </main>
  </div>
</template>

<style>
/* Pastikan html dan body mendukung sticky positioning tanpa memicu overflow clip */
html, body {
  margin: 0;
  padding: 0;
  background-color: #f8fafc;
  color: #0f172a;
  font-family: 'Inter', system-ui, -apple-system, sans-serif;
  overflow-x: hidden;
}

.app-root {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background-color: #f8fafc;
}

.main-content-padded {
  padding-top: 68px; /* Offset persis untuk header navbar fixed */
  flex-grow: 1;
}

/* Invoice page: no navbar offset needed */
.main-invoice-padded {
  padding-top: 0;
  flex-grow: 1;
}

@media (max-width: 860px) {
  .main-content-padded {
    padding-top: 58px;
    /* Prevent content hiding behind fixed bottom tab bar */
    padding-bottom: 62px;
  }
}
</style>
