<script setup>
import { ref, reactive, computed, watch, onMounted, onUnmounted } from 'vue'

const emit = defineEmits(['submit-success'])

// Active Stepper State
const currentStep = ref(1)
const steps = [
  { number: 1, label: 'Informasi Data Diri & Fisik', desc: 'Isi data pribadi dan fisik atlet' },
  { number: 2, label: 'Kontak & Riwayat Pendidikan', desc: 'Isi alamat dan kontak wali' },
  { number: 3, label: 'Perlengkapan Jersey & Surat', desc: 'Isi ukuran baju dan surat pernyataan' }
]

// Form State
const formData = reactive({
  // Section 1: Data Diri & Fisik
  email: '',
  fullName: '',
  dob: '',
  birthPlace: '',
  gender: 'Pria',
  height: '',
  weight: '',
  birthCertFile: null,
  birthCertFileName: '',

  // Section 2: Kontak & Pendidikan
  address: '',
  athletePhone: '',
  parentPhone: '',
  education: 'SMA',
  className: '',
  diplomaFile: null,
  diplomaFileName: '',
  achievements: '',

  // Section 3: Jersey, Foto & Dokumen
  jerseyName: '',
  shirtSize: 'M',
  pantsSize: 'M',
  athletePhotoFile: null,
  athletePhotoFileName: '',
  declarationFile: null,
  declarationFileName: '',
  agreeAllStatements: false,
  agreePbvRules: false
})

// Validation Errors
const errors = reactive({})

// File Handling Helpers
const handleFileChange = (e, fieldKey, nameKey) => {
  const file = e.target.files[0]
  if (file) {
    if (file.size > 10 * 1024 * 1024) {
      alert('Ukuran file maksimal 10 MB!')
      return
    }
    formData[fieldKey] = file
    formData[nameKey] = file.name
  }
}

const removeFile = (fieldKey, nameKey) => {
  formData[fieldKey] = null
  formData[nameKey] = ''
}

// Format Phone Numbers
const sanitizePhone = (val) => {
  return val.replace(/\D/g, '')
}

// Navigation & Validation
const validateCurrentStep = () => {
  Object.keys(errors).forEach(key => delete errors[key])
  let isValid = true

  if (currentStep.value === 1) {
    if (!formData.email || !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(formData.email)) {
      errors.email = 'Email tidak valid'
      isValid = false
    }
    if (!formData.fullName.trim()) {
      errors.fullName = 'Nama lengkap wajib diisi'
      isValid = false
    }
    if (!formData.dob) {
      errors.dob = 'Tanggal lahir wajib diisi'
      isValid = false
    }
    if (!formData.birthCertFileName) {
      errors.birthCertFile = 'Upload Akte Kelahiran wajib diunggah (PDF, Maks 10MB)'
      isValid = false
    }
    if (!formData.height) {
      errors.height = 'Tinggi badan wajib diisi'
      isValid = false
    }
    if (!formData.weight) {
      errors.weight = 'Berat badan wajib diisi'
      isValid = false
    }
  } else if (currentStep.value === 2) {
    if (!formData.address.trim()) {
      errors.address = 'Alamat lengkap wajib diisi'
      isValid = false
    }
    if (!formData.parentPhone.trim()) {
      errors.parentPhone = 'Nomor HP Orang Tua wajib diisi'
      isValid = false
    }
    if (!formData.education) {
      errors.education = 'Jenjang pendidikan wajib dipilih'
      isValid = false
    }
    if (!formData.diplomaFileName) {
      errors.diplomaFile = 'Upload Ijazah Terakhir wajib diunggah (PDF, Maks 10MB)'
      isValid = false
    }
    if (!formData.achievements.trim()) {
      errors.achievements = 'Riwayat prestasi wajib diisi'
      isValid = false
    }
  } else if (currentStep.value === 3) {
    if (!formData.jerseyName.trim()) {
      errors.jerseyName = 'Nama Jersey wajib diisi'
      isValid = false
    } else if (formData.jerseyName.trim().length > 12) {
      errors.jerseyName = 'Nama Jersey maksimal 12 karakter'
      isValid = false
    }
    if (!formData.athletePhotoFileName) {
      errors.athletePhotoFile = 'Foto Atlet wajib diunggah (Image, Maks 10MB)'
      isValid = false
    }
    if (!formData.declarationFileName) {
      errors.declarationFile = 'Surat Pernyataan wajib diunggah (PDF, Maks 10MB)'
      isValid = false
    }
    if (!formData.agreeAllStatements) {
      errors.agreeAllStatements = 'Anda wajib menyetujui pernyataan'
      isValid = false
    }
    if (!formData.agreePbvRules) {
      errors.agreePbvRules = 'Anda wajib menyetujui aturan PBV MALUKU'
      isValid = false
    }
  }

  if (!isValid) {
    setTimeout(() => {
      const errEl = document.querySelector('.error-message')
      if (errEl) errEl.scrollIntoView({ behavior: 'smooth', block: 'center' })
    }, 100)
  }

  return isValid
}

