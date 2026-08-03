<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

// Import local image assets
import heroBg from '../assets/images/spike_elite_hero.png'
import celebrationImg from '../assets/images/spike_elite_celebration.png'
import blogHuddle from '../assets/images/blog_huddle.png'
import blogTeam from '../assets/images/blog_team.png'
import blogPreview from '../assets/images/blog_preview.png'
import blogPlayer from '../assets/images/blog_player.png'
import venueBulunganImg from '../assets/images/venue_bulungan.png'
import venueCilandakImg from '../assets/images/venue_cilandak.png'
import logoImg from '../assets/images/M (1).png'

const emit = defineEmits(['navigate'])

const openBlogDetail = (title, category, img) => {
  emit('navigate', 'blog-detail', {
    title,
    category,
    categoryColor: category.includes('PRATINJAU') ? '#2563eb' : (category.includes('BERITA') ? '#059669' : '#e10600'),
    mainImage: img,
    date: '04 Agustus 2026',
    readTime: '5 Menit Baca'
  })
}

// Countdown timer state
const days = ref('02')
const hours = ref('14')
const minutes = ref('38')
const seconds = ref('00')

let countdownInterval = null

onMounted(() => {
  // Set target date to exactly 2 days, 14 hours, 38 minutes, and 0 seconds from now for a live ticking countdown
  const targetDate = new Date(Date.now() + (2 * 24 * 60 * 60 * 1000) + (14 * 60 * 60 * 1000) + (38 * 60 * 1000))
  
  const updateCountdown = () => {
    const now = new Date()
    const diff = targetDate - now
    if (diff <= 0) {
      days.value = '00'
      hours.value = '00'
      minutes.value = '00'
      seconds.value = '00'
      if (countdownInterval) clearInterval(countdownInterval)
      return
    }
    
    const d = Math.floor(diff / (1000 * 60 * 60 * 24))
    const h = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60))
    const m = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60))
    const s = Math.floor((diff % (1000 * 60)) / 1000)
    
    days.value = String(d).padStart(2, '0')
    hours.value = String(h).padStart(2, '0')
    minutes.value = String(m).padStart(2, '0')
    seconds.value = String(s).padStart(2, '0')
  }
  
  updateCountdown()
  countdownInterval = setInterval(updateCountdown, 1000) // Update every second
})

onUnmounted(() => {
  if (countdownInterval) clearInterval(countdownInterval)
})

const handleGetTickets = () => {
  emit('navigate', 'tickets')
}

const handleOpenMaps = () => {
  window.open('https://maps.google.com/?q=Sporthall+Volleyball+Arena', '_blank')
}
</script>

