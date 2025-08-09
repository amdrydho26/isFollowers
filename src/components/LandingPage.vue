<template>
  <div class="landing-page">
    <div class="hero-section">
      <div class="hero-content">
        <h1 class="hero-title">
          <span class="discover-text">Discover <span class="brand-bold">isFollowers</span></span>
        </h1>
        <p class="hero-description">
            Instagram Analytics yang Bikin Kamu Paham Banget Siapa Sahabat Asli vs Ghost Follower!
        </p>
      </div>
    </div>

    <div class="upload-section">
      <div class="upload-container">
        <h2 class="upload-title">Upload Data</h2>
        <p class="upload-description">
          Upload file JSON yang berisi data followers dan following Anda
        </p>
        
        <div class="file-upload-area">
          <div class="file-input-group">
            <label for="followers-file" class="file-label">
              <div class="file-input-content">
                <svg class="upload-icon" width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
                  <path d="M19.35 10.04C18.67 6.59 15.64 4 12 4 9.11 4 6.6 5.64 5.35 8.04 2.34 8.36 0 10.91 0 14c0 3.31 2.69 6 6 6h13c2.76 0 5-2.24 5-5 0-2.64-2.05-4.78-4.65-4.96zM14 13v4h-4v-4H7l5-5 5 5h-3z"/>
                </svg>
                <span>Upload followers_1.json</span>
              </div>
              <input 
                id="followers-file" 
                type="file" 
                accept=".json" 
                @change="handleFollowersUpload" 
                class="file-input"
              />
            </label>
            <div v-if="followersFile" class="file-info">
              <span class="file-name">{{ followersFile.name }}</span>
              <button @click="removeFollowersFile" class="remove-btn">×</button>
            </div>
          </div>

          <div class="file-input-group">
            <label for="following-file" class="file-label">
              <div class="file-input-content">
                <svg class="upload-icon" width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
                  <path d="M19.35 10.04C18.67 6.59 15.64 4 12 4 9.11 4 6.6 5.64 5.35 8.04 2.34 8.36 0 10.91 0 14c0 3.31 2.69 6 6 6h13c2.76 0 5-2.24 5-5 0-2.64-2.05-4.78-4.65-4.96zM14 13v4h-4v-4H7l5-5 5 5h-3z"/>
                </svg>
                <span>Upload following.json</span>
              </div>
              <input 
                id="following-file" 
                type="file" 
                accept=".json" 
                @change="handleFollowingUpload" 
                class="file-input"
              />
            </label>
            <div v-if="followingFile" class="file-info">
              <span class="file-name">{{ followingFile.name }}</span>
              <button @click="removeFollowingFile" class="remove-btn">×</button>
            </div>
          </div>
        </div>

        <div class="upload-actions">
          <button 
            @click="analyzeData" 
            :disabled="!canAnalyze" 
            class="analyze-btn"
            :class="{ 'disabled': !canAnalyze }"
          >
            Analisis Data
          </button>
        </div>

        <div class="upload-help" id="upload-help">
          <h3>Bagaimana cara mendapatkan file JSON?</h3>
          <ol class="help-steps">
            <li>Buka aplikasi <b>Instagram</b> dan pergi ke <b>Pengaturan</b>.</li>
            <li>Pilih <b>Pusat Akun</b>.</li>
            <li>Pada bagian <b>Pengaturan Akun</b>, pilih <b>Informasi dan izin Anda</b>.</li>
            <li>Kemudian pilih <b>Ekspor informasi Anda</b>. Jika anda diarahkan ke website, lakukan login jika belum.</li>
            <li>Selanjutnya pilih <b>Buat Ekspor</b> dan pilih <b>profil Anda</b> saat ini.</li>
            <li>Pilih <b>Ekspor ke perangkat</b>.</li>
            <li>Pilih <b>Sesuaikan Informasi</b> dan hapus semua centang kecuali <b>Pengikut dan mengikuti</b>. Jika sudah, pilih <b>Simpan</b>.</li>
            <li>Pilih <b>Rentang Tanggal</b> dan ubah menjadi <b>Sepanjang Waktu</b>. Jika sudah, pilih <b>Simpan</b>.</li>
            <li>Pilih <b>Format</b> dan ganti menjadi <b>JSON</b>. Jika sudah, pilih <b>Simpan</b>.</li>
            <li>Selanjutnya pilih <b>Mulai Ekspor</b>.</li>
            <li>Tunggu proses pembuatan data selesai, biasanya membutuhkan waktu sekitar 5 Menit (Waktu dapat bervariasi). Jika sudah selesai Anda akan menerima email dari Instagram.</li>
            <li>Kemudian jika sudah selesai, silahkan Download data Anda dengan menekan tombol <b>Download</b>. Kata sandi diperlukan untuk men-download data.</li>
            <li>Setelah ter-download, <b>ekstrak</b> file zip tersebut dan Anda sudah mendapatkan <b>file JSON-nya</b>.</li>
            <li>Terakhir Anda hanya perlu memasukkan file <b>followers_1.json</b> dan <b>following.json</b> sesuai kolomnya (tidak boleh terbalik).</li>
            <li>Lalu pilih <b>Analisis Data</b>.</li>
          </ol>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const emit = defineEmits(['data-ready'])