const nextStep = () => {
  if (validateCurrentStep()) {
    if (currentStep.value < 3) {
      currentStep.value++
      window.scrollTo({ top: 0, behavior: 'smooth' })
    } else {
      handleFormSubmit()
    }
  }
}

const prevStep = () => {
  if (currentStep.value > 1) {
    currentStep.value--
    window.scrollTo({ top: 0, behavior: 'smooth' })
  }
}

const handleFormSubmit = () => {
  emit('submit-success', {
    buyer: {
      fullName: formData.fullName,
      email: formData.email,
      phone: formData.athletePhone || formData.parentPhone,
      countryCode: '+62'
    },
    holders: [
      {
        fullName: formData.fullName,
        email: formData.email,
        phone: formData.athletePhone || formData.parentPhone
      }
    ],
    totalPayment: 150000
  })
}
</script>

<template>
  <div class="personal-info-wrapper">
    <!-- 1. DIV TITLE HEADER BERBED (MENGIKUTI SCROLL NORMAL) -->
    <div class="form-title-section">
      <div class="title-inner-container">
        <h2 class="form-main-title">Pendaftaran Atlet PBV MALUKU</h2>
        <p class="form-main-subtitle">Langkah {{ currentStep }} dari 3: {{ steps[currentStep - 1].label }}</p>
      </div>
    </div>

    <!-- 2. DIV TERPISAH MANDIRI DENGAN STICKY STAY DI LAYER PALING ATAS (PERMANEN STICKY STAY 68PX) -->
    <div class="standalone-stepper-sticky-bar">
      <div class="stepper-inner-container">
        <div class="stepper-header-row">
          <div class="stepper-container">
            <div 
              v-for="(step, idx) in steps" 
              :key="step.number" 
              class="stepper-item-group"
            >
              <!-- Step Circle Badge -->
              <div 
                class="step-circle" 
                :class="{ 'active': currentStep >= step.number }"
              >
                {{ step.number }}
              </div>

              <!-- Connecting Line between steps -->
              <div 
                v-if="idx < steps.length - 1" 
                class="step-line" 
                :class="{ 'active': currentStep > step.number }"
              ></div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- 3. MAIN FORM CARDS CONTAINER -->
    <div class="personal-info-container">
      <div class="info-layout-container">
        <div class="forms-column">
          
          <!-- STEP 1: Data Diri & Fisik -->
          <div v-show="currentStep === 1" class="step-section">
            <div class="category-group-card">
              <div class="category-card-header">
                <h3 class="category-title">Informasi Data Diri & Fisik</h3>
                <p class="category-subtitle">Silakan isi data pribadi dan informasi fisik atlet secara lengkap</p>
              </div>

              <div class="category-card-body">
                <!-- Field 1: Email -->
                <div class="form-field-item" :class="{ 'has-error': errors.email }">
                  <label class="field-label">Email <span class="required">*</span></label>
                  <input type="email" v-model="formData.email" placeholder="Contoh: atlet@gmail.com" class="styled-input" />
                  <span v-if="errors.email" class="error-message">{{ errors.email }}</span>
                </div>

                <!-- Field 2: Nama Lengkap -->
                <div class="form-field-item" :class="{ 'has-error': errors.fullName }">
                  <label class="field-label">Nama Lengkap <span class="required">*</span></label>
                  <input type="text" v-model="formData.fullName" placeholder="Masukkan nama lengkap sesuai identitas" class="styled-input" />
                  <span v-if="errors.fullName" class="error-message">{{ errors.fullName }}</span>
                </div>

                <!-- Grid Row: Tanggal Lahir & Tempat Lahir -->
                <div class="form-field-row grid-2">
                  <div class="form-field-item" :class="{ 'has-error': errors.dob }">
                    <label class="field-label">Tanggal Lahir <span class="required">*</span></label>
                    <input type="date" v-model="formData.dob" class="styled-input" />
                    <span v-if="errors.dob" class="error-message">{{ errors.dob }}</span>
                  </div>

                  <div class="form-field-item">
                    <label class="field-label">Tempat Lahir</label>
                    <input type="text" v-model="formData.birthPlace" placeholder="Kota kelahiran" class="styled-input" />
                  </div>
                </div>

                <!-- Grid Row: Jenis Kelamin -->
                <div class="form-field-item">
                  <label class="field-label">Jenis Kelamin <span class="required">*</span></label>
                  <div class="select-radio-group">
                    <label class="radio-card-option" :class="{ selected: formData.gender === 'Pria' }">
                      <input type="radio" v-model="formData.gender" value="Pria" />
                      <span class="radio-circle"></span>
                      <span>Pria</span>
                    </label>
                    <label class="radio-card-option" :class="{ selected: formData.gender === 'Wanita' }">
                      <input type="radio" v-model="formData.gender" value="Wanita" />
                      <span class="radio-circle"></span>
                      <span>Wanita</span>
                    </label>
                  </div>
                </div>

                <!-- Grid Row: Tinggi Badan & Berat Badan -->
                <div class="form-field-row grid-2">
                  <div class="form-field-item" :class="{ 'has-error': errors.height }">
                    <label class="field-label">Tinggi Badan <span class="required">*</span></label>
                    <input type="text" v-model="formData.height" placeholder="Contoh: 175 cm" class="styled-input" />
                    <span v-if="errors.height" class="error-message">{{ errors.height }}</span>
                  </div>

                  <div class="form-field-item" :class="{ 'has-error': errors.weight }">
                    <label class="field-label">Berat Badan <span class="required">*</span></label>
                    <input type="text" v-model="formData.weight" placeholder="Contoh: 65 kg" class="styled-input" />
                    <span v-if="errors.weight" class="error-message">{{ errors.weight }}</span>
                  </div>
                </div>

                <!-- Field Upload Akte Kelahiran -->
                <div class="form-field-item" :class="{ 'has-error': errors.birthCertFile }">
                  <label class="field-label">Upload Akte Kelahiran <span class="required">*</span></label>
                  <p class="field-help-text">Upload 1 file dokumen PDF (Maksimal 10 MB)</p>
                  
                  <div v-if="formData.birthCertFileName" class="file-uploaded-badge">
                    <svg fill="currentColor" viewBox="0 0 20 20" class="icon-file"><path fill-rule="evenodd" d="M4 4a2 2 0 012-2h4.586A2 2 0 0112 2.586L15.414 6A2 2 0 0116 7.414V16a2 2 0 01-2 2H6a2 2 0 01-2-2V4z" clip-rule="evenodd" /></svg>
                    <span class="file-name">{{ formData.birthCertFileName }}</span>
                    <button type="button" class="btn-remove" @click="removeFile('birthCertFile', 'birthCertFileName')">✕</button>
                  </div>

                  <label v-else class="styled-upload-box">
                    <svg fill="none" viewBox="0 0 24 24" stroke="currentColor" class="icon-upload"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12" /></svg>
                    <span>Pilih / Unggah File PDF</span>
                    <input type="file" accept="application/pdf" class="hidden-file-input" @change="e => handleFileChange(e, 'birthCertFile', 'birthCertFileName')" />
                  </label>

                  <span v-if="errors.birthCertFile" class="error-message">{{ errors.birthCertFile }}</span>
                </div>

              </div>
            </div>
          </div>

          <!-- STEP 2: Kontak & Pendidikan -->
          <div v-show="currentStep === 2" class="step-section">
            <div class="category-group-card">
              <div class="category-card-header">
                <h3 class="category-title">Kontak & Riwayat Pendidikan</h3>
                <p class="category-subtitle">Silakan isi alamat tempat tinggal, kontak wali, dan riwayat pendidikan atlet</p>
              </div>

              <div class="category-card-body">
                <!-- Alamat Lengkap -->
                <div class="form-field-item" :class="{ 'has-error': errors.address }">
                  <label class="field-label">Alamat Lengkap <span class="required">*</span></label>
                  <textarea v-model="formData.address" rows="3" placeholder="Nama Jalan, RT, RW, Kel, Kec, Kota/Kabupaten, Provinsi" class="styled-input textarea"></textarea>
                  <span v-if="errors.address" class="error-message">{{ errors.address }}</span>
                </div>

                <!-- Grid Row: No HP Atlet & No HP Orang Tua -->
                <div class="form-field-row grid-2">
                  <div class="form-field-item">
                    <label class="field-label">Nomor HP Atlet</label>
                    <input type="tel" v-model="formData.athletePhone" placeholder="Contoh: 081234567890" class="styled-input" @input="formData.athletePhone = sanitizePhone(formData.athletePhone)" />
                  </div>

                  <div class="form-field-item" :class="{ 'has-error': errors.parentPhone }">
                    <label class="field-label">Nomor HP Orang Tua / Wali <span class="required">*</span></label>
                    <input type="tel" v-model="formData.parentPhone" placeholder="Contoh: 081234567890" class="styled-input" @input="formData.parentPhone = sanitizePhone(formData.parentPhone)" />
                    <span v-if="errors.parentPhone" class="error-message">{{ errors.parentPhone }}</span>
                  </div>
                </div>

                <!-- Grid Row: Jenjang Pendidikan & Kelas -->
                <div class="form-field-row grid-2">
                  <div class="form-field-item">
                    <label class="field-label">Jenjang Pendidikan <span class="required">*</span></label>
                    <select v-model="formData.education" class="styled-input select">
                      <option value="TK">TK</option>
                      <option value="SD">SD</option>
                      <option value="SMP">SMP</option>
                      <option value="SMA">SMA / SMK</option>
                      <option value="Kuliah">Kuliah</option>
                    </select>
                  </div>

                  <div class="form-field-item">
                    <label class="field-label">Kelas / Semester</label>
                    <input type="text" v-model="formData.className" placeholder="Contoh: Kelas XI IPA 2 / Sem 3" class="styled-input" />
                  </div>
                </div>

                <!-- Upload Ijazah Terakhir -->
                <div class="form-field-item" :class="{ 'has-error': errors.diplomaFile }">
                  <label class="field-label">Upload Ijazah Terakhir <span class="required">*</span></label>
                  <p class="field-help-text">Upload 1 file dokumen PDF (Maksimal 10 MB)</p>
                  
                  <div v-if="formData.diplomaFileName" class="file-uploaded-badge">
                    <svg fill="currentColor" viewBox="0 0 20 20" class="icon-file"><path fill-rule="evenodd" d="M4 4a2 2 0 012-2h4.586A2 2 0 0112 2.586L15.414 6A2 2 0 0116 7.414V16a2 2 0 01-2 2H6a2 2 0 01-2-2V4z" clip-rule="evenodd" /></svg>
                    <span class="file-name">{{ formData.diplomaFileName }}</span>
                    <button type="button" class="btn-remove" @click="removeFile('diplomaFile', 'diplomaFileName')">✕</button>
                  </div>

                  <label v-else class="styled-upload-box">
                    <svg fill="none" viewBox="0 0 24 24" stroke="currentColor" class="icon-upload"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12" /></svg>
                    <span>Pilih / Unggah File Ijazah (PDF)</span>
                    <input type="file" accept="application/pdf" class="hidden-file-input" @change="e => handleFileChange(e, 'diplomaFile', 'diplomaFileName')" />
                  </label>

                  <span v-if="errors.diplomaFile" class="error-message">{{ errors.diplomaFile }}</span>
                </div>

                <!-- Riwayat Prestasi Bola Voli -->
                <div class="form-field-item" :class="{ 'has-error': errors.achievements }">
                  <label class="field-label">Riwayat Prestasi Bola Voli <span class="required">*</span></label>
                  <p class="field-help-text">Jika belum pernah mengikuti kejuaraan, tulis: "Tidak Ada"</p>
                  <input type="text" v-model="formData.achievements" placeholder="Jawaban Anda" class="styled-input" />
                  <span v-if="errors.achievements" class="error-message">{{ errors.achievements }}</span>
                </div>

              </div>
            </div>
          </div>

          <!-- STEP 3: Jersey & Dokumen Persyaratan -->
          <div v-show="currentStep === 3" class="step-section">
            <div class="category-group-card">
              <div class="category-card-header">
                <h3 class="category-title">Perlengkapan Jersey & Surat Pernyataan</h3>
                <p class="category-subtitle">Silakan isi ukuran jersey, upload foto atlet, dan unggah surat pernyataan</p>
              </div>

              <div class="category-card-body">
                <!-- Nama Jersey -->
                <div class="form-field-item" :class="{ 'has-error': errors.jerseyName }">
                  <label class="field-label">Nama Jersey <span class="required">*</span></label>
                  <p class="field-help-text">Nama pada Jersey (Maksimal 12 karakter, huruf kapital). Contoh: BUDI</p>
                  <input type="text" v-model="formData.jerseyName" placeholder="Maks 12 Huruf Kapital" class="styled-input uppercase" maxlength="12" @input="formData.jerseyName = formData.jerseyName.toUpperCase()" />
                  <span v-if="errors.jerseyName" class="error-message">{{ errors.jerseyName }}</span>
                </div>

                <!-- Grid Row: Ukuran Baju & Ukuran Celana -->
                <div class="form-field-row grid-2">
                  <div class="form-field-item">
                    <label class="field-label">Ukuran Baju <span class="required">*</span></label>
                    <select v-model="formData.shirtSize" class="styled-input select">
                      <option v-for="sz in ['XS', 'S', 'M', 'L', 'XL', 'XXL']" :key="sz" :value="sz">{{ sz }}</option>
                    </select>
                  </div>

                  <div class="form-field-item">
                    <label class="field-label">Ukuran Celana <span class="required">*</span></label>
                    <select v-model="formData.pantsSize" class="styled-input select">
                      <option v-for="sz in ['XS', 'S', 'M', 'L', 'XL', 'XXL']" :key="sz" :value="sz">{{ sz }}</option>
                    </select>
                  </div>
                </div>

                <!-- Upload Foto Atlet -->
                <div class="form-field-item" :class="{ 'has-error': errors.athletePhotoFile }">
                  <label class="field-label">Foto Atlet <span class="required">*</span></label>
                  <p class="field-help-text">Foto menghadap depan, latar belakang polos, tanpa topi atau kacamata (Maksimal 10 MB)</p>
                  
                  <div v-if="formData.athletePhotoFileName" class="file-uploaded-badge">
                    <svg fill="currentColor" viewBox="0 0 20 20" class="icon-file"><path fill-rule="evenodd" d="M4 3a2 2 0 00-2 2v10a2 2 0 002 2h12a2 2 0 002-2V5a2 2 0 00-2-2H4zm12 12H4l4-8 3 6 2-4 3 6z" clip-rule="evenodd" /></svg>
                    <span class="file-name">{{ formData.athletePhotoFileName }}</span>
                    <button type="button" class="btn-remove" @click="removeFile('athletePhotoFile', 'athletePhotoFileName')">✕</button>
                  </div>

                  <label v-else class="styled-upload-box">
                    <svg fill="none" viewBox="0 0 24 24" stroke="currentColor" class="icon-upload"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12" /></svg>
                    <span>Pilih / Unggah Foto Atlet</span>
                    <input type="file" accept="image/*" class="hidden-file-input" @change="e => handleFileChange(e, 'athletePhotoFile', 'athletePhotoFileName')" />
                  </label>

                  <span v-if="errors.athletePhotoFile" class="error-message">{{ errors.athletePhotoFile }}</span>
                </div>

                <!-- Upload Surat Pernyataan -->
                <div class="form-field-item" :class="{ 'has-error': errors.declarationFile }">
                  <label class="field-label">Surat Pernyataan <span class="required">*</span></label>
                  <p class="field-help-text">Silakan mengunduh Surat Pernyataan, mengisi & menandatanganinya, lalu unggah kembali format PDF</p>
                  
                  <a 
                    href="https://drive.google.com/drive/folders/1_zakeXvRv1k2r6OOaO0rsuD4hrro2tsU" 
                    target="_blank" 
                    rel="noopener noreferrer" 
                    class="btn-download-pdf"
                  >
                    📄 Unduh Surat Pernyataan (PDF)
                  </a>

                  <div v-if="formData.declarationFileName" class="file-uploaded-badge mt-2">
                    <svg fill="currentColor" viewBox="0 0 20 20" class="icon-file"><path fill-rule="evenodd" d="M4 4a2 2 0 012-2h4.586A2 2 0 0112 2.586L15.414 6A2 2 0 0116 7.414V16a2 2 0 01-2 2H6a2 2 0 01-2-2V4z" clip-rule="evenodd" /></svg>
                    <span class="file-name">{{ formData.declarationFileName }}</span>
                    <button type="button" class="btn-remove" @click="removeFile('declarationFile', 'declarationFileName')">✕</button>
                  </div>

                  <label v-else class="styled-upload-box mt-2">
                    <svg fill="none" viewBox="0 0 24 24" stroke="currentColor" class="icon-upload"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12" /></svg>
                    <span>Unggah Surat Pernyataan (PDF)</span>
                    <input type="file" accept="application/pdf" class="hidden-file-input" @change="e => handleFileChange(e, 'declarationFile', 'declarationFileName')" />
                  </label>

                  <span v-if="errors.declarationFile" class="error-message">{{ errors.declarationFile }}</span>
                </div>

                <!-- Checkbox Persetujuan -->
                <div class="form-field-item mt-3" :class="{ 'has-error': errors.agreeAllStatements || errors.agreePbvRules }">
                  <label class="field-label mb-2">Persetujuan <span class="required">*</span></label>

                  <div class="checkbox-stack">
                    <label class="custom-checkbox-row">
                      <input type="checkbox" v-model="formData.agreeAllStatements" />
                      <span class="checkbox-square"></span>
                      <span class="checkbox-text-label">Saya telah membaca dan menyetujui seluruh pernyataan di atas.</span>
                    </label>

                    <label class="custom-checkbox-row">
                      <input type="checkbox" v-model="formData.agreePbvRules" />
                      <span class="checkbox-square"></span>
                      <span class="checkbox-text-label">Saya bersedia menaati dan mengikuti aturan PBV. MALUKU.</span>
                    </label>
                  </div>

                  <span v-if="errors.agreeAllStatements || errors.agreePbvRules" class="error-message">
                    {{ errors.agreeAllStatements || errors.agreePbvRules }}
                  </span>
                </div>

              </div>
            </div>
          </div>

        </div>
      </div>

    </div>

    <!-- STICKY BOTTOM BAR FOOTER NAVIGASI -->
    <div class="sticky-bottom-bar">
      <div class="bottom-bar-container">
        <!-- Kiri: Brand Name PBV Maluku -->
        <div class="bottom-bar-left">
          <span class="brand-bottom-text">PBV Maluku</span>
        </div>

        <!-- Kanan: Button Navigasi -->
        <div class="bottom-bar-right">
          <button v-if="currentStep > 1" type="button" class="btn-bottom-nav prev" @click="prevStep">
            Sebelumnya
          </button>
          
          <button type="button" class="btn-bottom-nav next" @click="nextStep">
            {{ currentStep === 3 ? 'Kirim Pendaftaran' : 'Selanjutnya' }}
          </button>
        </div>
      </div>
    </div>

  </div>