<template>
  <div class="home-container">
    <!-- HERO SECTION -->
    <section id="hero" class="hero-section" :style="{ backgroundImage: `url(${heroBg})` }">
      <div class="hero-overlay"></div>
      <div class="container hero-container-inner">
        <div class="hero-content">
          <span class="hero-badge">Performa Elite</span>
          <h1 class="hero-title">
            Kuasai <span class="text-red">Lapangan</span>
          </h1>
          <p class="hero-description">
            Bergabunglah dalam warisan keunggulan. Alami bola voli profesional yang didorong oleh presisi, kekuatan, dan pengejaran kemenangan.
          </p>
          <div class="hero-buttons">
            <button class="btn-red" @click="handleGetTickets">Daftar Sekarang</button>
            <button class="btn-outline" @click="handleGetTickets">Lihat Jadwal</button>
          </div>
        </div>
      </div>
    </section>

    <!-- NEXT MATCH FLOATING CARD -->
    <div class="next-match-wrapper">
      <div class="next-match-card">
        <!-- Match Info -->
        <div class="match-info">
          <span class="match-badge">Pertandingan Berikutnya</span>
          <h3 class="match-teams">PBV Maluku vs. City Titans</h3>
          <div class="match-date">
            <svg class="icon-calendar" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect>
              <line x1="16" y1="2" x2="16" y2="6"></line>
              <line x1="8" y1="2" x2="8" y2="6"></line>
              <line x1="3" y1="10" x2="21" y2="10"></line>
            </svg>
            <span>24 Okt 2026 • 19:30 WIT</span>
          </div>
          <div class="match-location">
            <svg class="icon-location" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path>
              <circle cx="12" cy="10" r="3"></circle>
            </svg>
            <span>GOR Bulungan, Jakarta</span>
          </div>
        </div>

        <!-- Countdown Divider Left (Desktop) -->
        <div class="vertical-divider"></div>

        <!-- Match Countdown -->
        <div class="match-countdown">
          <div class="countdown-item">
            <div class="countdown-num-wrapper">
              <transition name="tick" mode="out-in">
                <span :key="days" class="countdown-num">{{ days }}</span>
              </transition>
            </div>
            <span class="countdown-label">Hari</span>
          </div>
          <span class="countdown-colon">:</span>
          <div class="countdown-item">
            <div class="countdown-num-wrapper">
              <transition name="tick" mode="out-in">
                <span :key="hours" class="countdown-num">{{ hours }}</span>
              </transition>
            </div>
            <span class="countdown-label">Jam</span>
          </div>
          <span class="countdown-colon">:</span>
          <div class="countdown-item">
            <div class="countdown-num-wrapper">
              <transition name="tick" mode="out-in">
                <span :key="minutes" class="countdown-num">{{ minutes }}</span>
              </transition>
            </div>
            <span class="countdown-label">Menit</span>
          </div>
          <span class="countdown-colon">:</span>
          <div class="countdown-item">
            <div class="countdown-num-wrapper">
              <transition name="tick" mode="out-in">
                <span :key="seconds" class="countdown-num">{{ seconds }}</span>
              </transition>
            </div>
            <span class="countdown-label">Detik</span>
          </div>
        </div>

        <!-- Countdown Divider Right (Desktop) -->
        <div class="vertical-divider"></div>

        <!-- Action Button -->
        <div class="match-action">
          <button class="btn-maps" @click="handleOpenMaps">Peta</button>
        </div>
      </div>
    </div>

    <!-- RECENT RESULTS SECTION -->
    <section class="results-section">
      <div class="container">
        <div class="results-header">
          <div>
            <h2 class="section-title-italic">Hasil Pertandingan</h2>
            <p class="section-subtitle">Dominasi yang terus berlanjut.</p>
          </div>
          <a href="#" class="btn-link" @click.prevent="handleGetTickets">
            Seluruh Musim
            <svg class="icon-arrow-right" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <line x1="5" y1="12" x2="19" y2="12"></line>
              <polyline points="12 5 19 12 12 19"></polyline>
            </svg>
          </a>
        </div>

        <div class="results-grid">
          <!-- Column 1 -->
          <div class="score-cards-stack">
            <!-- Card 1 -->
            <div class="result-card score-card-item">
              <span class="row-meta">Laga Tandang • 12 Okt</span>
              <div class="row-team-score">
                <span class="row-team-name text-bold">PBV Maluku</span>
                <span class="row-score-value text-bold">3</span>
              </div>
              <div class="row-team-score">
                <span class="row-team-name text-muted">Thunder Jets</span>
                <span class="row-score-value text-muted">0</span>
              </div>
            </div>

            <!-- Card 2 -->
            <div class="result-card score-card-item">
              <span class="row-meta">Laga Kandang • 05 Okt</span>
              <div class="row-team-score">
                <span class="row-team-name text-bold">PBV Maluku</span>
                <span class="row-score-value text-bold">3</span>
              </div>
              <div class="row-team-score">
                <span class="row-team-name text-muted">Phoenix Pro</span>
                <span class="row-score-value text-muted">2</span>
              </div>
            </div>
          </div>

          <!-- Column 2 -->
          <div class="score-cards-stack">
            <!-- Card 3 -->
            <div class="result-card score-card-item">
              <span class="row-meta">Laga Tandang • 28 Sep</span>
              <div class="row-team-score">
                <span class="row-team-name text-bold">PBV Maluku</span>
                <span class="row-score-value text-bold">3</span>
              </div>
              <div class="row-team-score">
                <span class="row-team-name text-muted">Wolves VC</span>
                <span class="row-score-value text-muted">1</span>
              </div>
            </div>

            <!-- Card 4 -->
            <div class="result-card score-card-item">
              <span class="row-meta">Laga Kandang • 20 Sep</span>
              <div class="row-team-score">
                <span class="row-team-name text-bold">PBV Maluku</span>
                <span class="row-score-value text-bold">3</span>
              </div>
              <div class="row-team-score">
                <span class="row-team-name text-muted">City Titans</span>
                <span class="row-score-value text-muted">1</span>
              </div>
            </div>
          </div>

          <!-- Column 3 -->
          <div class="score-cards-stack">
            <!-- Card 5 -->
            <div class="result-card score-card-item">
              <span class="row-meta">Laga Tandang • 12 Sep</span>
              <div class="row-team-score">
                <span class="row-team-name text-bold">PBV Maluku</span>
                <span class="row-score-value text-bold">3</span>
              </div>
              <div class="row-team-score">
                <span class="row-team-name text-muted">Vipers VC</span>
                <span class="row-score-value text-muted">0</span>
              </div>
            </div>

            <!-- Card 6 -->
            <div class="result-card score-card-item">
              <span class="row-meta">Laga Kandang • 04 Sep</span>
              <div class="row-team-score">
                <span class="row-team-name text-bold">PBV Maluku</span>
                <span class="row-score-value text-bold">3</span>
              </div>
              <div class="row-team-score">
                <span class="row-team-name text-muted">Eagle Spikes</span>
                <span class="row-score-value text-muted">2</span>
              </div>
            </div>
          </div>

          <!-- Column 4 -->
          <div class="score-cards-stack">
            <!-- Card 7 -->
            <div class="result-card score-card-item">
              <span class="row-meta">Laga Tandang • 27 Agt</span>
              <div class="row-team-score">
                <span class="row-team-name text-bold">PBV Maluku</span>
                <span class="row-score-value text-bold">3</span>
              </div>
              <div class="row-team-score">
                <span class="row-team-name text-muted">Galaxy Club</span>
                <span class="row-score-value text-muted">0</span>
              </div>
            </div>

            <!-- Card 8 -->
            <div class="result-card score-card-item">
              <span class="row-meta">Laga Kandang • 19 Agt</span>
              <div class="row-team-score">
                <span class="row-team-name text-bold">PBV Maluku</span>
                <span class="row-score-value text-bold">3</span>
              </div>
              <div class="row-team-score">
                <span class="row-team-name text-muted">Storm VC</span>
                <span class="row-score-value text-muted">1</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- OUR VENUES SECTION -->
    <section class="venues-section">
      <div class="container">
        <div class="venues-header">
          <h2 class="section-title-bold-blue">Tempat Latihan</h2>
        </div>

        <div class="venues-grid">
          <!-- Venue 1: GOR Bulungan -->
          <div class="venue-card" :style="{ backgroundImage: `url(${venueBulunganImg})` }">
            <div class="venue-overlay">
              <span class="venue-badge">Lapangan Kandang</span>
              <h3 class="venue-title">GOR Bulungan</h3>
              <p class="venue-description">Pusat kekuatan PBV Maluku. Kapasitas 3.000 penonton dengan teknologi lantai modern dan tata lampu profesional.</p>
            </div>
          </div>

          <!-- Venue 2: GOR Cilandak Barat -->
          <div class="venue-card" :style="{ backgroundImage: `url(${venueCilandakImg})` }">
            <div class="venue-overlay">
              <span class="venue-badge">Lapangan Latihan</span>
              <h3 class="venue-title">GOR Cilandak Barat</h3>
              <p class="venue-description">Lapangan latihan khusus kami di Cilandak Barat. Permukaan kayu premium dan fasilitas yang dioptimalkan untuk sesi latihan intensif.</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- CLUB BLOG SECTION -->
    <section class="blog-section">
      <div class="container">
        <div class="blog-header">
          <div>
            <h2 class="section-title-italic">Blog Klub</h2>
            <p class="section-subtitle">Berita terbaru, pratinjau pertandingan, dan ringkasan dari PBV Maluku.</p>
          </div>
        </div>

        <div class="blog-grid">
          <!-- Blog Card 1 -->
          <article class="blog-card" @click="openBlogDetail('Ringkasan Laga: PBV Maluku bermain imbang di kandang melawan Columbus Crew', 'RINGKASAN LAGA', blogHuddle)">
            <div class="blog-img-wrapper">
              <img :src="blogHuddle" alt="Match Recap huddle" class="blog-img" />
            </div>
            <div class="blog-info">
              <h3 class="blog-title">
                Ringkasan Laga: PBV Maluku bermain imbang di kandang melawan Columbus Crew
              </h3>
            </div>
          </article>

          <!-- Blog Card 2 -->
          <article class="blog-card" @click="openBlogDetail('Ringkasan Laga: PBV Maluku II kalah tipis melawan Chattanooga FC', 'RINGKASAN LAGA', blogTeam)">
            <div class="blog-img-wrapper">
              <img :src="blogTeam" alt="Match Recap team" class="blog-img" />
            </div>
            <div class="blog-info">
              <h3 class="blog-title">
                Ringkasan Laga: PBV Maluku II kalah tipis melawan Chattanooga FC
              </h3>
            </div>
          </article>

          <!-- Blog Card 3 -->
          <article class="blog-card" @click="openBlogDetail('Pratinjau Laga: PBV Maluku menjamu Columbus Crew untuk kemenangan ketujuh beruntun', 'PRATINJAU PERTANDINGAN', blogPreview)">
            <div class="blog-img-wrapper">
              <img :src="blogPreview" alt="Match Preview Casemiro" class="blog-img" />
            </div>
            <div class="blog-info">
              <h3 class="blog-title">
                Pratinjau Laga: PBV Maluku menjamu Columbus Crew untuk kemenangan ketujuh beruntun
              </h3>
            </div>
          </article>

          <!-- Blog Card 4 -->
          <article class="blog-card" @click="openBlogDetail('Pratinjau Laga: PBV Maluku II menjamu Chattanooga FC Jumat ini', 'PRATINJAU PERTANDINGAN', blogPlayer)">
            <div class="blog-img-wrapper">
              <img :src="blogPlayer" alt="Match Preview running" class="blog-img" />
            </div>
            <div class="blog-info">
              <h3 class="blog-title">
                Pratinjau Laga: PBV Maluku II menjamu Chattanooga FC Jumat ini
              </h3>
            </div>
          </article>

          <!-- Blog Card 5 -->
          <article class="blog-card" @click="openBlogDetail('Ringkasan Laga: PBV Maluku mengamankan kemenangan dramatis 3-2 melawan Wolves VC', 'RINGKASAN LAGA', blogHuddle)">
            <div class="blog-img-wrapper">
              <img :src="blogHuddle" alt="Match Recap Wolves" class="blog-img" />
            </div>
            <div class="blog-info">
              <h3 class="blog-title">
                Ringkasan Laga: PBV Maluku mengamankan kemenangan dramatis 3-2 melawan Wolves VC
              </h3>
            </div>
          </article>

          <!-- Blog Card 6 -->
          <article class="blog-card" @click="openBlogDetail('Pratinjau Laga: Laga utama - PBV Maluku bersiap menghadapi City Titans', 'PRATINJAU PERTANDINGAN', blogTeam)">
            <div class="blog-img-wrapper">
              <img :src="blogTeam" alt="Match Preview Titans" class="blog-img" />
            </div>
            <div class="blog-info">
              <h3 class="blog-title">
                Pratinjau Laga: Laga utama - PBV Maluku bersiap menghadapi City Titans
              </h3>
            </div>
          </article>

          <!-- Blog Card 7 -->
          <article class="blog-card" @click="openBlogDetail('Berita Klub: Pendaftaran akademi PBV Maluku dibuka untuk musim gugur', 'BERITA KLUB', blogPreview)">
            <div class="blog-img-wrapper">
              <img :src="blogPreview" alt="Club News Academy" class="blog-img" />
            </div>
            <div class="blog-info">
              <h3 class="blog-title">
                Berita Klub: Pendaftaran akademi PBV Maluku dibuka untuk musim gugur
              </h3>
            </div>
          </article>

          <!-- Blog Card 8 -->
          <article class="blog-card" @click="openBlogDetail('Sorotan Latihan: Cuplikan latihan pertahanan intensitas tinggi', 'SOROTAN LATIHAN', blogPlayer)">
            <div class="blog-img-wrapper">
              <img :src="blogPlayer" alt="Training Spotlight defense" class="blog-img" />
            </div>
            <div class="blog-info">
              <h3 class="blog-title">
                Sorotan Latihan: Cuplikan latihan pertahanan intensitas tinggi
              </h3>
            </div>
          </article>
        </div>
      </div>
    </section>

    <!-- FOOTER -->
    <footer class="footer-section">
      <div class="container footer-content-inner">
        <!-- Logo Image -->
        <div class="footer-logo-container">
          <img :src="logoImg" alt="PBV MALUKU Logo" class="footer-logo-img" />
        </div>
        
        <p class="footer-desc">
          Tujuan utama untuk bola voli profesional dan pengembangan atletik.
        </p>
        
        <div class="social-links">
          <a href="#" class="social-btn" title="Website">
            <svg class="social-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <circle cx="12" cy="12" r="10"></circle>
              <line x1="2" y1="12" x2="22" y2="12"></line>
              <path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"></path>
            </svg>
          </a>
          <a href="#" class="social-btn" title="Instagram">
            <svg class="social-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <rect x="2" y="2" width="20" height="20" rx="5" ry="5"></rect>
              <path d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z"></path>
              <line x1="17.5" y1="6.5" x2="17.51" y2="6.5"></line>
            </svg>
          </a>
          <a href="#" class="social-btn" title="Share">
            <svg class="social-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <circle cx="18" cy="5" r="3"></circle>
              <circle cx="6" cy="12" r="3"></circle>
              <circle cx="18" cy="19" r="3"></circle>
              <line x1="8.59" y1="13.51" x2="15.42" y2="17.49"></line>
              <line x1="15.41" y1="6.51" x2="8.59" y2="10.49"></line>
            </svg>
          </a>
        </div>

        <div class="footer-bottom">
          <p class="copyright-text">
            &copy; 2026 Klub Bola Voli PBV Maluku. Hak cipta dilindungi undang-undang.
          </p>
        </div>
      </div>
    </footer>
  </div>
