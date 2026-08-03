<script setup>
import { computed } from 'vue'
import logoImg from '../assets/images/M (1).png'

const props = defineProps({
  data: {
    type: Object,
    default: () => ({})
  }
})

// ── Computed helpers ────────────────────────────────────────
const athleteName  = computed(() => (props.data?.athlete?.fullName || 'MUHAMMAD ALFAREZ').toUpperCase())
const email        = computed(() => props.data?.buyer?.email        || '-')
const phone        = computed(() => props.data?.athlete?.athletePhone || props.data?.buyer?.phone || '-')
const parentPhone  = computed(() => props.data?.athlete?.parentPhone  || '-')
const memberNo     = computed(() => props.data?.athlete?.memberNo     || 'PBVM-26-0001')
const status       = computed(() => props.data?.athlete?.status       || 'ATLET')
const gender       = computed(() => props.data?.athlete?.gender       || '-')
const dob          = computed(() => props.data?.athlete?.dob          || '-')
const birthPlace   = computed(() => props.data?.athlete?.birthPlace   || '-')
const height       = computed(() => props.data?.athlete?.height       ? props.data.athlete.height + ' cm' : '-')
const weight       = computed(() => props.data?.athlete?.weight       ? props.data.athlete.weight + ' kg' : '-')
const address      = computed(() => props.data?.athlete?.address      || '-')
const education    = computed(() => props.data?.athlete?.education    || '-')
const achievements = computed(() => props.data?.athlete?.achievements || '-')
const jerseyName   = computed(() => props.data?.athlete?.jerseyName   || '-')
const shirtSize    = computed(() => props.data?.athlete?.shirtSize    || '-')
const pantsSize    = computed(() => props.data?.athlete?.pantsSize    || '-')
const validThru    = computed(() => props.data?.athlete?.validThru    || '2026 - 2028')
const photoUrl     = computed(() => props.data?.athlete?.photoUrl     || null)
const regDate      = computed(() => props.data?.registration?.date    || new Date().toLocaleDateString('id-ID', { day: '2-digit', month: 'long', year: 'numeric' }))
const regTime      = computed(() => props.data?.registration?.time    || '-')
const invoiceNo    = computed(() => props.data?.registration?.invoiceNo || 'INV-2026-08-0001')
const totalPayment = computed(() => {
  const v = props.data?.totalPayment || 150000
  return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', minimumFractionDigits: 0 }).format(v)
})

const triggerPrint = () => window.print()
</script>

