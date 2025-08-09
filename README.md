# isFollowers

Aplikasi web untuk menganalisis data Instagram followers dan following Anda. Aplikasi ini membantu Anda memahami pola follow back dan memberikan insight tentang hubungan sosial media Anda.

## Fitur

- **Landing Page yang Menarik**: Halaman selamat datang dengan desain modern
- **Upload File JSON**: Kemampuan untuk mengunggah file data Instagram
- **Analisis Mendalam**: Statistik lengkap tentang followers dan following
- **Identifikasi Mutual Followers**: Temukan siapa yang saling mengikuti
- **Responsive Design**: Bekerja dengan baik di desktop dan mobile
- **Font Inter**: Menggunakan font modern dan mudah dibaca

## Cara Menggunakan

### 1. Persiapan Data
Untuk menggunakan aplikasi ini, Anda memerlukan file JSON yang berisi data Instagram:

- **followers_1.json**: File yang berisi daftar followers Anda
- **following.json**: File yang berisi daftar akun yang Anda ikuti

### 2. Cara Mendapatkan File JSON
Ada beberapa cara untuk mendapatkan file data Instagram:

- **Browser Extension**: Gunakan extension seperti "Instagram Data Export"
- **Third-party Tools**: Gunakan tool seperti "InstaScrape" atau yang serupa
- **Manual Export**: Jika Anda memiliki akses ke API Instagram

### 3. Upload dan Analisis
1. Buka aplikasi isFollowers
2. Upload file `followers_1.json` dan `following.json`
3. Klik tombol "Analisis Data"
4. Lihat hasil analisis yang detail

## Struktur Data yang Diharapkan

### Followers Data
```json
[
  {
    "string_list_data": [
      {
        "value": "username",
        "href": "https://instagram.com/username",
        "timestamp": 1234567890
      }
    ]
  }
]
```

### Following Data
```json
{
  "relationships_following": [
    {
      "string_list_data": [
        {
          "value": "username",
          "href": "https://instagram.com/username",
          "timestamp": 1234567890
        }
      ]
    }
  ]
}
```

## Statistik yang Disediakan

- **Total Pengikut**: Jumlah total followers
- **Total Mengikuti**: Jumlah total akun yang diikuti
- **Saling Mengikuti**: Jumlah mutual followers
- **Kamu Tidak Follow Back**: Followers yang tidak Anda ikuti balik
- **Tidak Follow Back Kamu**: Akun yang Anda ikuti tapi tidak mengikuti Anda balik
- **Tingkat Follow Back**: Persentase mutual followers

## Teknologi yang Digunakan

- **Vue.js 3**: Framework JavaScript modern
- **Vite**: Build tool yang cepat
- **CSS Variables**: Untuk theming yang konsisten
- **Inter Font**: Font modern dan mudah dibaca
- **Responsive Design**: Menggunakan CSS Grid dan Flexbox

## Instalasi dan Pengembangan

### Prerequisites
- Node.js (versi 16 atau lebih baru)
- npm atau yarn

### Setup
```bash
# Clone repository
git clone [repository-url]
cd isFollowers

# Install dependencies
npm install

# Run development server
npm run dev

# Build for production
npm run build
```

### Struktur Project
```
isFollowers/
├── src/
│   ├── components/
│   │   ├── LandingPage.vue      # Halaman landing dan upload
│   │   └── DataAnalysis.vue     # Halaman analisis data
│   ├── App.vue                  # Komponen utama
│   ├── main.js                  # Entry point
│   └── style.css                # CSS global
├── public/                      # Assets statis
├── index.html                   # HTML template
└── package.json                 # Dependencies
```

## Kontribusi

Kontribusi sangat diterima! Jika Anda ingin berkontribusi:

1. Fork repository ini
2. Buat branch fitur baru (`git checkout -b feature/AmazingFeature`)
3. Commit perubahan Anda (`git commit -m 'Add some AmazingFeature'`)
4. Push ke branch (`git push origin feature/AmazingFeature`)
5. Buka Pull Request

## Lisensi

Project ini dilisensikan di bawah MIT License - lihat file [LICENSE](LICENSE) untuk detail lebih lanjut.

## Dukungan

Jika Anda mengalami masalah atau memiliki pertanyaan:

- Buat issue di GitHub repository
- Hubungi tim pengembang
- Periksa dokumentasi yang tersedia

## Privasi dan Keamanan

- **Data Lokal**: Semua data diproses di browser Anda
- **Tidak Ada Upload ke Server**: File JSON tidak dikirim ke server eksternal
- **Privasi Terjamin**: Data Anda tetap aman dan pribadi

---

Dibuat dengan ❤️ untuk komunitas Instagram