</template>

<style scoped>
/* CONTAINER */
.home-container {
  display: flex;
  flex-direction: column;
  background-color: #f8fafc;
  color: #0f172a;
}

.container {
  max-width: 1540px;
  margin: 0 auto;
  padding: 0 2rem;
  width: 100%;
}

/* HERO SECTION */
.hero-section {
  position: relative;
  height: 85vh;
  min-height: 650px;
  max-height: 900px;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  display: flex;
  align-items: center;
  color: #ffffff;
  /* Slanted diagonal bottom edge */
  clip-path: polygon(0 0, 100% 0, 100% 90%, 0 98%);
  
  /* Pull up behind fixed navbar */
  margin-top: -68px;
  padding-top: 68px;
}

.hero-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, #0f2c59 0%, rgba(15, 44, 89, 0.7) 45%, rgba(15, 44, 89, 0) 85%);
  z-index: 1;
}

.hero-container-inner {
  position: relative;
  z-index: 2;
  display: flex;
  justify-content: flex-start;
  width: 100%;
}

.hero-content {
  position: relative;
  max-width: 850px;
  text-align: left;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
  transform: translateY(-20px);
}

.hero-badge {
  background-color: #e10600;
  color: #ffffff;
  font-size: 0.75rem;
  font-weight: 800;
  padding: 0.35rem 0.95rem;
  border-radius: 4px;
  letter-spacing: 1.5px;
  margin-bottom: 1.5rem;
  display: inline-block;
}