<template>
  <div class="invoice-wrapper">



    <!-- ══════════════════════════════════════════════════════
         PRINTABLE A4 PAPER
         ══════════════════════════════════════════════════════ -->
    <div id="printable-card-area">
      <div class="a4-paper">
        <div class="watermark-bg">
          <img :src="logoImg" alt="" />
        </div>

        <!-- ╔══════════════════════════════════╗
             ║  HEADER BANNER                  ║
             ╚══════════════════════════════════╝ -->
        <header class="paper-header">

          <!-- Decorative shape images positioned inside header -->
          <img src="/invoice/shape (6).png" class="hdr-shape hdr-shape-6" alt="" />
          <img src="/invoice/shape (2).png" class="hdr-shape hdr-shape-2" alt="" />
          <img src="/invoice/shape (4).png" class="hdr-shape hdr-shape-4" alt="" />

          <div class="hdr-inner">
            <!-- Left: org identity -->
            <div class="hdr-left">
              <img :src="logoImg" alt="Logo PBV Maluku" class="hdr-logo" />
              <div class="hdr-org">
                <p class="hdr-org-sub">PERKUMPULAN BOLA VOLI</p>
                <h1 class="hdr-org-name">PBV. <span class="red">MALUKU</span></h1>
                <p class="hdr-org-address">Kota Ambon, Provinsi Maluku, Indonesia</p>
              </div>
            </div>

            <!-- Right: ticket type + meta -->
            <div class="hdr-right">
              <div class="hdr-ticket-label">KARTU ANGGOTA</div>
              <div class="hdr-meta-row">
                <svg fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/></svg>
                <span>Berlaku: <strong>{{ validThru }}</strong></span>
              </div>
              <div class="hdr-meta-row">
                <svg fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>
                <span>{{ regDate }} — {{ regTime }}</span>
              </div>
              <div class="hdr-meta-row">
                <svg fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"/></svg>
                <span>Kota Ambon, Maluku</span>
              </div>
              <div class="hdr-meta-row">
                <svg fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 7h.01M7 3h5c.512 0 1.024.195 1.414.586l7 7a2 2 0 010 2.828l-5 5a2 2 0 01-2.828 0l-7-7A1.994 1.994 0 013 10V5a2 2 0 012-2z"/></svg>
                <span>No. Invoice: <strong>{{ invoiceNo }}</strong></span>
              </div>
            </div>
          </div>

          <!-- Member No banner strip -->
          <div class="hdr-strip">
            <span class="hdr-strip-no">{{ memberNo }}</span>
            <span class="hdr-strip-sep">•</span>
            <span class="hdr-strip-name">{{ athleteName }}</span>
            <span class="hdr-strip-sep">•</span>
            <span class="hdr-strip-status">{{ status }}</span>
          </div>
        </header>

        <!-- ╔══════════════════════════════════╗
             ║  SECTION: DETAIL PENDAFTARAN    ║
             ╚══════════════════════════════════╝ -->
        <section class="paper-section">
          <div class="section-header-row">
            <h2 class="section-title">
              <svg fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"/></svg>
              Detail Pendaftaran Atlet
            </h2>
            <button class="btn-download-inline no-print" @click="triggerPrint">
              <svg fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4" />
              </svg>
              Unduh PDF
            </button>
          </div>

          <div class="detail-grid">
            <!-- LEFT: Data table -->
            <div class="detail-table">

              <div class="detail-group">
                <span class="detail-label">Nama Lengkap</span>
                <span class="detail-sep">:</span>
                <span class="detail-value bold">{{ athleteName }}</span>
              </div>

              <div class="detail-group">
                <span class="detail-label">No. Anggota</span>
                <span class="detail-sep">:</span>
                <span class="detail-value red bold">{{ memberNo }}</span>
              </div>

              <div class="detail-group">
                <span class="detail-label">Status</span>
                <span class="detail-sep">:</span>
                <span class="detail-value"><span class="status-pill">{{ status }}</span></span>
              </div>

              <div class="detail-group">
                <span class="detail-label">Jenis Kelamin</span>
                <span class="detail-sep">:</span>
                <span class="detail-value">{{ gender }}</span>
              </div>

              <div class="detail-group">
                <span class="detail-label">Tanggal Lahir</span>
                <span class="detail-sep">:</span>
                <span class="detail-value">{{ dob }}</span>
              </div>

              <div class="detail-group">
                <span class="detail-label">Tempat Lahir</span>
                <span class="detail-sep">:</span>
                <span class="detail-value">{{ birthPlace }}</span>
              </div>

              <div class="detail-group">
                <span class="detail-label">Tinggi / Berat</span>
                <span class="detail-sep">:</span>
                <span class="detail-value">{{ height }} / {{ weight }}</span>
              </div>

              <div class="detail-group">
                <span class="detail-label">Pendidikan</span>
                <span class="detail-sep">:</span>
                <span class="detail-value">{{ education }}</span>
              </div>

              <div class="detail-group">
                <span class="detail-label">Email</span>
                <span class="detail-sep">:</span>
                <span class="detail-value">{{ email }}</span>
              </div>

              <div class="detail-group">
                <span class="detail-label">No. HP Atlet</span>
                <span class="detail-sep">:</span>
                <span class="detail-value">{{ phone }}</span>
              </div>

              <div class="detail-group">
                <span class="detail-label">No. HP Orang Tua</span>
                <span class="detail-sep">:</span>
                <span class="detail-value">{{ parentPhone }}</span>
              </div>

              <div class="detail-group">
                <span class="detail-label">Alamat</span>
                <span class="detail-sep">:</span>
                <span class="detail-value">{{ address }}</span>
              </div>

              <!-- Jersey row -->
              <div class="detail-divider"></div>

              <div class="detail-group">
                <span class="detail-label">Nama Jersey</span>
                <span class="detail-sep">:</span>
                <span class="detail-value bold">{{ jerseyName }}</span>
              </div>

              <div class="detail-group">
                <span class="detail-label">Ukuran Baju / Celana</span>
                <span class="detail-sep">:</span>
                <span class="detail-value">{{ shirtSize }} / {{ pantsSize }}</span>
              </div>

              <div class="detail-group">
                <span class="detail-label">Masa Berlaku</span>
                <span class="detail-sep">:</span>
                <span class="detail-value bold">{{ validThru }}</span>
              </div>

              <div class="detail-group" v-if="achievements && achievements !== '-'">
                <span class="detail-label">Riwayat Prestasi</span>
                <span class="detail-sep">:</span>
                <span class="detail-value">{{ achievements }}</span>
              </div>

              <div class="detail-divider"></div>

              <div class="detail-group total-row">
                <span class="detail-label">Biaya Pendaftaran</span>
                <span class="detail-sep">:</span>
                <span class="detail-value red bold">{{ totalPayment }}</span>
              </div>
            </div>

            <!-- RIGHT: Photo + QR -->
            <div class="detail-right">
              <!-- Photo -->
              <div class="photo-box">
                <img v-if="photoUrl" :src="photoUrl" alt="Foto Atlet" class="photo-img" />
                <div v-else class="photo-placeholder">
                  <svg fill="currentColor" viewBox="0 0 24 24">
                    <path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z"/>
                  </svg>
                  <span>Foto Atlet</span>
                </div>
              </div>

              <!-- QR code placeholder -->
              <div class="qr-box">
                <svg viewBox="0 0 100 100" class="qr-svg" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <!-- Finder top-left -->
                  <rect x="5" y="5" width="30" height="30" rx="3" fill="none" stroke="#0f172a" stroke-width="3"/>
                  <rect x="12" y="12" width="16" height="16" rx="1" fill="#0f172a"/>
                  <!-- Finder top-right -->
                  <rect x="65" y="5" width="30" height="30" rx="3" fill="none" stroke="#0f172a" stroke-width="3"/>
                  <rect x="72" y="12" width="16" height="16" rx="1" fill="#0f172a"/>
                  <!-- Finder bottom-left -->
                  <rect x="5" y="65" width="30" height="30" rx="3" fill="none" stroke="#0f172a" stroke-width="3"/>
                  <rect x="12" y="72" width="16" height="16" rx="1" fill="#0f172a"/>
                  <!-- Data modules (decorative) -->
                  <rect x="42" y="5"  width="5" height="5" fill="#0f172a"/>
                  <rect x="50" y="5"  width="5" height="5" fill="#0f172a"/>
                  <rect x="58" y="5"  width="5" height="5" fill="#0f172a"/>
                  <rect x="42" y="12" width="5" height="5" fill="#0f172a"/>
                  <rect x="50" y="12" width="5" height="5" fill="#0f172a"/>
                  <rect x="42" y="20" width="5" height="5" fill="#0f172a"/>
                  <rect x="58" y="20" width="5" height="5" fill="#0f172a"/>
                  <rect x="42" y="28" width="5" height="5" fill="#0f172a"/>
                  <rect x="50" y="28" width="5" height="5" fill="#0f172a"/>
                  <rect x="42" y="42" width="5" height="5" fill="#0f172a"/>
                  <rect x="50" y="42" width="5" height="5" fill="#0f172a"/>
                  <rect x="58" y="42" width="5" height="5" fill="#0f172a"/>
                  <rect x="65" y="42" width="5" height="5" fill="#0f172a"/>
                  <rect x="73" y="42" width="5" height="5" fill="#0f172a"/>
                  <rect x="80" y="42" width="5" height="5" fill="#0f172a"/>
                  <rect x="88" y="42" width="5" height="5" fill="#0f172a"/>
                  <rect x="42" y="50" width="5" height="5" fill="#0f172a"/>
                  <rect x="65" y="50" width="5" height="5" fill="#0f172a"/>
                  <rect x="80" y="50" width="5" height="5" fill="#0f172a"/>
                  <rect x="42" y="58" width="5" height="5" fill="#0f172a"/>
                  <rect x="50" y="58" width="5" height="5" fill="#0f172a"/>
                  <rect x="58" y="58" width="5" height="5" fill="#0f172a"/>
                  <rect x="73" y="58" width="5" height="5" fill="#0f172a"/>
                  <rect x="88" y="58" width="5" height="5" fill="#0f172a"/>
                  <rect x="5"  y="42" width="5" height="5" fill="#0f172a"/>
                  <rect x="12" y="42" width="5" height="5" fill="#0f172a"/>
                  <rect x="20" y="42" width="5" height="5" fill="#0f172a"/>
                  <rect x="28" y="42" width="5" height="5" fill="#0f172a"/>
                  <rect x="35" y="42" width="5" height="5" fill="#0f172a"/>
                  <rect x="5"  y="50" width="5" height="5" fill="#0f172a"/>
                  <rect x="20" y="50" width="5" height="5" fill="#0f172a"/>
                  <rect x="35" y="50" width="5" height="5" fill="#0f172a"/>
                  <rect x="5"  y="58" width="5" height="5" fill="#0f172a"/>
                  <rect x="12" y="58" width="5" height="5" fill="#0f172a"/>
                  <rect x="28" y="58" width="5" height="5" fill="#0f172a"/>
                  <rect x="50" y="65" width="5" height="5" fill="#0f172a"/>
                  <rect x="65" y="65" width="5" height="5" fill="#0f172a"/>
                  <rect x="80" y="65" width="5" height="5" fill="#0f172a"/>
                  <rect x="42" y="73" width="5" height="5" fill="#0f172a"/>
                  <rect x="58" y="73" width="5" height="5" fill="#0f172a"/>
                  <rect x="73" y="73" width="5" height="5" fill="#0f172a"/>
                  <rect x="88" y="73" width="5" height="5" fill="#0f172a"/>
                  <rect x="50" y="80" width="5" height="5" fill="#0f172a"/>
                  <rect x="65" y="80" width="5" height="5" fill="#0f172a"/>
                  <rect x="42" y="88" width="5" height="5" fill="#0f172a"/>
                  <rect x="58" y="88" width="5" height="5" fill="#0f172a"/>
                  <rect x="73" y="88" width="5" height="5" fill="#0f172a"/>
                  <rect x="88" y="88" width="5" height="5" fill="#0f172a"/>
                </svg>
                <span class="qr-label">{{ memberNo }}</span>
              </div>
            </div>
          </div>


        </section>

        <!-- ── Section footer (page 1) ── -->
        <footer class="paper-footer">
          <div class="footer-centered-content">
            <div class="footer-org-name">pbv maluku</div>
            <div class="footer-contact-line">
              For any inquiries, contact us at: <strong>pbvmaluku@official.id</strong> | Website: <strong>www.pbvmaluku.id</strong>
            </div>
            <img :src="logoImg" alt="" class="footer-centered-logo" />
          </div>
        </footer>
      </div>

      <!-- ══ PAGE BREAK ══ -->
      <div class="page-break"></div>

      <!-- PAPER 2: SYARAT & KETENTUAN -->
      <div class="a4-paper">
        <div class="watermark-bg">
          <img :src="logoImg" alt="" />
        </div>
        <!-- ╔══════════════════════════════════╗
             ║  PAGE 2: SYARAT & KETENTUAN     ║
             ╚══════════════════════════════════╝ -->
        <div class="page2">

          <!-- Small header repeat -->
          <div class="page2-header">
            <img :src="logoImg" alt="PBV Maluku" class="page2-logo" />
            <div>
              <p class="page2-org">PBV. MALUKU — Perkumpulan Bola Voli Maluku</p>
              <p class="page2-invoice-ref">Invoice Ref: {{ invoiceNo }} | Anggota: {{ memberNo }}</p>
            </div>
          </div>

          <!-- Syarat & Ketentuan -->
          <section class="paper-section sk-section">
            <h2 class="section-title">
              <svg fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-6 9l2 2 4-4"/></svg>
              Syarat &amp; Ketentuan Keanggotaan
            </h2>

            <ol class="sk-list">
              <li>Kartu anggota ini wajib dibawa saat mengikuti seluruh kegiatan resmi PBV. Maluku. Tunjukkan kartu ini kepada panitia untuk mendapatkan akses ke sesi latihan dan pertandingan.</li>
              <li>Kartu anggota hanya berlaku sesuai periode yang tertera. Perpanjangan keanggotaan wajib dilakukan sebelum tanggal berakhir agar atlet tetap terdaftar aktif dalam sistem PBV. Maluku.</li>
              <li>Dilarang meminjamkan kartu anggota kepada pihak lain. Apabila terbukti terjadi penyalahgunaan kartu, maka keanggotaan dapat dicabut tanpa pengembalian biaya pendaftaran.</li>
              <li>Pengurus mempunyai hak penuh untuk menolak pendaftaran apabila data yang diisikan tidak sesuai dengan dokumen resmi yang diunggah. Pemalsuan data dapat dikenakan sanksi sesuai peraturan yang berlaku.</li>
              <li>Biaya pendaftaran yang sudah dibayarkan tidak dapat dikembalikan (<em>non-refundable</em>) kecuali atas keputusan khusus dari pengurus PBV. Maluku.</li>
              <li>Atlet yang terdaftar wajib mengikuti ketentuan disiplin, etika olahraga, dan tata tertib yang ditetapkan oleh PBV. Maluku selama masa keanggotaan berlangsung.</li>
              <li>Setiap perubahan data diri (nomor HP, alamat, dll) wajib dilaporkan kepada sekretariat PBV. Maluku paling lambat 7 (tujuh) hari setelah perubahan terjadi.</li>
            </ol>
          </section>

          <!-- Signature block -->
          <div class="sig-block">
            <div class="sig-left">
              <p class="sig-note">Disetujui dan diterbitkan oleh:</p>
              <div class="sig-space"></div>
              <p class="sig-name">Ketua Umum PBV. MALUKU</p>
              <p class="sig-date">Ambon, {{ regDate }}</p>
            </div>
            <div class="sig-right">
              <p class="sig-note">Tanda tangan atlet / wali:</p>
              <div class="sig-space"></div>
              <p class="sig-name">{{ athleteName }}</p>
              <p class="sig-date">Tanggal: ___________________________</p>
            </div>
          </div>

          <!-- Shape decoration bottom -->
          <img src="/invoice/shape (1).png" class="p2-shape p2-shape-1" alt="" />
          <img src="/invoice/shape (5).png" class="p2-shape p2-shape-5" alt="" />

          <!-- Page 2 footer -->
          <footer class="paper-footer">
            <div class="footer-centered-content">
              <div class="footer-org-name">pbv maluku</div>
              <div class="footer-contact-line">
                For any inquiries, contact us at: <strong>pbvmaluku@official.id</strong> | Website: <strong>www.pbvmaluku.id</strong>
              </div>
              <img :src="logoImg" alt="" class="footer-centered-logo" />
            </div>
          </footer>

        </div>
      </div>
    </div>

    <!-- ══════════════════════════════════════════════════════
         PRINT-ONLY CR80 PORTRAIT ID CARDS (54mm x 85.6mm)
         ══════════════════════════════════════════════════════ -->
    <div class="print-only-id-cards">
      <!-- FRONT SIDE -->
      <div class="print-card print-card-front">
        <div class="pfront-bg"></div>

        <!-- Shapes -->
        <img src="/invoice/shape (6).png" class="pshape pshape-6" alt="" />
        <img src="/invoice/shape (4).png" class="pshape pshape-4" alt="" />
        <img src="/invoice/shape (2).png" class="pshape pshape-2" alt="" />
        <div class="pwatermark-text">PBVM</div>
        <img src="/invoice/shape (3).png" class="pshape pshape-3" alt="" />
        <img src="/invoice/shape (1).png" class="pshape pshape-1" alt="" />

        <!-- Content -->
        <div class="pfront-content">
          <div class="pfront-header">
            <img :src="logoImg" alt="" class="pheader-logo" />
            <p class="psub-title">PERKUMPULAN BOLA VOLI</p>
            <h2 class="pmain-title">PBV. <span class="pred">MALUKU</span></h2>
            <div class="pstars">★ ★ ★</div>
          </div>

          <div class="pphoto-frame-wrap">
            <div class="pphoto-frame">
              <img v-if="photoUrl" :src="photoUrl" alt="" class="pphoto-img" />
              <div v-else class="pphoto-placeholder">
                <svg fill="currentColor" viewBox="0 0 24 24"><path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z"/></svg>
              </div>
            </div>
          </div>

          <div class="pdata-table">
            <div class="pdata-row">
              <span class="pdata-label">NAMA</span>
              <span class="pdata-sep">:</span>
              <span class="pdata-value pbold">{{ athleteName }}</span>
            </div>
            <div class="pdata-row">
              <span class="pdata-label">NO. ANGGOTA</span>
              <span class="pdata-sep">:</span>
              <span class="pdata-value pbold pred">{{ memberNo }}</span>
            </div>
            <div class="pdata-row">
              <span class="pdata-label">STATUS</span>
              <span class="pdata-sep">:</span>
              <span class="pdata-value"><span class="pbadge-navy">{{ status }}</span></span>
            </div>
            <div class="pdata-row">
              <span class="pdata-label">JABATAN</span>
              <span class="pdata-sep">:</span>
              <span class="pdata-value">{{ position }}</span>
            </div>
            <div class="pdata-row">
              <span class="pdata-label">BERLAKU</span>
              <span class="pdata-sep">:</span>
              <span class="pdata-value pbold">{{ validThru }}</span>
            </div>
          </div>

          <div class="pbarcode-wrap">
            <svg viewBox="0 0 210 32" class="pbarcode-svg">
              <rect x="4" y="0" width="2" height="32" fill="#000"/><rect x="8" y="0" width="1" height="32" fill="#000"/>
              <rect x="11" y="0" width="3" height="32" fill="#000"/><rect x="17" y="0" width="2" height="32" fill="#000"/>
              <rect x="21" y="0" width="1" height="32" fill="#000"/><rect x="25" y="0" width="4" height="32" fill="#000"/>
              <rect x="32" y="0" width="2" height="32" fill="#000"/><rect x="37" y="0" width="3" height="32" fill="#000"/>
              <rect x="42" y="0" width="1" height="32" fill="#000"/><rect x="46" y="0" width="4" height="32" fill="#000"/>
              <rect x="53" y="0" width="2" height="32" fill="#000"/><rect x="57" y="0" width="2" height="32" fill="#000"/>
              <rect x="62" y="0" width="1" height="32" fill="#000"/><rect x="65" y="0" width="3" height="32" fill="#000"/>
              <rect x="71" y="0" width="2" height="32" fill="#000"/><rect x="76" y="0" width="4" height="32" fill="#000"/>
              <rect x="83" y="0" width="1" height="32" fill="#000"/><rect x="86" y="0" width="3" height="32" fill="#000"/>
              <rect x="91" y="0" width="2" height="32" fill="#000"/><rect x="96" y="0" width="4" height="32" fill="#000"/>
              <rect x="103" y="0" width="1" height="32" fill="#000"/><rect x="106" y="0" width="2" height="32" fill="#000"/>
              <rect x="111" y="0" width="3" height="32" fill="#000"/><rect x="117" y="0" width="1" height="32" fill="#000"/>
              <rect x="121" y="0" width="4" height="32" fill="#000"/><rect x="128" y="0" width="2" height="32" fill="#000"/>
              <rect x="133" y="0" width="3" height="32" fill="#000"/><rect x="138" y="0" width="1" height="32" fill="#000"/>
              <rect x="141" y="0" width="4" height="32" fill="#000"/><rect x="148" y="0" width="2" height="32" fill="#000"/>
              <rect x="153" y="0" width="1" height="32" fill="#000"/><rect x="156" y="0" width="3" height="32" fill="#000"/>
              <rect x="162" y="0" width="4" height="32" fill="#000"/><rect x="169" y="0" width="1" height="32" fill="#000"/>
              <rect x="172" y="0" width="2" height="32" fill="#000"/><rect x="177" y="0" width="3" height="32" fill="#000"/>
              <rect x="183" y="0" width="1" height="32" fill="#000"/><rect x="187" y="0" width="4" height="32" fill="#000"/>
              <rect x="194" y="0" width="2" height="32" fill="#000"/><rect x="199" y="0" width="2" height="32" fill="#000"/>
              <rect x="203" y="0" width="1" height="32" fill="#000"/><rect x="206" y="0" width="3" height="32" fill="#000"/>
            </svg>
            <span class="pbarcode-label">{{ memberNo }}</span>
          </div>
        </div>

        <div class="pfront-footer">
          <img src="/invoice/shape (5).png" class="pshape pshape-5" alt="" />
          <span class="pmotto">SOLID • <span class="pmotto-gold">SPORTIF</span> • BERPRESTASI</span>
        </div>
      </div>

      <!-- BACK SIDE -->
      <div class="print-card print-card-back">
        <div class="pback-bg"></div>

        <!-- Shapes decoration on back -->
        <img src="/invoice/shape (6).png" class="pshape pshape-6 opacity-40" alt="" />
        <img src="/invoice/shape (1).png" class="pshape pshape-1 opacity-20" alt="" />
        <img src="/invoice/shape (5).png" class="pshape pshape-5 opacity-15" alt="" />

        <div class="pback-content">
          <div class="pback-header">
            <img :src="logoImg" alt="" class="pback-logo" />
            <h3 class="pback-title">PBV. MALUKU</h3>
            <p class="pback-subtitle">KETENTUAN KEANGGOTAAN</p>
          </div>

          <ul class="pback-rules">
            <li>Kartu wajib dibawa saat latihan &amp; pertandingan.</li>
            <li>Kartu hanya berlaku untuk atlet bersangkutan.</li>
            <li>Dilarang meminjamkan/menyalahgunakan kartu ini.</li>
            <li>Segala pelanggaran dapat dikenakan sanksi disiplin.</li>
            <li>Kehilangan kartu harap segera lapor sekretariat.</li>
            <li>Informasi lebih lanjut hubungi email: pbvmaluku@official.id</li>
          </ul>

          <div class="pback-qr-wrap">
            <svg viewBox="0 0 100 100" class="pback-qr-svg" fill="none" xmlns="http://www.w3.org/2000/svg">
              <rect x="5" y="5" width="30" height="30" rx="3" fill="none" stroke="#ffffff" stroke-width="3"/>
              <rect x="12" y="12" width="16" height="16" rx="1" fill="#ffffff"/>
              <rect x="65" y="5" width="30" height="30" rx="3" fill="none" stroke="#ffffff" stroke-width="3"/>
              <rect x="72" y="12" width="16" height="16" rx="1" fill="#ffffff"/>
              <rect x="5" y="65" width="30" height="30" rx="3" fill="none" stroke="#ffffff" stroke-width="3"/>
              <rect x="12" y="72" width="16" height="16" rx="1" fill="#ffffff"/>
              <rect x="42" y="5"  width="5" height="5" fill="#ffffff"/><rect x="50" y="5"  width="5" height="5" fill="#ffffff"/>
              <rect x="42" y="12" width="5" height="5" fill="#ffffff"/><rect x="50" y="12" width="5" height="5" fill="#ffffff"/>
              <rect x="42" y="20" width="5" height="5" fill="#ffffff"/><rect x="58" y="20" width="5" height="5" fill="#ffffff"/>
              <rect x="42" y="28" width="5" height="5" fill="#ffffff"/><rect x="50" y="28" width="5" height="5" fill="#ffffff"/>
              <rect x="42" y="42" width="5" height="5" fill="#ffffff"/><rect x="58" y="42" width="5" height="5" fill="#ffffff"/>
              <rect x="42" y="50" width="5" height="5" fill="#ffffff"/><rect x="65" y="50" width="5" height="5" fill="#ffffff"/>
              <rect x="5"  y="42" width="5" height="5" fill="#ffffff"/><rect x="20" y="42" width="5" height="5" fill="#ffffff"/>
              <rect x="50" y="65" width="5" height="5" fill="#ffffff"/><rect x="80" y="65" width="5" height="5" fill="#ffffff"/>
              <rect x="42" y="73" width="5" height="5" fill="#ffffff"/><rect x="73" y="73" width="5" height="5" fill="#ffffff"/>
              <rect x="58" y="88" width="5" height="5" fill="#ffffff"/><rect x="88" y="88" width="5" height="5" fill="#ffffff"/>
            </svg>
            <span class="pback-qr-label">{{ memberNo }}</span>
          </div>

          <div class="pback-footer">
            <span>www.pbvmaluku.id</span>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<style scoped>
