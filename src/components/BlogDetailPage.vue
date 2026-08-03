<script setup>
import { ref, computed } from 'vue'

// Assets
import logoImg from '../assets/images/M (1).png'
import blogHuddle from '../assets/images/blog_huddle.png'
import blogTeam from '../assets/images/blog_team.png'
import blogPreview from '../assets/images/blog_preview.png'
import blogPlayer from '../assets/images/blog_player.png'

const props = defineProps({
  article: {
    type: Object,
    default: null
  }
})

const emit = defineEmits(['navigate'])

// Fallback / default article if none passed
const defaultArticle = {
  id: 1,
  category: 'RINGKASAN LAGA',
  categoryColor: '#e10600',
  title: 'Ringkasan Laga: PBV Maluku Mengamankan Kemenangan Dramatis 3-2 Melawan Wolves VC',
  date: '04 Agustus 2026',
  readTime: '5 Menit Baca',
  author: 'Tim Redaksi PBV Maluku',
  authorRole: 'Official Media Officer',
  mainImage: blogHuddle,
  imageCaption: 'Sesi huddle pemain PBV Maluku merayakan poin penentu di set kelima GOR Bulungan.',
  content: [
    {
      type: 'lead',
      text: 'Pertandingan pekan ke-12 Liga Bola Voli Nasional menyajikan pertarungan penuh tensi tinggi di GOR Bulungan, Ambon. PBV Maluku sukses membungkam perlawanan sengit dari Wolves VC melalui pertarungan marathon lima set dengan skor akhir 3-2 (25-22, 21-25, 25-19, 22-25, 15-12).'
    },
    {
      type: 'paragraph',
      text: 'Sejak peluit pertama ditiupkan, kedua tim langsung saling jual beli serangan spike tajam. Di set pertama, kerja sama apik antara setter dan spiker utama PBV Maluku berhasil mendobrak blok rapat lawan, memberikan keunggulan awal bagi tuan rumah. Tempo permainan yang cepat memaksa kedua tim untuk terus berkonsentrasi penuh di setiap poin yang diperebutkan.'
    },
    {
      type: 'paragraph',
      text: 'Set kedua berjalan lebih sengit. Wolves VC mulai menyesuaikan strategi dengan memanfaatkan celah di sisi kiri pertahanan PBV Maluku. Pergantian pemain yang dilakukan pelatih lawan di poin 18-16 terbukti efektif membalikkan keadaan dan memaksa set kedua jatuh ke tangan tamu dengan skor 21-25.'
    },
    {
      type: 'quote',
      text: 'Kedisiplinan di garis pertahanan dan ketenangan saat poin-poin kritis menjadi kunci utama kemenangan kami malam ini.',
      author: 'Pelatih Kepala PBV Maluku'
    },
    {
      type: 'paragraph',
      text: 'Set ketiga menjadi titik balik. PBV Maluku kembali tampil impresif dengan dukungan penuh suporter kandang yang tak henti-hentinya memberikan semangat. Spiker andalan berhasil mencetak enam poin beruntun melalui spike diagonal yang tidak mampu dibaca oleh libero lawan, memenangkan set ketiga 25-19.'
    },
    {
      type: 'heading',
      text: 'Momentum Kunci di Set Kelima'
    },
    {
      type: 'paragraph',
      text: 'Memasuki set penentuan, suasana GOR Bulungan bergemuruh hebat. Dukungan ribuan pendukung menjadi bahan bakar emosional para pemain. Kedua tim berimbang ketat hingga skor 12-12 sebelum dua kali spike menyilang mematikan dari spiker utama PBV Maluku di poin 13-12 dan 14-12 memastikan tuan rumah mengunci kemenangan dramatis ini dengan skor akhir 15-12.'
    },
    {
      type: 'paragraph',
      text: 'Dengan hasil kemenangan spektakuler ini, PBV Maluku memperkokoh posisi di papan atas klasemen sementara dan menjaga rekor tak terkalahkan di laga kandang sepanjang musim ini. Kemenangan ini juga menjadi modal psikologis penting menjelang laga tandang pekan depan melawan tim peringkat dua, Jakarta Falcons.'
    },
    {
      type: 'highlights',
      title: 'Catatan Statistik Utama Pertandingan:',
      items: [
        'Total Spike Poin: 68 Poin (Tingkat Keberhasilan 58%)',
        'Total Blok Sukses: 14 Blok Mematikan',
        'Ace Serve: 7 Ace Service Poin',
        'Pemain Terbaik (MVP): Muhammad Alfarez (24 Poin)'
      ]
    },
    {
      type: 'paragraph',
      text: 'Muhammad Alfarez, yang dinobatkan sebagai pemain terbaik malam itu, mengakui bahwa kemenangan ini adalah hasil kerja keras seluruh tim. "Kami berlatih sangat keras untuk menghadapi Wolves VC. Mereka tim yang kuat dan disiplin. Tapi kami percaya pada sistem permainan kami dan alhamdulillah hasilnya bisa kami raih malam ini," ujarnya usai pertandingan.'
    }
  ]
}