.hero-title {
  font-family: 'Outfit', 'Inter', sans-serif;
  font-size: 4rem;
  font-weight: 900;
  font-style: normal;
  line-height: 1.1;
  letter-spacing: -1.5px;
  margin-bottom: 1.5rem;
  color: #ffffff;
  white-space: nowrap;
}

.hero-title .text-red {
  color: #e10600;
}

.hero-description {
  font-size: 1.15rem;
  line-height: 1.6;
  color: #cbd5e1;
  margin-bottom: 2.5rem;
  max-width: 580px;
}

.hero-buttons {
  display: flex;
  gap: 1.25rem;
  width: 100%;
  justify-content: flex-start;
}

.btn-red {
  background-color: #e10600;
  color: #ffffff;
  border: none;
  padding: 0.9rem 2.2rem;
  border-radius: 4px;
  font-size: 0.9rem;
  font-weight: 800;
  cursor: pointer;
  letter-spacing: 0.8px;
  transition: all 0.25s ease;
  box-shadow: 0 4px 14px rgba(225, 6, 0, 0.4);
}

.btn-red:hover {
  background-color: #b50500;
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(225, 6, 0, 0.6);
}

.btn-outline {
  background-color: rgba(255, 255, 255, 0.05);
  color: #ffffff;
  border: 1.5px solid rgba(255, 255, 255, 0.35);
  padding: 0.9rem 2.2rem;
  border-radius: 4px;
  font-size: 0.9rem;
  font-weight: 800;
  cursor: pointer;
  letter-spacing: 0.8px;
  backdrop-filter: blur(8px);
  transition: all 0.25s ease;
}

.btn-outline:hover {
  background-color: rgba(255, 255, 255, 0.15);
  border-color: #ffffff;
  transform: translateY(-2px);
}

