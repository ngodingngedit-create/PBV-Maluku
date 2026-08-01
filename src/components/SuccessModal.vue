<script setup>
import { ref } from 'vue'

const props = defineProps({
  show: {
    type: Boolean,
    default: false
  },
  data: {
    type: Object,
    default: null
  }
})

const emit = defineEmits(['close'])

const formatPrice = (val) => {
  return val ? val.toLocaleString('id-ID') : '0'
}
</script>

<template>
  <transition name="modal-fade">
    <div v-if="show" class="modal-backdrop" @click.self="emit('close')">
      <div class="modal-card">
        <div class="success-icon-badge">
          <svg fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M5 13l4 4L19 7" />
          </svg>
        </div>

        <h3 class="modal-title">Pendaftaran & Pembayaran Berhasil!</h3>
        <p class="modal-desc">
          Tiket elektronik (E-Ticket) PBV Maluku telah dikirimkan ke email pemesan <strong>{{ data?.buyer?.email }}</strong>.
        </p>

        <div class="summary-box">
          <div class="info-row">
            <span>Nama Pemesan:</span>
            <strong>{{ data?.buyer?.fullName }}</strong>
          </div>
          <div class="info-row">
            <span>No Telepon:</span>
            <strong>{{ data?.buyer?.countryCode }} {{ data?.buyer?.phone }}</strong>
          </div>
          <div class="info-row">
            <span>Jumlah Tiket:</span>
            <strong>{{ data?.holders?.length }} Tiket</strong>
          </div>
          <div class="info-row total">
            <span>Total Pembayaran:</span>
            <strong class="price">Rp {{ formatPrice(data?.totalPayment) }}</strong>
          </div>
        </div>

        <button class="btn-close-modal" @click="emit('close')">
          Tutup & Kembali ke Beranda
        </button>
      </div>
    </div>
  </transition>
</template>

<style scoped>
.modal-backdrop {
  position: fixed;
  inset: 0;
  background-color: rgba(0, 0, 0, 0.8);
  backdrop-filter: blur(8px);
  z-index: 9999;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1.5rem;
}

.modal-card {
  background-color: #181822;
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 20px;
  width: 100%;
  max-width: 480px;
  padding: 2rem;
  text-align: center;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.8);
}

.success-icon-badge {
  width: 64px;
  height: 64px;
  background-color: rgba(16, 185, 129, 0.15);
  border: 2px solid #10b981;
  border-radius: 50%;
  color: #10b981;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 1.25rem;
}

.success-icon-badge svg {
  width: 32px;
  height: 32px;
}

.modal-title {
  font-size: 1.35rem;
  font-weight: 800;
  color: #ffffff;
  margin-bottom: 0.5rem;
}

.modal-desc {
  font-size: 0.9rem;
  color: #9ca3af;
  line-height: 1.5;
  margin-bottom: 1.5rem;
}

.summary-box {
  background-color: #111118;
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 12px;
  padding: 1rem 1.25rem;
  margin-bottom: 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  text-align: left;
}

.info-row {
  display: flex;
  justify-content: space-between;
  font-size: 0.88rem;
  color: #9ca3af;
}

.info-row strong {
  color: #ffffff;
}

.info-row.total {
  border-top: 1px solid rgba(255, 255, 255, 0.08);
  padding-top: 0.65rem;
  font-weight: 700;
}

.info-row .price {
  color: #f59e0b;
}

.btn-close-modal {
  width: 100%;
  background-color: #ffffff;
  color: #000000;
  border: none;
  border-radius: 10px;
  padding: 0.85rem;
  font-size: 0.95rem;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.2s ease;
}

.btn-close-modal:hover {
  background-color: #e5e7eb;
}

.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.3s ease;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}
</style>