// Merge: if article prop is passed (from card click), merge its metadata over the default
// This ensures content is always present even when only title/image/category is passed
const currentArticle = computed(() => {
  if (!props.article) return defaultArticle
  return {
    ...defaultArticle,   // base: full default (including content)
    ...props.article      // override header fields (title, image, category, date, etc.)
  }
})

// Related Articles List
const relatedArticles = ref([
  {
    id: 2,
    category: 'PRATINJAU PERTANDINGAN',
    categoryColor: '#2563eb',
    title: 'Pratinjau Laga: PBV Maluku Menjamu Columbus Crew Untuk Kemenangan Ketujuh',
    date: '02 Agustus 2026',
    image: blogPreview
  },
  {
    id: 3,
    category: 'BERITA KLUB',
    categoryColor: '#059669',
    title: 'Pendaftaran Akademi Atlet Muda PBV Maluku Resmi Dibuka Untuk Musim Gugur',
    date: '01 Agustus 2026',
    image: blogTeam
  },
  {
    id: 4,
    category: 'SOROTAN LATIHAN',
    categoryColor: '#d97706',
    title: 'Cuplikan Latihan Pertahanan Intensitas Tinggi Sambut Laga Penentu',
    date: '30 Juli 2026',
    image: blogPlayer
  }
])

const isCopied = ref(false)
const copyArticleLink = () => {
  navigator.clipboard?.writeText(window.location.href)
  isCopied.value = true
  setTimeout(() => { isCopied.value = false }, 2000)
}

const goHome = () => emit('navigate', 'home')
const goRegister = () => emit('navigate', 'tickets')

const selectRelatedArticle = (art) => {
  emit('navigate', 'blog-detail', art)
  window.scrollTo({ top: 0, behavior: 'smooth' })
}
</script>