/* ═══════════════════════════════════════════════════════════
   WRAPPER & DOWNLOAD BUTTON
   ═══════════════════════════════════════════════════════════ */
.invoice-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
  width: 100%;
}

.action-bar {
  display: flex;
  justify-content: center;
}

.btn-download {
  display: flex;
  align-items: center;
  gap: 0.55rem;
  background: #0f172a;
  color: #fff;
  border: none;
  padding: 0.75rem 2rem;
  border-radius: 10px;
  font-size: 0.92rem;
  font-weight: 700;
  cursor: pointer;
  box-shadow: 0 4px 16px rgba(15, 23, 42, 0.3);
  transition: all 0.2s ease;
}

.btn-download svg { width: 20px; height: 20px; }
.btn-download:hover { background: #1e293b; transform: translateY(-2px); }

/* ── PRINTABLE CARD AREA CONTAINER ── */
#printable-card-area {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem; /* Gap between papers on screen */
  width: 100%;
}

/* ═══════════════════════════════════════════════════════════
   A4 PAPER
   ═══════════════════════════════════════════════════════════ */
.a4-paper {
  width: 794px;
  background: #ffffff;
  box-shadow: 0 8px 40px rgba(0,0,0,0.18);
  font-family: 'Inter', system-ui, sans-serif;
  font-size: 13px;
  line-height: 1.5;
  border-radius: 4px;
  overflow: hidden;
  position: relative;
}