const followersFile = ref(null)
const followingFile = ref(null)

const canAnalyze = computed(() => {
  return followersFile.value && followingFile.value
})

const handleFollowersUpload = (event) => {
  const file = event.target.files[0]
  if (file && file.type === 'application/json') {
    followersFile.value = file
  } else {
    alert('Silakan upload file JSON yang valid')
  }
}

const handleFollowingUpload = (event) => {
  const file = event.target.files[0]
  if (file && file.type === 'application/json') {
    followingFile.value = file
  } else {
    alert('Silakan upload file JSON yang valid')
  }
}

const removeFollowersFile = () => {
  followersFile.value = null
  document.getElementById('followers-file').value = ''
}

const removeFollowingFile = () => {
  followingFile.value = null
  document.getElementById('following-file').value = ''
}

const analyzeData = async () => {
  if (!canAnalyze.value) return

  try {
    const followersData = await readFileAsJSON(followersFile.value)
    const followingData = await readFileAsJSON(followingFile.value)
    
    // Parse the data to match expected structure
    const parsedFollowers = parseUserData(followersData, false)
    const parsedFollowing = parseUserData(followingData, true)
    
    emit('data-ready', { 
      followersData: parsedFollowers, 
      followingData: parsedFollowing 
    })
  } catch (error) {
    alert('Error membaca file: ' + error.message)
  }
}

// Parse data function
const parseUserData = (data, isFollowing = false) => {
  if (isFollowing) {
    // Following data has different structure
    return data.relationships_following?.map(item => ({
      username: item.string_list_data[0]?.value || '',
      href: item.string_list_data[0]?.href || '',
      timestamp: item.string_list_data[0]?.timestamp || 0,
      date: new Date((item.string_list_data[0]?.timestamp || 0) * 1000)
    })) || []
  } else {
    // Followers data structure
    return data.map(item => ({
      username: item.string_list_data[0]?.value || '',
      href: item.string_list_data[0]?.href || '',
      timestamp: item.string_list_data[0]?.timestamp || 0,
      date: new Date((item.string_list_data[0]?.timestamp || 0) * 1000)
    }))
  }
}

const readFileAsJSON = (file) => {
  return new Promise((resolve, reject) => {
    const reader = new FileReader()
    reader.onload = (e) => {
      try {
        const json = JSON.parse(e.target.result)
        resolve(json)
      } catch (error) {
        reject(new Error('File JSON tidak valid'))
      }
    }
    reader.onerror = () => reject(new Error('Error membaca file'))
    reader.readAsText(file)
  })
}
</script>