<template>
  <div class="blog-detail-page">

    <!-- BREADCRUMB BAR (no back button) -->
    <div class="top-breadcrumb-bar">
      <div class="container breadcrumb-container">
        <nav class="breadcrumb-list">
          <a href="#" @click.prevent="goHome">Beranda</a>
          <span class="sep">/</span>
          <a href="#" @click.prevent="goHome">Blog Klub</a>
          <span class="sep">/</span>
          <span class="current">Detail Artikel</span>
        </nav>
      </div>
    </div>

    <!-- ARTICLE HEADER -->
    <header class="article-header-section">
      <div class="container article-header-container">

        <!-- Category Badge -->
        <div class="badge-category" :style="{ backgroundColor: currentArticle.categoryColor || '#e10600' }">
          {{ currentArticle.category }}
        </div>

        <!-- Article Main Title -->
        <h1 class="article-main-title">
          {{ currentArticle.title }}
        </h1>

        <!-- Simplified author & meta row — plain inline icon + text -->
        <div class="author-meta-row">
          <span class="meta-item">
            <svg fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"/>
            </svg>
            {{ currentArticle.author }}
          </span>
          <span class="meta-dot">·</span>
          <span class="meta-item">
            <svg fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/>
            </svg>
            {{ currentArticle.date }}
          </span>
          <span class="meta-dot">·</span>
          <span class="meta-item">
            <svg fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"/>
            </svg>
            {{ currentArticle.readTime }}
          </span>
        </div>

      </div>
    </header>

    <!-- FEATURED HERO IMAGE -->
    <div class="featured-image-wrapper">
      <div class="container">
        <div class="featured-image-card">
          <img :src="currentArticle.mainImage" :alt="currentArticle.title" class="featured-img" />
          <p v-if="currentArticle.imageCaption" class="image-caption-text">{{ currentArticle.imageCaption }}</p>
        </div>
      </div>
    </div>

    <!-- CONTENT BODY & SIDEBAR GRID -->
    <div class="content-section">
      <div class="container content-grid-container">

        <!-- LEFT COLUMN: MAIN ARTICLE BODY -->
        <article class="main-article-body">

          <div v-for="(block, idx) in currentArticle.content" :key="idx" class="content-block">

            <!-- Lead paragraph (bold intro) -->
            <p v-if="block.type === 'lead'" class="body-lead">
              {{ block.text }}
            </p>

            <!-- Regular paragraph -->
            <p v-else-if="block.type === 'paragraph'" class="body-paragraph">
              {{ block.text }}
            </p>

            <!-- Sub-heading -->
            <h2 v-else-if="block.type === 'heading'" class="body-heading">
              {{ block.text }}
            </h2>

            <!-- Blockquote -->
            <blockquote v-else-if="block.type === 'quote'" class="body-quote">
              <p class="quote-text">"{{ block.text }}"</p>
              <cite v-if="block.author" class="quote-author">— {{ block.author }}</cite>
            </blockquote>

            <!-- Highlights Checklist -->
            <div v-else-if="block.type === 'highlights'" class="highlights-box">
              <p class="highlights-title">{{ block.title }}</p>
              <ul class="highlights-list">
                <li v-for="(item, itemIdx) in block.items" :key="itemIdx">
                  <svg class="check-icon" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M5 13l4 4L19 7" />
                  </svg>
                  <span>{{ item }}</span>
                </li>
              </ul>
            </div>

          </div>

          <!-- SHARE FOOTER -->
          <div class="share-footer">
            <span class="share-label">Bagikan:</span>
            <div class="share-buttons-row">
              <button class="btn-share share-wa">
                <svg fill="currentColor" viewBox="0 0 24 24"><path d="M.057 24l1.687-6.163c-1.041-1.804-1.588-3.849-1.587-5.946.003-6.556 5.338-11.891 11.893-11.891 3.181.001 6.167 1.24 8.413 3.488 2.245 2.248 3.481 5.236 3.48 8.414-.003 6.557-5.338 11.892-11.893 11.892-1.99-.001-3.951-.5-5.688-1.448l-6.305 1.654zm6.597-3.807c1.676.995 3.276 1.591 5.392 1.592 5.448 0 9.886-4.434 9.889-9.885.002-5.462-4.415-9.89-9.881-9.892-5.452 0-9.887 4.434-9.889 9.884-.001 2.225.651 3.891 1.746 5.634l-1.161 4.238 4.204-1.103z"/></svg>
                WhatsApp
              </button>
              <button class="btn-share share-fb">
                <svg fill="currentColor" viewBox="0 0 24 24"><path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/></svg>
                Facebook
              </button>
              <button class="btn-share share-copy" @click="copyArticleLink">
                <svg fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"/></svg>
                {{ isCopied ? 'Tersalin!' : 'Salin Tautan' }}
              </button>
            </div>
          </div>

        </article>

        <!-- RIGHT COLUMN: SIDEBAR -->
        <aside class="article-sidebar">



          <!-- RELATED ARTICLES -->
          <div class="sidebar-card related-card">
            <h3 class="sidebar-card-title">Artikel Terkait</h3>

            <div class="related-list">
              <div
                v-for="rel in relatedArticles"
                :key="rel.id"
                class="related-item"
                @click="selectRelatedArticle(rel)"
              >
                <div class="related-thumb-wrap">
                  <img :src="rel.image" :alt="rel.title" class="related-thumb" />
                </div>
                <div class="related-info">
                  <span class="related-badge" :style="{ color: rel.categoryColor }">{{ rel.category }}</span>
                  <h4 class="related-title">{{ rel.title }}</h4>
                  <span class="related-date">{{ rel.date }}</span>
                </div>
              </div>
            </div>
          </div>

        </aside>

      </div>
    </div>

  </div>
