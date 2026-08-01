<script setup>
const props = defineProps({
  eventTitle: {
    type: String,
    default: 'PBV MALUKU'
  },
  heroImage: {
    type: String,
    required: true
  },
  eventDate: {
    type: String,
    default: '5 Jun 2026'
  },
  eventTime: {
    type: String,
    default: '19:30 - 23:30 WIT'
  },
  eventLocation: {
    type: String,
    default: 'Gor Ghabata, Ambon, Maluku'
  },
  organizerName: {
    type: String,
    default: 'PBV MALUKU OFFICIAL'
  }
})
</script>

<template>
  <div class="hero-section">
    <!-- Hero Blurred Image Background Overlay -->
    <div 
      class="hero-blurred-bg"
      :style="{ backgroundImage: `url(${heroImage})` }"
    ></div>

    <!-- Dark Gradient Overlay for readability -->
    <div class="hero-gradient-overlay"></div>

    <div class="hero-container">
      <!-- Title Row (Positions above banner image on desktop) -->
      <div class="hero-top-row">
        <h1 class="event-title">{{ eventTitle }}</h1>
      </div>

      <div class="hero-grid">
        <!-- 1. Left: Image Banner Card -->
        <div class="banner-card">
          <img :src="heroImage" :alt="eventTitle" class="banner-image" />
        </div>

        <!-- 2. Right: Meta Info Card Stack -->
        <div class="info-sidebar">
          <div class="meta-card">
            <!-- Item 1: Tim Bola Voli -->
            <div class="meta-item">
              <svg class="meta-icon" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10" />
              </svg>
              <span class="meta-text highlight">Tim Bola Voli, PBV. Maluku - Jakarta</span>
            </div>

            <!-- Item 2: Putra & Putri -->
            <div class="meta-item">
              <svg class="meta-icon" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4.354a4 4 0 110 5.292M15 21H3v-1a6 6 0 0112 0v1zm0 0h6v-1a6 6 0 00-9-5.197M13 7a4 4 0 11-8 0 4 4 0 018 0z" />
              </svg>
              <span class="meta-text">Putra &amp; Putri</span>
            </div>

            <!-- Item 3: Gor Cilandak & Bulungan, JakSel -->
            <div class="meta-item">
              <svg class="meta-icon" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z" />
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" />
              </svg>
              <span class="meta-text normal">Gor Cilandak &amp; Bulungan, JakSel</span>
            </div>

            <!-- Item 4: Senin, Rabu s.d Sabtu -->
            <div class="meta-item">
              <svg class="meta-icon" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
              </svg>
              <span class="meta-text">Senin, Rabu s.d Sabtu</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.hero-section {
  position: relative;
  padding: 2rem 2rem;
  overflow: hidden;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  background-color: #0f172a;
}

/* Background Image Blur Effect */
.hero-blurred-bg {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  filter: blur(28px) brightness(0.45);
  transform: scale(1.1);
  z-index: 1;
}

.hero-gradient-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, rgba(15, 23, 42, 0.6) 0%, rgba(15, 23, 42, 0.85) 100%);
  z-index: 2;
}

.hero-container {
  max-width: 1540px;
  margin: 0 auto;
  position: relative;
  z-index: 3;
}

.hero-top-row {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  margin-bottom: 1.25rem;
}

.hero-grid {
  display: grid;
  grid-template-columns: 1fr 380px;
  gap: 1rem; /* Jarak antara hero image dan meta-card diperdekat */
  align-items: flex-start;
}

.banner-card {
  position: relative;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 12px 35px rgba(0, 0, 0, 0.4);
  width: 100%;
  max-height: 250px;
  background-color: rgba(2, 6, 23, 0.85);
  border: 1px solid rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(10px);
  display: flex;
  align-items: center;
  justify-content: center;
}

.banner-image {
  width: 100%;
  height: 100%;
  max-height: 250px;
  object-fit: contain;
  object-position: center;
  display: block;
}

.info-sidebar {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.event-title {
  font-size: clamp(1.4rem, 2.5vw, 2.1rem);
  font-weight: 800;
  letter-spacing: -0.01em;
  color: #ffffff;
  line-height: 1.25;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.5);
}

.meta-card {
  background-color: rgba(30, 41, 59, 0.75);
  border: 1px solid rgba(255, 255, 255, 0.12);
  backdrop-filter: blur(12px);
  border-radius: 18px;
  padding: 1.75rem 1.5rem 2.5rem 1.5rem; /* Sisi bawah diperpanjang sedikit saja lagi */
  display: flex;
  flex-direction: column;
  gap: 1.55rem;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.25);
  height: 100%;
}

.meta-item {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.meta-icon {
  width: 22px;
  height: 22px;
  color: #ffffff;
  flex-shrink: 0;
  opacity: 0.95;
}

.meta-text {
  font-size: 1.05rem;
  color: #f8fafc;
  line-height: 1.4;
  font-weight: 500;
}

.meta-text.highlight {
  font-weight: 700;
}

.meta-text.normal {
  font-weight: 600;
}

/* RESPONSIVE LAYOUT UNTUK MODE MOBILE (MENIRU PERSIS CONTOH GAMBAR NYOMAN FEST) */
@media (max-width: 960px) {
  .hero-section {
    padding: 1.25rem 1rem;
  }
  .hero-grid {
    grid-template-columns: 1fr;
    gap: 1.25rem;
  }
  .banner-card {
    border-radius: 8px; /* Rounded gambar hero dikurangi pada mode mobile */
    aspect-ratio: auto;
    max-height: 220px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .banner-image {
    object-fit: contain;
  }
  .hero-title-block {
    margin-top: 0.25rem;
    margin-bottom: 0.5rem;
  }
  .event-title {
    font-size: 1.45rem;
    font-weight: 800;
    color: #ffffff;
  }
  .meta-card {
    background: transparent;
    border: none;
    backdrop-filter: none;
    box-shadow: none;
    padding: 0;
    gap: 0.95rem;
  }
  .meta-item {
    gap: 0.85rem;
  }
  .meta-icon {
    width: 20px;
    height: 20px;
    color: #94a3b8; /* Ikon abu-abu halus persis contoh Nyoman Fest */
  }
  .meta-text {
    font-size: 0.98rem;
    color: #f8fafc;
    font-weight: 500;
  }
}
</style>