/* NEXT MATCH FLOATING CARD */
.next-match-wrapper {
  max-width: 1540px;
  margin: 0 auto;
  padding: 0 2rem;
  width: 100%;
  position: relative;
  z-index: 10;
  margin-top: -6rem; /* Pull up to overlay bottom of Hero */
}

.next-match-card {
  background-color: #ffffff;
  border-radius: 8px;
  padding: 2.25rem 3rem;
  box-shadow: 0 16px 40px rgba(8, 17, 40, 0.08);
  display: flex;
  align-items: center;
  justify-content: space-between;
  border: 1px solid rgba(226, 232, 240, 0.8);
}

.match-info {
  flex: 1;
}

.match-badge {
  color: #e10600;
  font-size: 0.72rem;
  font-weight: 700;
  letter-spacing: 1.2px;
  margin-bottom: 0.5rem;
  display: block;
}

.match-teams {
  font-size: 1.4rem;
  font-weight: 700;
  color: #000000;
  margin-bottom: 0.4rem;
  letter-spacing: -0.4px;
}

.match-date {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: #64748b;
  font-size: 0.88rem;
  font-weight: 500;
}

.icon-calendar {
  width: 16px;
  height: 16px;
  stroke: #e10600;
}

.match-location {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: #64748b;
  font-size: 0.88rem;
  font-weight: 500;
  margin-top: 0.4rem;
}

.icon-location {
  width: 16px;
  height: 16px;
  stroke: #e10600;
}

.vertical-divider {
  width: 1px;
  height: 55px;
  background-color: #e2e8f0;
  margin: 0 2.5rem;
}

.match-countdown {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.countdown-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-width: 50px;
}

.countdown-num-wrapper {
  position: relative;
  height: 2.2rem;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  width: 100%;
}

.countdown-num {
  font-size: 2rem;
  font-weight: 700;
  color: #000000;
  line-height: 1;
  font-family: 'Outfit', 'Inter', sans-serif;
  display: inline-block;
}

/* flip/tick animation */
.tick-enter-active,
.tick-leave-active {
  transition: transform 0.25s cubic-bezier(0.25, 0.46, 0.45, 0.94), opacity 0.25s ease;
  position: absolute;
}

.tick-enter-from {
  transform: translateY(15px);
  opacity: 0;
}

.tick-leave-to {
  transform: translateY(-15px);
  opacity: 0;
}

.countdown-label {
  font-size: 0.65rem;
  font-weight: 500;
  color: #94a3b8;
  margin-top: 0.35rem;
  letter-spacing: 0.5px;
}

.countdown-colon {
  font-size: 1.75rem;
  font-weight: 700;
  color: #cbd5e1;
  transform: translateY(-3px);
}

.match-action {
  margin-left: 1.5rem;
}

.btn-navy {
  background-color: #000000;
  color: #ffffff;
  border: none;
  padding: 0.95rem 2.2rem;
  border-radius: 4px;
  font-size: 0.82rem;
  font-weight: 700;
  cursor: pointer;
  letter-spacing: 1px;
  transition: all 0.25s ease;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.25);
}

.btn-navy:hover {
  background-color: #2563eb;
  transform: translateY(-1px);
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.35);
}

.btn-maps {
  background-color: #2563eb;
  color: #ffffff;
  border: none;
  padding: 0.95rem 2.2rem;
  border-radius: 4px;
  font-size: 0.82rem;
  font-weight: 700;
  cursor: pointer;
  letter-spacing: 1px;
  transition: all 0.25s ease;
  box-shadow: 0 4px 10px rgba(37, 99, 235, 0.25);
}

.btn-maps:hover {
  background-color: #1d4ed8;
  transform: translateY(-1px);
  box-shadow: 0 6px 15px rgba(29, 78, 216, 0.35);
}

/* RECENT RESULTS SECTION */
.results-section {
  padding: 4.5rem 0 3.5rem 0;
  background-color: #f8fafc;
}

.results-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 3.5rem;
}

.section-title-italic {
  font-family: 'Outfit', 'Inter', sans-serif;
  font-size: 2.25rem;
  font-weight: 700;
  font-style: normal;
  color: #000000;
  letter-spacing: -0.5px;
  margin-bottom: 0.5rem;
}

.section-subtitle {
  font-size: 0.95rem;
  color: #64748b;
  font-weight: 500;
}

.btn-link {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  color: #000000;
  text-decoration: none;
  font-size: 0.78rem;
  font-weight: 900;
  letter-spacing: 1px;
  transition: color 0.2s ease;
}

.btn-link:hover {
  color: #e10600;
}

.icon-arrow-right {
  width: 15px;
  height: 15px;
  transition: transform 0.25s ease;
}

.btn-link:hover .icon-arrow-right {
  transform: translateX(4px);
}

.results-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 0.85rem;
  max-width: 1540px;
  margin: 0 auto;
}

.result-card {
  background-color: #ffffff;
  border-radius: 8px;
  border: 1px solid rgba(226, 232, 240, 0.8);
  box-shadow: 0 4px 18px rgba(0, 0, 0, 0.03);
  padding: 2.25rem 2rem;
  display: flex;
  flex-direction: column;
  transition: transform 0.25s ease, box-shadow 0.25s ease;
}

.result-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 28px rgba(0, 0, 0, 0.06);
}

/* Final Score Card specifically */
.card-badge-violet {
  background-color: #ebedff;
  color: #3b42c4;
  font-size: 0.65rem;
  font-weight: 800;
  padding: 0.3rem 0.8rem;
  border-radius: 12px;
  letter-spacing: 0.8px;
  margin-bottom: 2rem;
}