</template>

<style scoped>
/* ── ROOT ──────────────────────────────────────────────────── */
.blog-detail-page {
  min-height: 100vh;
  background: #f8fafc;
  color: #0f172a;
}

.container {
  max-width: 1540px;
  margin: 0 auto;
  padding: 0 2rem;
  width: 100%;
}

/* ── BREADCRUMB BAR ────────────────────────────────────────── */
.top-breadcrumb-bar {
  background: #ffffff;
  border-bottom: 1px solid #e2e8f0;
  padding: 0.7rem 0;
}

.breadcrumb-container {
  display: flex;
  align-items: center;
}

.breadcrumb-list {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  font-size: 0.8rem;
  color: #64748b;
}

.breadcrumb-list a {
  color: #64748b;
  text-decoration: none;
  transition: color 0.15s;
}

.breadcrumb-list a:hover { color: #e10600; }
.breadcrumb-list .sep { opacity: 0.4; }
.breadcrumb-list .current { color: #0f172a; font-weight: 600; }

/* ── ARTICLE HEADER ────────────────────────────────────────── */
.article-header-section {
  background: #ffffff;
  padding: 2rem 0 1.5rem;
  border-bottom: 1px solid #e2e8f0;
}

.article-header-container {
  display: flex;
  flex-direction: column;
  gap: 0.9rem;
}

.badge-category {
  display: inline-block;
  color: #ffffff;
  font-size: 0.68rem;
  font-weight: 800;
  letter-spacing: 1px;
  padding: 0.3rem 0.8rem;
  border-radius: 4px;
  text-transform: uppercase;
  align-self: flex-start;
}

.article-main-title {
  font-size: clamp(1.6rem, 3.5vw, 2.6rem);
  font-weight: 800;
  color: #0f172a;
  line-height: 1.3;
  margin: 0;
  max-width: 1100px;
}

/* Simplified plain author + meta row */
.author-meta-row {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: 0.5rem;
  color: #64748b;
}

.meta-item {
  display: flex;
  align-items: center;
  gap: 0.3rem;
  font-size: 0.82rem;
  font-weight: 500;
}

.meta-item svg {
  width: 15px;
  height: 15px;
  color: #94a3b8;
  flex-shrink: 0;
}

.meta-dot {
  color: #cbd5e1;
  font-size: 1rem;
  line-height: 1;
}

/* ── FEATURED IMAGE ────────────────────────────────────────── */
.featured-image-wrapper {
  background: #ffffff;
  padding: 0.75rem 0 0;
}

.featured-image-card {
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 6px 24px rgba(0, 0, 0, 0.07);
}

.featured-img {
  width: 100%;
  max-height: 480px;
  object-fit: cover;
  display: block;
}

.image-caption-text {
  background: #f8fafc;
  border-top: 1px solid #e2e8f0;
  padding: 0.5rem 1rem;
  font-size: 0.75rem;
  color: #94a3b8;
  margin: 0;
  font-style: italic;
}

/* ── CONTENT GRID ──────────────────────────────────────────── */
.content-section {
  padding: 0.85rem 0 5rem;
}

.content-grid-container {
  display: grid;
  grid-template-columns: 1fr 360px;
  gap: 1.5rem;
  align-items: start;
}

/* ── ARTICLE BODY ──────────────────────────────────────────── */
.main-article-body {
  background: #ffffff;
  border-radius: 10px;
  padding: 2rem 2rem 1.5rem;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.04);
  display: flex;
  flex-direction: column;
  gap: 0.55rem;
}

/* Lead paragraph */
.body-lead {
  font-size: 1.05rem;
  line-height: 1.75;
  color: #1e293b;
  font-weight: 600;
  margin: 0;
}

/* Regular paragraph */
.body-paragraph {
  font-size: 1rem;
  line-height: 1.8;
  color: #334155;
  margin: 0;
}

/* Sub-heading */
.body-heading {
  font-size: 1.3rem;
  font-weight: 800;
  color: #0f172a;
  margin: 0.4rem 0 0;
}

/* Quote */
.body-quote {
  border-left: 3px solid #e10600;
  padding: 0.65rem 1.1rem;
  margin: 0;
  background: #fff8f8;
  border-radius: 0 8px 8px 0;
}

.quote-text {
  font-style: italic;
  font-size: 1rem;
  line-height: 1.65;
  color: #1e293b;
  margin: 0 0 0.35rem;
}

.quote-author {
  font-size: 0.78rem;
  font-weight: 700;
  color: #e10600;
  font-style: normal;
  text-transform: uppercase;
  letter-spacing: 0.4px;
}

/* Highlights */
.highlights-box {
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 1.1rem 1.2rem;
}

.highlights-title {
  font-size: 0.88rem;
  font-weight: 700;
  color: #0f172a;
  margin: 0 0 0.75rem;
}

.highlights-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 0.55rem;
}