/* ═══════════════════════════════════════════════════════════
   HEADER BANNER
   ═══════════════════════════════════════════════════════════ */
.paper-header {
  background: linear-gradient(135deg, #0a1128 0%, #0f1f4a 60%, #14002d 100%);
  position: relative;
  overflow: hidden;
  padding: 0;
}

/* Shape overlays */
.hdr-shape {
  position: absolute;
  pointer-events: none;
  z-index: 1;
}

.hdr-shape-6 {
  top: 0; left: 0;
  width: 25%;
  opacity: 0.7;
}

.hdr-shape-2 {
  top: 0; right: 0;
  width: 45%;
  opacity: 0.8;
}

.hdr-shape-4 {
  top: 0; right: 0;
  width: 30%;
  opacity: 0.6;
}

/* Header inner row */
.hdr-inner {
  position: relative;
  z-index: 10;
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  padding: 22px 28px 18px;
  gap: 1rem;
}

/* Left: logo + org */
.hdr-left {
  display: flex;
  align-items: center;
  gap: 14px;
}

.hdr-logo {
  width: 64px;
  height: 64px;
  object-fit: contain;
  filter: drop-shadow(0 2px 8px rgba(0,0,0,0.6));
  flex-shrink: 0;
}

.hdr-org-sub {
  font-size: 0.6rem;
  font-weight: 700;
  letter-spacing: 1.5px;
  color: #ffffff;
  text-transform: uppercase;
  margin: 0 0 2px;
}

.hdr-org-name {
  font-family: 'Outfit', 'Inter', sans-serif;
  font-size: 1.9rem;
  font-weight: 700;
  color: #ffffff;
  margin: 0;
  letter-spacing: -0.5px;
  line-height: 1;
  font-style: normal;
  text-shadow: 0 2px 8px rgba(0,0,0,0.5);
}

.hdr-org-name .red { color: #ffffff !important; }

.hdr-org-address {
  font-size: 0.65rem;
  color: #ffffff;
  opacity: 0.8;
  margin: 3px 0 0;
}

/* Right: ticket label + meta */
.hdr-right {
  text-align: right;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 5px;
}

.hdr-ticket-label {
  font-family: 'Outfit', 'Inter', sans-serif;
  font-size: 1.4rem;
  font-weight: 900;
  color: #ffffff;
  letter-spacing: 1px;
  text-transform: uppercase;
}

.hdr-meta-row {
  display: flex;
  align-items: center;
  gap: 5px;
  justify-content: flex-end;
  color: #ffffff;
  font-size: 0.72rem;
}

.hdr-meta-row svg { width: 13px; height: 13px; flex-shrink: 0; }
.hdr-meta-row strong { color: #ffffff; }

/* Member no strip */
.hdr-strip {
  position: relative;
  z-index: 10;
  background: rgba(225, 6, 0, 0.85);
  padding: 7px 28px;
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 0.78rem;
  font-weight: 700;
  color: #ffffff;
  letter-spacing: 0.5px;
}

.hdr-strip-no   { font-family: monospace; font-size: 0.85rem; letter-spacing: 2px; }
.hdr-strip-sep  { opacity: 0.5; }
.hdr-strip-name { flex: 1; font-weight: 800; }
.hdr-strip-status {
  background: rgba(255,255,255,0.2);
  padding: 1px 10px;
  border-radius: 20px;
  font-size: 0.7rem;
  letter-spacing: 0.5px;
}

/* ═══════════════════════════════════════════════════════════
   SECTION COMMON
   ═══════════════════════════════════════════════════════════ */
.paper-section {
  padding: 20px 28px;
}

.section-header-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 2px solid #e2e8f0;
  padding-bottom: 8px;
  margin-bottom: 14px;
}

.section-title {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 0.82rem;
  font-weight: 800;
  color: #0f172a;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin: 0;
}

.section-title svg {
  width: 16px;
  height: 16px;
  color: #e10600;
  flex-shrink: 0;
}

.btn-download-inline {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  background: #2563eb;
  color: #ffffff;
  border: 1px solid #2563eb;
  padding: 0.4rem 1rem;
  border-radius: 6px;
  font-size: 0.72rem;
  font-weight: 700;
  cursor: pointer;
  box-shadow: 0 1px 3px rgba(37, 99, 235, 0.2);
  transition: all 0.2s ease;
}

.btn-download-inline svg {
  width: 14px;
  height: 14px;
}

.btn-download-inline:hover {
  background: #1d4ed8;
  border-color: #1d4ed8;
  color: #ffffff;
}

/* ═══════════════════════════════════════════════════════════
   DETAIL GRID (table + photo/QR)
   ═══════════════════════════════════════════════════════════ */
.detail-grid {
  display: flex;
  gap: 20px;
  align-items: flex-start;
}

/* ── Data table (left) ── */
.detail-table {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 0;
}

.detail-group {
  display: flex;
  align-items: baseline;
  gap: 6px;
  padding: 5px 8px;
  border-radius: 5px;
  transition: background 0.15s;
}

.detail-group:nth-child(odd) { background: #f8fafc; }

.detail-label {
  font-weight: 700;
  min-width: 145px;
  font-size: 0.73rem;
  color: #475569;
  flex-shrink: 0;
}

.detail-sep {
  font-weight: 700;
  color: #94a3b8;
  flex-shrink: 0;
}

.detail-value {
  flex: 1;
  font-size: 0.78rem;
  color: #0f172a;
  word-break: break-word;
}

.detail-value.bold { font-weight: 800; }
.detail-value.red  { color: #e10600; }

.status-pill {
  background: #0f172a;
  color: #fff;
  font-size: 0.65rem;
  font-weight: 900;
  padding: 1px 10px;
  border-radius: 20px;
  letter-spacing: 0.5px;
}

.detail-divider {
  height: 1px;
  background: #e2e8f0;
  margin: 6px 0;
}

.total-row .detail-label { font-weight: 800; color: #0f172a; }
.total-row .detail-value { font-size: 0.88rem; }

/* ── Photo + QR (right) ── */
.detail-right {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
  flex-shrink: 0;
  width: 148px;
}

.photo-box {
  width: 120px;
  height: 150px;
  border: 2px solid #e2e8f0;
  border-radius: 8px;
  overflow: hidden;
  background: #f1f5f9;
  display: flex;
  align-items: center;
  justify-content: center;
}

.photo-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.photo-placeholder {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  color: #94a3b8;
  font-size: 0.62rem;
  font-weight: 600;
}

.photo-placeholder svg { width: 36px; height: 36px; }

.qr-box {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  border: 1.5px solid #e2e8f0;
  border-radius: 8px;
  padding: 8px;
  background: #fff;
}

.qr-svg {
  width: 100px;
  height: 100px;
}

.qr-label {
  font-size: 0.58rem;
  font-weight: 700;
  letter-spacing: 1px;
  color: #475569;
  font-family: monospace;
}

/* ── Achievements ── */
.achievements-row {
  margin-top: 10px;
  padding: 10px 12px;
  background: #f8fafc;
  border-radius: 6px;
  border-left: 3px solid #e10600;
}

.achievements-row .detail-label {
  font-weight: 700;
  font-size: 0.73rem;
  color: #475569;
  display: block;
  margin-bottom: 4px;
}

.achievements-text {
  font-size: 0.75rem;
  color: #0f172a;
  margin: 0;
  line-height: 1.6;
}

.paper-footer {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  padding: 20px 28px;
  border-top: 1px solid #e2e8f0;
  background: #ffffff;
}

.footer-centered-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
  z-index: 10;
}

.footer-org-name {
  font-size: 0.8rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: 0.5px;
}

.footer-contact-line {
  font-size: 0.7rem;
  color: #64748b;
  line-height: 1.4;
}

.footer-contact-line strong {
  color: #0f172a;
  font-weight: 600;
}

.footer-centered-logo {
  width: 28px;
  height: 28px;
  object-fit: contain;
  margin-top: 4px;
}

/* ═══════════════════════════════════════════════════════════
   PAGE BREAK
   ═══════════════════════════════════════════════════════════ */
.page-break {
  break-after: page;
  page-break-after: always;
}

/* ═══════════════════════════════════════════════════════════
   PAGE 2
   ═══════════════════════════════════════════════════════════ */
.page2 {
  min-height: 1000px;
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.page2-header {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 14px 28px;
  border-bottom: 2px solid #e2e8f0;
  background: #f8fafc;
}

.page2-logo {
  width: 32px;
  height: 32px;
  object-fit: contain;
}

.page2-org {
  font-size: 0.78rem;
  font-weight: 800;
  color: #0f172a;
  margin: 0;
}

.page2-invoice-ref {
  font-size: 0.65rem;
  color: #64748b;
  margin: 1px 0 0;
  font-family: monospace;
  letter-spacing: 0.5px;
}

/* S&K section */
.sk-section {
  flex: 1;
}

.sk-list {
  margin: 0;
  padding-left: 20px;
  display: flex;
  flex-direction: column;
  gap: 9px;
}

.sk-list li {
  font-size: 0.78rem;
  color: #334155;
  line-height: 1.65;
  padding-left: 4px;
}

.sk-list li::marker {
  color: #e10600;
  font-weight: 800;
}

.sig-block {
  display: none !important;
}

.sig-left, .sig-right {
  flex: 1;
}

.sig-note {
  font-size: 0.72rem;
  color: #475569;
  margin: 0 0 4px;
}

.sig-space {
  height: 48px;
  border-bottom: 1px solid #94a3b8;
  margin-bottom: 6px;
}

.sig-name {
  font-size: 0.75rem;
  font-weight: 700;
  color: #0f172a;
  margin: 0;
}

.sig-date {
  font-size: 0.68rem;
  color: #64748b;
  margin: 2px 0 0;
}

/* Centered Watermark Logo for both papers */
.watermark-bg {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  opacity: 0.035; /* subtle adjusted opacity */
  pointer-events: none;
  z-index: 0;
}

.watermark-bg img {
  width: 320px;
  height: 320px;
  object-fit: contain;
}

/* Page 2 shape decorations */
.p2-shape {
  position: absolute;
  pointer-events: none;
  z-index: 0;
}

.p2-shape-1 {
  bottom: 60px;
  left: -20px;
  width: 30%;
  opacity: 0.25;
}

.p2-shape-5 {
  bottom: 60px;
  right: -30px;
  width: 35%;
  opacity: 0.12;
  transform: scaleX(-1);
}

/* ═══════════════════════════════════════════════════════════
   PRINT-ONLY ID CARDS (54mm x 85.6mm portrait style)
   ═══════════════════════════════════════════════════════════ */
.print-only-id-cards {
  display: none;
}

.print-card {
  width: 54mm;
  height: 85.6mm;
  box-sizing: border-box;
  position: relative;
  overflow: hidden;
  background: #0a1128;
}

.print-card-front {
  background: #0a1128;
}

.pfront-bg {
  position: absolute;
  inset: 0;
  background: linear-gradient(160deg, #0d1535 0%, #091020 100%);
  z-index: 0;
}

.pshape {
  position: absolute;
  z-index: 1;
  pointer-events: none;
}

.pshape-6 {
  top: 0; left: 0;
  width: 25mm;
  opacity: 0.85;
}

.pshape-4 {
  top: 0; right: 0;
  width: 25mm;
  opacity: 0.8;
}

.pshape-2 {
  top: 0; right: 0;
  width: 32mm;
  opacity: 0.9;
}

.pshape-3 {
  top: 38mm; left: -2mm;
  width: 58mm;
}

.pshape-1 {
  bottom: 2mm; left: -2mm;
  width: 28mm;
}

.pshape-5 {
  position: absolute;
  top: 0; left: 0;
  width: 28mm;
  height: 100%;
  object-fit: cover;
}

.pwatermark-text {
  position: absolute;
  bottom: 25mm;
  left: 50%;
  transform: translateX(-50%);
  font-size: 24pt;
  font-weight: 900;
  color: rgba(255, 255, 255, 0.04);
  letter-spacing: 2px;
  z-index: 1;
  pointer-events: none;
  white-space: nowrap;
}

.pfront-content {
  position: relative;
  z-index: 10;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 80mm;
  padding-top: 3mm;
}

.pheader-logo {
  width: 10mm;
  height: 10mm;
  object-fit: contain;
}

.psub-title {
  font-size: 3.5pt;
  font-weight: 700;
  letter-spacing: 0.5px;
  color: #cbd5e1;
  margin: 1px 0 0;
  text-transform: uppercase;
}

.pmain-title {
  font-family: 'Outfit', sans-serif;
  font-size: 8pt;
  font-weight: 800;
  color: #ffffff;
  margin: 0;
  line-height: 1;
  font-style: italic;
}

.pmain-title .pred {
  color: #e10600;
}

.pstars {
  color: #f59e0b;
  font-size: 4pt;
  letter-spacing: 2px;
  margin-top: 1px;
}

.pphoto-frame-wrap {
  margin-top: 2.5mm;
}

.pphoto-frame {
  width: 18mm;
  height: 22mm;
  border-radius: 2mm;
  border: 1.5px solid #0f172a;
  overflow: hidden;
  background: #e2e8f0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.pphoto-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.pphoto-placeholder {
  color: #94a3b8;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.pphoto-placeholder svg { width: 8mm; height: 8mm; }

.pdata-table {
  width: 100%;
  padding: 2.5mm 4mm 0;
  display: flex;
  flex-direction: column;
  gap: 1.2mm;
}

.pdata-row {
  display: flex;
  align-items: center;
  gap: 1mm;
  font-size: 4.5pt;
  color: #e2e8f0;
  background: rgba(255,255,255,0.06);
  border-radius: 1mm;
  padding: 0.8mm 1.5mm;
}

.pdata-label {
  font-weight: 700;
  min-width: 13mm;
}

.pdata-sep {
  font-weight: 700;
  color: #94a3b8;
}

.pdata-value {
  flex: 1;
  font-size: 4.5pt;
  color: #f1f5f9;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.pdata-value.pbold { font-weight: 800; }
.pdata-value.pred { color: #ef4444; }

.pbadge-navy {
  background: #0f172a;
  color: #fff;
  font-size: 3.8pt;
  font-weight: 900;
  padding: 0.2mm 1.5mm;
  border-radius: 0.5mm;
}

.pbarcode-wrap {
  text-align: center;
  padding: 2mm 4mm 0;
  width: 100%;
}

.pbarcode-svg {
  width: 100%;
  height: 5mm;
}

.pbarcode-label {
  display: block;
  font-size: 4pt;
  font-weight: 700;
  letter-spacing: 0.5px;
  color: #e2e8f0;
  margin-top: 0.5mm;
}

.pfront-footer {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 5.6mm;
  background: #060d20;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
  overflow: hidden;
}

.pmotto {
  position: relative;
  z-index: 11;
  font-size: 4pt;
  font-weight: 900;
  color: #ffffff;
  letter-spacing: 0.5px;
  text-transform: uppercase;
}
.pmotto-gold { color: #f59e0b; }

.print-card-back {
  background: #091022;
}

.pback-bg {
  position: absolute;
  inset: 0;
  background: linear-gradient(160deg, #091022 0%, #050811 100%);
  z-index: 0;
}

.opacity-40 { opacity: 0.4; }
.opacity-20 { opacity: 0.2; }
.opacity-15 { opacity: 0.15; }

.pback-content {
  position: relative;
  z-index: 10;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100%;
  padding: 4mm 3mm;
}

.pback-header {
  text-align: center;
  margin-bottom: 2mm;
}

.pback-logo {
  width: 8mm;
  height: 8mm;
  object-fit: contain;
  margin-bottom: 1mm;
}

.pback-title {
  font-family: 'Outfit', sans-serif;
  font-size: 7.5pt;
  font-weight: 800;
  color: #ffffff;
  margin: 0;
  letter-spacing: 0.5px;
}

.pback-subtitle {
  font-size: 3.5pt;
  font-weight: 700;
  color: #ef4444;
  letter-spacing: 1px;
  margin: 1px 0 0;
}

.pback-rules {
  margin: 1mm 0;
  padding-left: 3.5mm;
  font-size: 4.2pt;
  color: #cbd5e1;
  display: flex;
  flex-direction: column;
  gap: 1mm;
  text-align: left;
  width: 100%;
}

.pback-rules li {
  line-height: 1.3;
}

.pback-qr-wrap {
  margin-top: 3.5mm;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.8mm;
  border: 1px solid rgba(255,255,255,0.1);
  background: rgba(255,255,255,0.03);
  border-radius: 1.5mm;
  padding: 1.5mm;
}

.pback-qr-svg {
  width: 12mm;
  height: 12mm;
}

.pback-qr-label {
  font-size: 3.5pt;
  color: #94a3b8;
  font-family: monospace;
}

.pback-footer {
  position: absolute;
  bottom: 3mm;
  font-size: 3.8pt;
  color: #64748b;
  letter-spacing: 0.5px;
}

/* ═══════════════════════════════════════════════════════════
   PRINT / CHROME HEADLESS PDF
   ═══════════════════════════════════════════════════════════ */
@media print {
  .no-print { display: none !important; }

  body * { visibility: hidden; }

  /* Hide screen A4 papers */
  #printable-card-area,
  #printable-card-area * {
    display: none !important;
  }

  /* Show only printable ID Cards container */
  .print-only-id-cards,
  .print-only-id-cards * {
    visibility: visible;
  }

  .print-only-id-cards {
    display: block !important;
    position: absolute;
    top: 0;
    left: 0;
    width: 54mm;
  }

  .print-card {
    width: 54mm !important;
    height: 85.6mm !important;
    break-after: page !important;
    page-break-after: always !important;
    display: block !important;
    -webkit-print-color-adjust: exact !important;
    print-color-adjust: exact !important;
  }

  /* Set paper size to exactly 8.56 x 5.4 cm (CR80 Portrait) */
  @page {
    size: 54mm 85.6mm;
    margin: 0;
  }

  .pfront-bg,
  .pback-bg,
  .pshape,
  .pfront-footer,
  .pbadge-navy,
  .pbarcode-svg,
  .pbarcode-svg *,
  .pback-qr-svg,
  .pback-qr-svg * {
    -webkit-print-color-adjust: exact !important;
    print-color-adjust: exact !important;
  }
}

/* ═══════════════════════════════════════════════════════════
   RESPONSIVE — scale paper on narrow screens
   ═══════════════════════════════════════════════════════════ */
@media (max-width: 860px) {
  .a4-paper {
    width: min(794px, 98vw);
    font-size: clamp(10px, 1.8vw, 13px);
  }

  .hdr-org-name { font-size: clamp(1.1rem, 4vw, 1.9rem); }
  .hdr-logo     { width: clamp(40px, 8vw, 64px); height: clamp(40px, 8vw, 64px); }
  .hdr-ticket-label { font-size: clamp(0.9rem, 3vw, 1.4rem); }

  /* Stack layout on mobile to prevent letter-by-letter compression */
  .detail-grid {
    flex-direction: column;
    align-items: center;
    gap: 1.5rem;
  }

  .detail-table {
    width: 100%;
  }

  .detail-label {
    min-width: 110px; /* fixed smaller min-width on mobile */
  }

  .detail-right {
    width: 100%;
    flex-direction: row; /* display photo and QR side-by-side */
    justify-content: center;
    gap: 1.5rem;
    padding-top: 1rem;
    border-top: 1px dashed #e2e8f0;
  }

  .photo-box {
    width: 110px;
    height: 138px;
  }

  .qr-box {
    width: 110px;
    padding: 6px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .qr-svg {
    width: 85px;
    height: 85px;
  }
}
</style>