.score-display {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  margin-bottom: 2rem;
}

.team-block {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.75rem;
  flex: 1;
}

.team-icon-circle {
  width: 54px;
  height: 54px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.bg-navy-light {
  background-color: #f1f5ff;
  color: #000000;
}

.bg-gray-light {
  background-color: #f8fafc;
  color: #64748b;
}

.icon-volleyball, .icon-shield {
  width: 24px;
  height: 24px;
}

.team-name {
  font-size: 0.72rem;
  font-weight: 800;
  color: #475569;
  letter-spacing: 0.8px;
  text-align: center;
}

.score-nums {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
}

.score-val {
  font-family: 'Outfit', 'Inter', sans-serif;
  font-size: 2.75rem;
  font-weight: 900;
  color: #000000;
}

.score-vs {
  font-size: 0.78rem;
  font-weight: 700;
  color: #94a3b8;
  margin-top: 6px;
}

.card-divider {
  width: 100%;
  height: 1px;
  background-color: #f1f5f9;
  margin-bottom: 1.5rem;
}

.card-footer-info {
  display: flex;
  justify-content: space-between;
  width: 100%;
  font-size: 0.72rem;
  font-weight: 800;
  letter-spacing: 0.8px;
}

.footer-label-gray {
  color: #94a3b8;
}

.footer-label-dark {
  color: #000000;
}

/* Celebration Photo Card */
.celebration-image-card {
  padding: 0;
  background-size: cover;
  background-position: center;
  position: relative;
  overflow: hidden;
  min-height: 280px;
}

/* Score rows card */
.score-cards-stack {
  display: flex;
  flex-direction: column;
  gap: 0.85rem;
}

.score-card-item {
  padding: 1.5rem 1.75rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 0.75rem;
}

.row-meta {
  font-size: 0.68rem;
  font-weight: 600;
  color: #94a3b8;
  letter-spacing: 1px;
}

.row-team-score {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.row-team-name {
  font-size: 0.9rem;
  font-weight: 500;
  color: #000000;
}

.row-score-value {
  font-size: 1.15rem;
  font-family: 'Outfit', 'Inter', sans-serif;
}

.text-bold {
  font-weight: 700;
  color: #000000;
}

.text-muted {
  color: #94a3b8 !important;
}

/* OUR VENUES SECTION */
.venues-section {
  padding: 3.5rem 0 3.5rem 0;
  background-color: #ffffff;
}

.venues-header {
  margin-bottom: 2.5rem;
}

.section-title-bold-blue {
  font-family: 'Outfit', 'Inter', sans-serif;
  font-size: 2.25rem;
  font-weight: 700;
  color: #000000;
  text-transform: none;
  letter-spacing: -0.5px;
}

.venues-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.25rem;
  max-width: 1540px;
  margin: 0 auto;
}

.venue-card {
  position: relative;
  width: 100%;
  min-height: 300px;
  background-size: cover;
  background-position: center;
  border-radius: 12px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
  transition: transform 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94), box-shadow 0.3s ease;
}

.venue-card:hover {
  transform: scale(1.03);
  box-shadow: 0 16px 36px rgba(0, 0, 0, 0.15);
}

.venue-card::after {
  content: '';
  position: absolute;
  top: 0;
  left: -150%;
  width: 60%;
  height: 100%;
  background: linear-gradient(
    90deg,
    rgba(255, 255, 255, 0) 0%,
    rgba(255, 255, 255, 0.35) 50%,
    rgba(255, 255, 255, 0) 100%
  );
  transform: skewX(-25deg);
  transition: left 0.6s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  pointer-events: none;
  z-index: 2;
}

.venue-card:hover::after {
  left: 150%;
}

.venue-overlay {
  background: linear-gradient(to top, rgba(0, 0, 0, 0.95) 0%, rgba(0, 0, 0, 0.55) 55%, rgba(0, 0, 0, 0) 100%);
  padding: 1.5rem;
  color: #ffffff;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  width: 100%;
  z-index: 1;
}