</template>

<style scoped>
.personal-info-wrapper {
  padding: 1.5rem 2rem 6rem 2rem;
  max-width: 1540px;
  margin: 0 auto;
}

/* TITLE SECTION BERDIRI SENDIRI */
.form-title-section {
  max-width: 1080px;
  margin: 0 auto 1.25rem auto;
}

.form-main-title {
  font-size: 1.5rem;
  font-weight: 800;
  color: #0f172a;
  letter-spacing: -0.01em;
}

.form-main-subtitle {
  font-size: 0.88rem;
  color: #64748b;
  margin-top: 0.15rem;
}

/* 2. STANDALONE STEPPER STICKY BAR (DIBUATKAN DIV BERBEDA & GUARANTEED STICKY STAY DI TOP 68PX) */
.standalone-stepper-sticky-bar {
  position: -webkit-sticky;
  position: sticky;
  top: 68px; /* Tepat di bawah Navbar fixed 68px */
  z-index: 800;
  background-color: rgba(248, 250, 252, 0.98);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  padding: 0.85rem 0;
  margin-bottom: 1.25rem;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.02);
}

.stepper-inner-container {
  max-width: 1080px;
  margin: 0 auto;
}

.stepper-header-row {
  display: flex;
  justify-content: flex-start;
}

.stepper-container {
  display: flex;
  align-items: center;
  gap: 0;
  max-width: 520px;
  width: 100%;
}

