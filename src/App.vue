<script setup>
import { ref } from 'vue'
import Navbar from './components/Navbar.vue'
import HeroSection from './components/HeroSection.vue'
import PersonalInfoForm from './components/PersonalInfoForm.vue'
import SuccessModal from './components/SuccessModal.vue'

// Import Hero Image Asset
import heroImg from './assets/images/hero.png'

// Sample Ticket Data
const ticketTypes = ref([
  { id: 'vVIP', name: 'VIP REGULAR PASS', price: 150000, qty: 1 }
])

// Modal state
const isSuccessModalOpen = ref(false)
const submissionResult = ref(null)

const handleFormSuccess = (payload) => {
  submissionResult.value = payload
  isSuccessModalOpen.value = true
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
    <!-- Header Navigation Sticky Stay -->
    <Navbar @sidebar-toggle="handleSidebarToggle" />

    <main class="main-content-padded">
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
    </main>

    <!-- Success Modal -->
    <SuccessModal
      :show="isSuccessModalOpen"
      :data="submissionResult"
      @close="isSuccessModalOpen = false"
    />
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

@media (max-width: 860px) {
  .main-content-padded {
    padding-top: 58px;
  }
}
</style>
