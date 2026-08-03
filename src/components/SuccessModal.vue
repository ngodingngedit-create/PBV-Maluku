<script setup>
import { ref } from 'vue'
import MemberInvoiceCard from './MemberInvoiceCard.vue'

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
      <div class="modal-card wide-modal">
        <!-- Top Close Button -->
        <button class="btn-x-close no-print" @click="emit('close')">✕</button>

        <div class="success-header no-print">
          <div class="success-icon-badge">
            <svg fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M5 13l4 4L19 7" />
            </svg>
          </div>

          <h3 class="modal-title">Pendaftaran Atlet PBV. MALUKU Berhasil!</h3>
          <p class="modal-desc">
            Kartu Anggota Resmi & Bukti Pendaftaran telah diterbitkan untuk <strong>{{ data?.buyer?.fullName }}</strong> ({{ data?.buyer?.email }}).
          </p>
        </div>

        <!-- Official 2-Sided Member Invoice Card Component -->
        <MemberInvoiceCard :data="data" />

        <!-- Bottom Action -->
        <div class="modal-footer-actions no-print">
          <button class="btn-close-modal" @click="emit('close')">
            Selesai & Kembali ke Beranda
          </button>
        </div>
      </div>
    </div>
  </transition>
</template>

<style scoped>
.modal-backdrop {
  position: fixed;
  inset: 0;
  background-color: rgba(0, 0, 0, 0.85);
  backdrop-filter: blur(8px);
  z-index: 9999;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  padding: 1.5rem 1rem;
  overflow-y: auto;
}

.modal-card.wide-modal {
  background-color: #0f172a;
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 20px;
  width: 100%;
  max-width: 820px;
  padding: 2rem 1.5rem;
  text-align: center;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.8);
  position: relative;
  margin: auto;
}

.btn-x-close {
  position: absolute;
  top: 1.25rem;
  right: 1.25rem;
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: #ffffff;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  font-size: 1rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.btn-x-close:hover {
  background: rgba(255, 255, 255, 0.25);
}

.success-icon-badge {
  width: 58px;
  height: 58px;
  background-color: rgba(16, 185, 129, 0.15);
  border: 2px solid #10b981;
  border-radius: 50%;
  color: #10b981;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 0.85rem;
}

.success-icon-badge svg {
  width: 30px;
  height: 30px;
}

.modal-title {
  font-size: 1.35rem;
  font-weight: 800;
  color: #ffffff;
  margin-bottom: 0.35rem;
}

.modal-desc {
  font-size: 0.88rem;
  color: #94a3b8;
  line-height: 1.5;
  margin-bottom: 1.25rem;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}

.modal-footer-actions {
  margin-top: 1.5rem;
  display: flex;
  justify-content: center;
}

.btn-close-modal {
  width: 100%;
  max-width: 340px;
  background-color: #ffffff;
  color: #0f172a;
  border: none;
  border-radius: 10px;
  padding: 0.85rem;
  font-size: 0.92rem;
  font-weight: 800;
  cursor: pointer;
  transition: all 0.2s ease;
}

.btn-close-modal:hover {
  background-color: #e2e8f0;
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