.stepper-item-group {
  display: flex;
  align-items: center;
  flex-grow: 1;
}

.stepper-item-group:last-child {
  flex-grow: 0;
}

.step-circle {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background-color: #ffffff;
  border: 2px solid #cbd5e1;
  color: #64748b;
  font-weight: 700;
  font-size: 0.95rem;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  transition: all 0.3s ease;
}

.step-circle.active {
  background-color: #2563eb;
  border-color: #2563eb;
  color: #ffffff;
  box-shadow: 0 2px 8px rgba(37, 99, 235, 0.25);
}

.step-line {
  height: 3px;
  background-color: #cbd5e1;
  flex-grow: 1;
  margin: 0 0.5rem;
  transition: all 0.3s ease;
}

.step-line.active {
  background-color: #2563eb;
}

/* LAYOUT CONTAINER & CARDS FORM */
.personal-info-container {
  max-width: 1080px;
  margin: 0 auto;
}

.info-layout-container {
  width: 100%;
}

.category-group-card {
  background-color: #ffffff;
  border: 1px solid #e2e8f0;
  border-radius: 16px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.04);
}

.category-card-header {
  padding: 1.5rem 2rem;
  border-bottom: 1px solid #f1f5f9;
  background-color: #ffffff;
  border-top-left-radius: 16px;
  border-top-right-radius: 16px;
}