.highlights-list li {
  display: flex;
  align-items: center;
  gap: 0.55rem;
  font-size: 0.9rem;
  color: #334155;
}

.check-icon {
  width: 16px;
  height: 16px;
  color: #10b981;
  flex-shrink: 0;
}

/* Share footer */
.share-footer {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  padding-top: 1.2rem;
  border-top: 1px solid #e2e8f0;
  flex-wrap: wrap;
}

.share-label {
  font-size: 0.82rem;
  font-weight: 700;
  color: #475569;
}

.share-buttons-row {
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
}

.btn-share {
  display: flex;
  align-items: center;
  gap: 0.35rem;
  border: none;
  padding: 0.42rem 0.9rem;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 700;
  cursor: pointer;
  transition: opacity 0.15s;
}

.btn-share svg { width: 14px; height: 14px; }
.btn-share:hover { opacity: 0.85; }

.share-wa  { background: #25d366; color: #fff; }
.share-fb  { background: #1877f2; color: #fff; }
.share-copy { background: #0f172a; color: #fff; }

/* ── SIDEBAR ───────────────────────────────────────────────── */
.article-sidebar {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  position: sticky;
  top: 90px;
}

.sidebar-card {
  background: #ffffff;
  border: 1px solid #e2e8f0;
  border-radius: 12px;
  padding: 1.4rem;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.04);
}

.cta-card {
  background: linear-gradient(135deg, #0a1128 0%, #1e1b4b 100%);
  border: none;
}

.cta-badge {
  font-size: 0.62rem;
  font-weight: 800;
  letter-spacing: 1.5px;
  color: #fbbf24;
  margin-bottom: 0.5rem;
}

.cta-title {
  font-size: 1.15rem;
  font-weight: 900;
  color: #ffffff;
  line-height: 1.3;
  margin: 0 0 0.5rem;
}

.cta-desc {
  font-size: 0.8rem;
  color: #cbd5e1;
  line-height: 1.5;
  margin: 0 0 1rem;
}

.btn-cta-sidebar {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  background: #e10600;
  color: #ffffff;
  border: none;
  padding: 0.65rem 1rem;
  border-radius: 7px;
  font-size: 0.82rem;
  font-weight: 800;
  cursor: pointer;
  transition: background 0.2s;
}

.btn-cta-sidebar svg { width: 15px; height: 15px; }
.btn-cta-sidebar:hover { background: #b90500; }

.sidebar-card-title {
  font-size: 0.9rem;
  font-weight: 800;
  color: #0f172a;
  margin: 0 0 1rem;
  padding-bottom: 0.6rem;
  border-bottom: 2px solid #e2e8f0;
}

.related-list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.related-item {
  display: flex;
  gap: 0.75rem;
  cursor: pointer;
  transition: transform 0.15s;
}

.related-item:hover { transform: translateX(3px); }

.related-thumb-wrap {
  width: 76px;
  height: 58px;
  border-radius: 7px;
  overflow: hidden;
  flex-shrink: 0;
}

.related-thumb {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.related-info { display: flex; flex-direction: column; }

.related-badge {
  font-size: 0.58rem;
  font-weight: 800;
  letter-spacing: 0.4px;
  text-transform: uppercase;
  margin-bottom: 0.15rem;
}

.related-title {
  font-size: 0.76rem;
  font-weight: 700;
  color: #0f172a;
  line-height: 1.35;
  margin: 0 0 0.2rem;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.related-date { font-size: 0.63rem; color: #94a3b8; }

/* ── RESPONSIVE ────────────────────────────────────────────── */
@media (max-width: 992px) {
  .content-grid-container {
    grid-template-columns: 1fr;
    gap: 2rem;
  }
  .article-sidebar { position: static; }
}

@media (max-width: 640px) {
  .container { padding: 0 1.25rem; }

  /* Breadcrumb */
  .top-breadcrumb-bar { padding: 0.45rem 0; }
  .breadcrumb-list { font-size: 0.68rem; }

  /* Header */
  .article-header-section { padding: 1rem 0 0.75rem; }
  .article-main-title { font-size: 1.05rem; line-height: 1.3; }
  .badge-category { font-size: 0.58rem; padding: 0.22rem 0.6rem; }

  /* Meta row */
  .author-meta-row { gap: 0.3rem; }
  .meta-dot { display: none; }
  .meta-item { font-size: 0.65rem; }
  .meta-item svg { width: 12px; height: 12px; }

  /* Content section */
  .content-section { padding: 0.75rem 0 5rem; }

  /* Article body */
  .main-article-body { padding: 0.9rem 0.85rem; gap: 0.5rem; }
  .body-lead { font-size: 0.8rem; line-height: 1.65; }
  .body-paragraph { font-size: 0.78rem; line-height: 1.65; }
  .body-heading { font-size: 0.88rem; margin: 0.3rem 0 0; }

  /* Quote */
  .body-quote { padding: 0.5rem 0.85rem; }
  .quote-text { font-size: 0.78rem; }
  .quote-author { font-size: 0.65rem; }

  /* Highlights */
  .highlights-box { padding: 0.75rem; }
  .highlights-title { font-size: 0.75rem; }
  .highlights-list li { font-size: 0.73rem; gap: 0.4rem; }
  .check-icon { width: 13px; height: 13px; }

  /* Share footer */
  .share-footer { flex-direction: column; align-items: flex-start; gap: 0.5rem; padding-top: 0.85rem; }
  .share-label { font-size: 0.72rem; }
  .share-buttons-row { width: 100%; }
  .btn-share { flex: 1; justify-content: center; font-size: 0.68rem; padding: 0.38rem 0.6rem; }
  .btn-share svg { width: 12px; height: 12px; }

  /* Sidebar */
  .sidebar-card { padding: 1rem; }
  .sidebar-card-title { font-size: 0.78rem; }
  .related-badge { font-size: 0.52rem; }
  .related-title { font-size: 0.68rem; }
  .related-date { font-size: 0.58rem; }
  .related-thumb-wrap { width: 62px; height: 48px; }

  /* Featured image caption */
  .image-caption-text { font-size: 0.65rem; padding: 0.4rem 0.75rem; }
}
</style>
