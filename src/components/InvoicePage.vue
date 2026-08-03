<script setup>
import MemberInvoiceCard from './MemberInvoiceCard.vue'

const props = defineProps({
  data: {
    type: Object,
    default: null
  }
})

const emit = defineEmits(['navigate'])
const goHome = () => emit('navigate', 'home')
</script>

<template>
  <div class="invoice-page">

    <!-- Back button (hidden on print) -->
    <div class="top-nav no-print">
      <button class="btn-back" @click="goHome">
        <svg fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M15 19l-7-7 7-7" />
        </svg>
        Kembali ke Beranda
      </button>
    </div>

    <!-- MAIN CONTENT (putih) -->
    <section class="main-section">
      <MemberInvoiceCard :data="data" />
    </section>

  </div>
</template>

<style scoped>
/* ── Page root ─────────────────────────────────────────────── */
.invoice-page {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background: #ffffff;
}

/* ── Top Nav ────────────────────────────────────────────────── */
.top-nav {
  width: 100%;
  max-width: 794px;
  margin: 0 auto;
  padding: 1.5rem 1rem 0;
  display: flex;
  justify-content: flex-start;
}

/* Back button on white background */
.btn-back {
  display: flex;
  align-items: center;
  gap: 0.35rem;
  background: #ffffff;
  color: #0f172a;
  border: 1px solid #e2e8f0;
  padding: 0.55rem 1.2rem;
  border-radius: 8px;
  font-size: 0.88rem;
  font-weight: 700;
  cursor: pointer;
  box-shadow: 0 1px 3px rgba(0,0,0,0.05);
  transition: all 0.2s ease;
}

.btn-back svg {
  width: 16px;
  height: 16px;
}

.btn-back:hover {
  background: #f8fafc;
  color: #e10600;
  border-color: #cbd5e1;
  transform: translateX(-2px);
}

/* ══ MAIN CONTENT SECTION ══════════════════════════════════ */
.main-section {
  background: #ffffff;
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 1.5rem 1rem 4rem;
}

/* ── PRINT ─────────────────────────────────────────────────── */
@media print {
  .no-print { display: none !important; }

  .invoice-page {
    display: block;
    background: #fff;
  }

  .main-section {
    padding: 0;
    display: block;
  }
}
</style>