.category-title {
  font-size: 1.3rem;
  font-weight: 800;
  color: #0f172a;
  margin-bottom: 0.25rem;
}

.category-subtitle {
  font-size: 0.88rem;
  color: #64748b;
}

.category-card-body {
  padding: 1.75rem 2rem;
  display: flex;
  flex-direction: column;
  gap: 1.35rem;
}

/* Form Controls */
.form-field-item {
  display: flex;
  flex-direction: column;
  gap: 0.45rem;
}

.form-field-row.grid-2 {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
}

.field-label {
  font-size: 0.95rem;
  font-weight: 700;
  color: #1e293b;
}

.required {
  color: #dc2626;
}

.field-help-text {
  font-size: 0.82rem;
  color: #64748b;
  margin-bottom: 0.25rem;
}

.styled-input {
  width: 100%;
  background-color: #ffffff;
  border: 1px solid #cbd5e1;
  border-radius: 10px;
  padding: 0.75rem 1rem;
  font-size: 0.95rem;
  color: #0f172a;
  outline: none;
  transition: all 0.2s ease;
}

.styled-input:focus {
  border-color: #2563eb;
  box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.12);
}

.styled-input.textarea {
  resize: vertical;
  min-height: 80px;
}

.styled-input.select {
  appearance: none;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' fill='none' viewBox='0 0 24 24' stroke='%252364748b'%3E%3Cpath stroke-linecap='round' stroke-linejoin='round' stroke-width='2' d='M19 9l-7 7-7-7'%3E%3C/path%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 1rem center;
  background-size: 1.2rem;
  padding-right: 2.5rem;
  cursor: pointer;
}