.venue-badge {
  background: linear-gradient(135deg, #e10600, #ff4d4d);
  color: #ffffff;
  font-size: 0.62rem;
  font-weight: 800;
  padding: 0.3rem 0.75rem;
  border-radius: 4px;
  letter-spacing: 0.8px;
  margin-bottom: 0.65rem;
  text-transform: uppercase;
  display: inline-block;
}

.venue-title {
  font-family: 'Outfit', 'Inter', sans-serif;
  font-size: 1.25rem;
  font-weight: 700;
  color: #ffffff;
  margin-bottom: 0.4rem;
  text-transform: uppercase;
  letter-spacing: -0.3px;
}

.venue-description {
  font-size: 0.82rem;
  color: rgba(255, 255, 255, 0.85);
  line-height: 1.45;
  max-width: 95%;
}

/* CLUB BLOG SECTION */
.blog-section {
  padding: 3.5rem 0 4.5rem 0;
  background-color: #f8fafc;
}

.blog-header {
  margin-bottom: 2rem;
}

.blog-grid {
  display: flex;
  overflow-x: auto;
  gap: 1.25rem;
  max-width: 100%;
  margin: 0 auto;
  padding: 0.5rem 0.25rem 2rem 0.25rem;
  scroll-behavior: smooth;
  -webkit-overflow-scrolling: touch;
}

/* Custom subtle scrollbar */
.blog-grid::-webkit-scrollbar {
  height: 6px;
}
.blog-grid::-webkit-scrollbar-track {
  background: rgba(0, 0, 0, 0.05);
  border-radius: 10px;
}
.blog-grid::-webkit-scrollbar-thumb {
  background: rgba(15, 44, 89, 0.25);
  border-radius: 10px;
  transition: background-color 0.25s ease;
}
.blog-grid::-webkit-scrollbar-thumb:hover {
  background: rgba(15, 44, 89, 0.55);
}

.blog-card {
  flex: 0 0 320px;
  display: flex;
  flex-direction: column;
  cursor: pointer;
  text-decoration: none;
}

.blog-img-wrapper {
  width: 100%;
  aspect-ratio: 16 / 9;
  overflow: hidden;
  border-radius: 6px;
  background-color: #f1f5f9;
  position: relative;
  transition: transform 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.blog-card:hover .blog-img-wrapper {
  transform: scale(1.04);
}

.blog-img-wrapper::after {
  content: '';
  position: absolute;
  top: 0;
  left: -150%;
  width: 60%;
  height: 100%;
  background: linear-gradient(
    90deg,
    rgba(255, 255, 255, 0) 0%,
    rgba(255, 255, 255, 0.3) 50%,
    rgba(255, 255, 255, 0) 100%
  );
  transform: skewX(-25deg);
  transition: left 0.6s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  pointer-events: none;
}

.blog-card:hover .blog-img-wrapper::after {
  left: 150%;
}

.blog-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: opacity 0.2s ease;
}

.blog-card:hover .blog-img {
  opacity: 0.92;
}

.blog-info {
  padding-top: 0.85rem;
}

.blog-title {
  font-family: 'Inter', sans-serif;
  font-size: 0.88rem;
  font-weight: 700;
  color: #000000;
  line-height: 1.35;
  letter-spacing: -0.1px;
}

/* FOOTER SECTION */
.footer-section {
  background-color: #ffffff;
  padding: 3rem 0;
  color: #000000;
  border-top: 1px solid #e2e8f0;
}

.footer-content-inner {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  gap: 1.25rem;
}

.footer-logo-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.footer-logo-img {
  width: auto;
  height: 80px;
}

.footer-desc {
  font-size: 0.9rem;
  color: #475569;
  line-height: 1.5;
  max-width: 450px;
  margin: 0 auto;
}

.social-links {
  display: flex;
  gap: 0.75rem;
  margin-top: 0.25rem;
}

.social-btn {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background-color: #f1f5f9;
  border: 1px solid #e2e8f0;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #475569;
  transition: all 0.25s ease;
  text-decoration: none;
}

.social-btn:hover {
  background-color: #2563eb;
  color: #ffffff;
  border-color: #2563eb;
  transform: translateY(-2px);
}

.social-icon {
  width: 16px;
  height: 16px;
}

.footer-bottom {
  margin-top: 1rem;
  border-top: 1px solid #e2e8f0;
  padding-top: 1.5rem;
  width: 100%;
}

.copyright-text {
  font-size: 0.72rem;
  color: #64748b;
  font-weight: 600;
  letter-spacing: 0.8px;
}

/* RESPONSIVE DESIGN (MEDIA QUERIES) */
@media (max-width: 1024px) {
  .results-grid {
    grid-template-columns: repeat(4, 280px);
  }
  .venues-grid {
    gap: 1rem;
  }
  .venue-card {
    min-height: 250px;
  }
  .blog-card {
    flex: 0 0 290px;
  }
}

@media (max-width: 860px) {
  /* Wider side margins on mobile */
  .container {
    padding: 0 1.25rem;
  }

  /* Hero section */
  .hero-section {
    height: 70vh;
    min-height: 480px;
    clip-path: polygon(0 0, 100% 0, 100% 93%, 0 98%);
    margin-top: -58px;
    padding-top: 58px;
  }
  .hero-title {
    font-size: 2.5rem;
    white-space: normal;
  }
  .hero-description {
    font-size: 0.9rem;
    margin-bottom: 1.75rem;
  }
  .hero-badge {
    font-size: 0.68rem;
    padding: 0.35rem 0.85rem;
    margin-bottom: 1rem;
  }
  .hero-content {
    transform: translateY(20px);
  }
  .btn-red, .btn-outline {
    font-size: 0.8rem;
    padding: 0.7rem 1.4rem;
  }

  /* Next match card */
  .next-match-card {
    flex-direction: column;
    padding: 1.5rem 1.25rem;
    gap: 1.1rem;
    align-items: stretch;
    text-align: center;
  }
  .match-badge {
    font-size: 0.6rem;
    padding: 0.25rem 0.7rem;
  }
  .match-teams {
    font-size: 1rem;
    margin-top: 0.4rem;
    margin-bottom: 0.4rem;
  }
  .match-date, .match-location {
    justify-content: center;
    font-size: 0.78rem;
    gap: 0.4rem;
  }
  .icon-calendar, .icon-location {
    width: 13px;
    height: 13px;
  }
  .vertical-divider {
    display: none;
  }
  .match-countdown {
    justify-content: center;
    gap: 0.5rem;
    border-top: 1px solid #f1f5f9;
    border-bottom: 1px solid #f1f5f9;
    padding: 0.85rem 0;
  }
  .countdown-num {
    font-size: 1.5rem;
  }
  .countdown-num-wrapper {
    height: 1.75rem;
  }
  .countdown-item {
    min-width: 38px;
  }
  .countdown-label {
    font-size: 0.58rem;
    margin-top: 0.2rem;
  }
  .countdown-colon {
    font-size: 1.35rem;
  }
  .match-action {
    margin-left: 0;
  }
  .btn-navy, .btn-maps {
    width: 100%;
    padding: 0.8rem;
    font-size: 0.78rem;
  }
  .next-match-wrapper {
    margin-top: -3.5rem;
  }

  /* Results section — horizontal scroll, desktop-style columns */
  .results-section {
    padding: 2.25rem 0 1.75rem 0;
  }
  .results-header {
    margin-bottom: 1.25rem;
  }
  .section-title-italic,
  .section-title-bold-blue {
    font-size: 1.5rem;
  }
  .section-subtitle {
    font-size: 0.8rem;
  }
  .btn-link {
    font-size: 0.7rem;
  }
  /* Horizontal scroll wrapper for results */
  .results-grid {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    overflow-x: auto;
    gap: 0.65rem;
    padding-bottom: 0.75rem;
    /* Smooth momentum scroll on iOS */
    -webkit-overflow-scrolling: touch;
    scroll-snap-type: x mandatory;
  }
  .results-grid::-webkit-scrollbar {
    height: 3px;
  }
  .results-grid::-webkit-scrollbar-track {
    background: #f1f5f9;
    border-radius: 4px;
  }
  .results-grid::-webkit-scrollbar-thumb {
    background: #cbd5e1;
    border-radius: 4px;
  }
  .score-cards-stack {
    flex: 0 0 220px;
    min-width: 220px;
    scroll-snap-align: start;
  }
  .result-card {
    padding: 1.1rem 1.15rem;
  }
  .score-card-item {
    padding: 1.1rem 1.15rem;
    gap: 0.55rem;
  }
  .row-meta {
    font-size: 0.6rem;
    letter-spacing: 0.5px;
  }
  .row-team-name {
    font-size: 0.78rem;
  }
  .row-score-value {
    font-size: 0.78rem;
  }

  /* Venues section */
  .venues-section {
    padding: 1.75rem 0;
  }
  .venues-grid {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    overflow-x: auto;
    gap: 0.85rem;
    padding-bottom: 0.5rem;
    -webkit-overflow-scrolling: touch;
    scroll-snap-type: x mandatory;
  }
  .venues-grid::-webkit-scrollbar {
    height: 3px;
  }
  .venues-grid::-webkit-scrollbar-track {
    background: #f1f5f9;
    border-radius: 4px;
  }
  .venues-grid::-webkit-scrollbar-thumb {
    background: #cbd5e1;
    border-radius: 4px;
  }
  .venue-card {
    flex: 0 0 260px;
    min-width: 260px;
    min-height: 185px;
    scroll-snap-align: start;
  }
  .venue-overlay {
    padding: 1rem;
  }
  .venue-badge {
    font-size: 0.6rem;
    padding: 0.2rem 0.6rem;
    margin-bottom: 0.5rem;
  }
  .venue-title {
    font-size: 1rem;
    margin-bottom: 0;
  }
  .venue-description {
    display: none;
  }

  /* Blog section */
  .blog-section {
    padding: 1.75rem 0 2.25rem 0;
  }
  .blog-card {
    flex: 0 0 240px;
  }
  .blog-grid {
    gap: 0.85rem;
  }
  .blog-title {
    font-size: 0.82rem;
  }
  .blog-category {
    font-size: 0.6rem;
  }
  .blog-date {
    font-size: 0.65rem;
  }

  /* Footer */
  .footer-logo-img {
    height: 60px;
  }
  .footer-desc {
    font-size: 0.8rem;
  }
  .copyright-text {
    font-size: 0.65rem;
  }
}

@media (max-width: 480px) {
  /* Hero */
  .hero-section {
    height: auto;
    min-height: 460px;
    margin-top: -58px;
    padding-top: 5.5rem;
    padding-bottom: 8.5rem;
  }
  .hero-title {
    font-size: 2rem;
    letter-spacing: -1px;
  }
  .hero-description {
    font-size: 0.82rem;
    margin-bottom: 1.5rem;
  }
  .hero-buttons {
    flex-direction: column;
    width: 100%;
    gap: 0.6rem;
  }
  .btn-red, .btn-outline {
    width: 100%;
    padding: 0.75rem;
    font-size: 0.78rem;
  }

  /* Next match */
  .next-match-wrapper {
    margin-top: -4.5rem;
  }
  .next-match-card {
    padding: 1.25rem 1rem;
    gap: 0.9rem;
  }
  .match-teams {
    font-size: 0.92rem;
  }
  .countdown-num {
    font-size: 1.35rem;
  }

  /* Results */
  .results-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 0.6rem;
    margin-bottom: 1.5rem;
  }
  .score-cards-stack {
    flex: 0 0 195px;
    min-width: 195px;
  }
  .score-card-item {
    padding: 0.9rem 1rem;
  }
  .row-team-name {
    font-size: 0.72rem;
  }
  .row-score-value {
    font-size: 0.72rem;
  }

  /* Venues */
  .venue-card {
    min-height: 180px;
  }
  .venue-title {
    font-size: 0.92rem;
  }
  .venue-description {
    font-size: 0.68rem;
  }

  /* Blog */
  .blog-card {
    flex: 0 0 210px;
  }
  .blog-title {
    font-size: 0.78rem;
  }

  /* General section titles */
  .section-title-italic,
  .section-title-bold-blue {
    font-size: 1.35rem;
  }
  .section-subtitle {
    font-size: 0.75rem;
  }
}
</style>