<style scoped>
.landing-page {
  min-height: 100vh;
  background: linear-gradient(135deg, #0D0D0D 0%, #1a1a1a 100%);
}

.hero-section {
  min-height: 5vh;
  padding: 120px 20px 80px;
  text-align: center;
  background: linear-gradient(180deg, 
    rgba(52, 87, 213, 0.1) 0%, 
    transparent 100%);
  position: relative;
  margin-top: -150px;
  padding-top: 140px;
}

.hero-content {
  max-width: 800px;
  margin: 0 auto;
  position: relative;
  margin-top: 125px;
}

.hero-title {
  font-size: 3.5rem;
  font-weight: 700;
  color: var(--text-primary);
  margin-bottom: 20px;
  line-height: 1.1;
  letter-spacing: -0.01em;
}

.brand {
  color: var(--accent-blue);
  font-weight: 400;
}

.brand-bold {
  font-weight: 700;
  background: linear-gradient(135deg, var(--accent-blue), #6b8eff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.discover-text {
  font-weight: 400;
  color: var(--text-secondary);
  opacity: 0.7;
}

.hero-description {
  font-size: 1.2rem;
  color: var(--text-secondary);
  line-height: 1.6;
  max-width: 500px;
  margin: 0 auto;
  font-weight: 400;
  opacity: 0.8;
}

.upload-section {
  padding: 60px 20px;
  max-width: 800px;
  margin: 0 auto;
}

.upload-container {
  background: var(--bg-secondary);
  border-radius: 20px;
  padding: 48px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
}

.upload-title {
  font-size: 2rem;
  font-weight: 600;
  color: var(--text-primary);
  margin-bottom: 16px;
  text-align: center;
}

.upload-description {
  font-size: 1.1rem;
  color: var(--text-secondary);
  text-align: center;
  margin-bottom: 40px;
}

.file-upload-area {
  margin-bottom: 32px;
}

.file-input-group {
  margin-bottom: 24px;
}

.file-label {
  display: block;
  cursor: pointer;
  transition: all 0.3s ease;
}

.file-label:hover {
  transform: translateY(-2px);
}

.file-input-content {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 24px;
  border: 2px dashed var(--border-color);
  border-radius: 12px;
  background: rgba(52, 87, 213, 0.05);
  transition: all 0.3s ease;
}

.file-label:hover .file-input-content {
  border-color: var(--accent-blue);
  background: rgba(52, 87, 213, 0.1);
}

.upload-icon {
  color: var(--accent-blue);
  flex-shrink: 0;
}

.file-input {
  display: none;
}

.file-info {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 20px;
  background: rgba(52, 87, 213, 0.1);
  border-radius: 8px;
  margin-top: 12px;
}

.file-name {
  color: var(--text-primary);
  font-weight: 500;
}

.remove-btn {
  background: none;
  border: none;
  color: #ff6b6b;
  font-size: 20px;
  cursor: pointer;
  padding: 4px 8px;
  border-radius: 4px;
  transition: all 0.2s ease;
}

.remove-btn:hover {
  background: rgba(255, 107, 107, 0.1);
}

.upload-actions {
  text-align: center;
  margin-bottom: 40px;
}

.analyze-btn {
  background: var(--accent-blue);
  color: white;
  border: none;
  padding: 16px 32px;
  font-size: 1.1rem;
  font-weight: 600;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-family: 'Inter', sans-serif;
}

.analyze-btn:hover:not(.disabled) {
  background: #2a4bc4;
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(52, 87, 213, 0.3);
}

.analyze-btn.disabled {
  background: var(--border-color);
  cursor: not-allowed;
  opacity: 0.6;
}

.upload-help {
  border-top: 1px solid var(--border-color);
  padding-top: 32px;
}

.upload-help h3 {
  color: var(--text-primary);
  font-size: 1.2rem;
  margin-bottom: 20px;
  text-align: center;
}

.help-steps {
  color: var(--text-secondary);
  line-height: 1.8;
  padding-left: 20px;
}

.help-steps li {
  margin-bottom: 8px;
}

@media (max-width: 768px) {
  .hero-section {
    padding: 80px 20px 60px;
  }
  
  .hero-title {
    font-size: 3rem;
    margin-bottom: 24px;
  }
  
  .hero-description {
    font-size: 1.2rem;
    max-width: 100%;
  }
  
  .upload-container {
    padding: 32px 24px;
  }
  
  .upload-section {
    padding: 40px 20px;
  }
}

@media (max-width: 480px) {
  .hero-section {
    padding: 60px 20px 40px;
  }
  
  .hero-title {
    font-size: 2.5rem;
    margin-bottom: 20px;
  }
  
  .hero-description {
    font-size: 1.1rem;
  }
  
  .upload-container {
    padding: 24px 20px;
  }
  
  .file-input-content {
    padding: 20px;
  }
}
</style>