.styled-input.uppercase {
  text-transform: uppercase;
}

/* Radio Cards */
.select-radio-group {
  display: flex;
  gap: 1rem;
}

.radio-card-option {
  flex: 1;
  display: flex;
  align-items: center;
  gap: 0.6rem;
  padding: 0.75rem 1rem;
  border: 1px solid #cbd5e1;
  border-radius: 10px;
  cursor: pointer;
  user-select: none;
  transition: all 0.2s ease;
}

.radio-card-option input {
  display: none;
}

.radio-circle {
  width: 18px;
  height: 18px;
  border-radius: 50%;
  border: 2px solid #94a3b8;
  display: inline-block;
  position: relative;
}

.radio-card-option.selected {
  border-color: #2563eb;
  background-color: #eff6ff;
}

.radio-card-option.selected .radio-circle {
  border-color: #2563eb;
}

.radio-card-option.selected .radio-circle::after {
  content: '';
  width: 9px;
  height: 9px;
  border-radius: 50%;
  background-color: #2563eb;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

/* Upload Box */
.hidden-file-input {
  display: none;
}

.styled-upload-box {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.6rem;
  border: 2px dashed #cbd5e1;
  border-radius: 10px;
  padding: 1rem;
  cursor: pointer;
  color: #2563eb;
  font-weight: 600;
  font-size: 0.9rem;
  transition: all 0.2s ease;
  background-color: #f8fafc;
}

.styled-upload-box:hover {
  border-color: #2563eb;
  background-color: #eff6ff;
}

.icon-upload {
  width: 22px;
  height: 22px;
}

.file-uploaded-badge {
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;
  background-color: #f1f5f9;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  padding: 0.6rem 1rem;
}

.icon-file {
  width: 20px;
  height: 20px;
  color: #2563eb;
}

.file-name {
  font-size: 0.88rem;
  font-weight: 600;
  color: #1e293b;
}

.btn-remove {
  background: none;
  border: none;
  color: #64748b;
  font-weight: 700;
  cursor: pointer;
  margin-left: 0.5rem;
}

.btn-remove:hover {
  color: #dc2626;
}

.btn-download-pdf {
  display: inline-block;
  color: #7c3aed;
  font-weight: 700;
  font-size: 0.9rem;
  text-decoration: underline;
}

/* Checkboxes */
.checkbox-stack {
  display: flex;
  flex-direction: column;
  gap: 0.85rem;
}

.custom-checkbox-row {
  display: flex;
  align-items: flex-start;
  gap: 0.65rem;
  cursor: pointer;
}

.custom-checkbox-row input {
  display: none;
}

.checkbox-square {
  width: 20px;
  height: 20px;
  border: 2px solid #2563eb;
  border-radius: 4px;
  background-color: #ffffff;
  flex-shrink: 0;
  margin-top: 0.1rem;
  position: relative;
}

.custom-checkbox-row input:checked + .checkbox-square {
  background-color: #2563eb;
}

.custom-checkbox-row input:checked + .checkbox-square::after {
  content: '';
  position: absolute;
  left: 6px;
  top: 2px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
}

.checkbox-text-label {
  font-size: 0.9rem;
  color: #334155;
  line-height: 1.45;
}

.error-message {
  font-size: 0.8rem;
  color: #dc2626;
  margin-top: 0.2rem;
}

/* STICKY BOTTOM BAR FOOTER NAVIGASI */
.sticky-bottom-bar {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  z-index: 1000;
  background-color: #ffffff;
  border-top: 1px solid #e2e8f0;
  box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.05);
  padding: 0.85rem 2rem;
  transition: transform 0.35s cubic-bezier(0.16, 1, 0.3, 1), opacity 0.3s ease;
}

body.sidebar-open .sticky-bottom-bar {
  transform: translateY(100%);
  opacity: 0;
  pointer-events: none;
}

.bottom-bar-container {
  max-width: 1540px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.bottom-bar-left {
  display: flex;
  align-items: center;
}

.brand-bottom-text {
  font-size: 1.1rem;
  font-weight: 800;
  color: #0f172a;
  letter-spacing: -0.01em;
}

.bottom-bar-right {
  display: flex;
  align-items: center;
  gap: 0.85rem;
}

.btn-bottom-nav {
  padding: 0.75rem 1.75rem;
  border-radius: 10px;
  font-size: 0.95rem;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.2s ease;
}

.btn-bottom-nav.prev {
  background-color: #ffffff;
  color: #334155;
  border: 1px solid #cbd5e1;
}

.btn-bottom-nav.prev:hover {
  background-color: #f1f5f9;
}

.btn-bottom-nav.next {
  background-color: #2563eb;
  color: #ffffff;
  border: none;
  box-shadow: none;
}

.btn-bottom-nav.next:hover {
  background-color: #1d4ed8;
  transform: translateY(-1px);
}

.mt-2 { margin-top: 0.5rem; }
.mt-3 { margin-top: 1rem; }
.mb-2 { margin-bottom: 0.5rem; }

/* Responsiveness */
@media (max-width: 768px) {
  .personal-info-wrapper {
    padding: 0.75rem 0.75rem 5rem 0.75rem;
  }
  .form-main-title {
    font-size: 1.05rem;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .form-main-subtitle {
    font-size: 0.75rem;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .standalone-stepper-sticky-bar {
    top: 58px;
    padding: 0.5rem 0;
    margin-bottom: 0.65rem;
  }
  .step-circle {
    width: 28px;
    height: 28px;
    font-size: 0.8rem;
  }
  .category-card-header {
    padding: 0.85rem 1rem;
  }
  .category-title {
    font-size: 0.98rem;
    white-space: normal;
    word-break: break-word;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
  .category-subtitle {
    font-size: 0.75rem;
    white-space: normal;
    word-break: break-word;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
  .category-card-body {
    padding: 1rem;
    gap: 1rem;
  }
  .field-label {
    font-size: 0.82rem;
  }
  .styled-input {
    padding: 0.55rem 0.75rem;
    font-size: 0.85rem;
    border-radius: 8px;
  }
  .form-field-row.grid-2 {
    grid-template-columns: 1fr;
    gap: 1rem;
  }
  .btn-download-pdf {
    font-size: 0.78rem;
  }
  .styled-upload-box {
    padding: 0.65rem;
    font-size: 0.78rem;
  }
  .icon-upload {
    width: 16px;
    height: 16px;
  }
  .checkbox-text-label {
    font-size: 0.78rem;
    line-height: 1.35;
  }
  .checkbox-square {
    width: 16px;
    height: 16px;
  }
  .custom-checkbox-row input:checked + .checkbox-square::after {
    left: 5px;
    top: 1px;
    width: 4px;
    height: 8px;
  }
  .sticky-bottom-bar {
    padding: 0.5rem 0.85rem;
  }
  .brand-bottom-text {
    font-size: 0.85rem;
    font-weight: 700;
  }
  .btn-bottom-nav {
    padding: 0.45rem 0.85rem;
    font-size: 0.75rem;
    border-radius: 6px;
    white-space: nowrap;
  }
}
</style>
