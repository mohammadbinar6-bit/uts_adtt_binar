# 🔍 Disonansi Moral di Institusi Hukum
### Pemodelan Topik & Analisis Sentimen — Diskursus Komentar TikTok

> **UTS Analisis Data Tidak Terstruktur** · Muhammad Binar Raffi Lazuardi (2306228005) · Statistika, Universitas Indonesia

---

## 📌 Overview

Proyek ini menganalisis **1.818 komentar TikTok** terkait kasus viral pelecehan seksual verbal yang melibatkan mahasiswa Fakultas Hukum UI (April 2026). Pipeline NLP end-to-end menggabungkan:

- **Sentiment Analysis** dengan model RoBERTa bahasa Indonesia
- **Topic Modelling** berbasis BERTopic (SBERT + UMAP + HDBSCAN)
- **Cross-analysis** Sentimen × Topik untuk pemetaan opini publik

**Key finding:** 68,5% komentar bersentimen negatif — mencerminkan kemarahan kolektif terhadap institusi.

---

## 📁 Struktur File

```
├── UTS_ADTT_Binar.ipynb          # Notebook utama (pipeline lengkap)
├── Tiktok_Crawled_Code__API_Based_.ipynb  # Crawling data TikTok via API
├── Template_Laporan_UTS.pdf     # Laporan akademik (format paper)
├── comments.csv                  # Dataset hasil crawling (1.818 komentar)
└── README.md
```

**Output yang dihasilkan notebook:**
```
├── distribusi_komentar.png       # Distribusi panjang & jumlah kata
├── wordcloud.png                 # Word cloud pasca-preprocessing
├── analisis_sentimen.png         # Pie chart, bar, confidence score
├── likes_per_sentimen.png        # Rata-rata likes per sentimen
├── distribusi_topik.png          # Bar chart distribusi topik
├── topic_barchart.html           # Visualisasi interaktif BERTopic
└── topic_distance.html           # Intertopic distance map
```

---

## 🗃️ Dataset

| Atribut | Detail |
|---|---|
| Sumber | TikTok `@sadampermanawiyana` — video viral FHUI |
| Jumlah komentar | 1.818 raw → 1.627 valid |
| Kolom | `username`, `comment`, `likes` |
| Total likes | 732.282 |
| Rata-rata panjang | ~52 karakter / ~6,1 kata |
| Max likes per komentar | 152.900 |

---

## ⚙️ Setup & Instalasi

### Requirements

```bash
pip install TikTokApi playwright transformers torch
pip install sentence-transformers bertopic umap-learn hdbscan
pip install pandas matplotlib seaborn wordcloud plotly scikit-learn
pip install Sastrawi
```

```bash
playwright install chromium
apt-get install -y libatk1.0-0 libatk-bridge2.0-0 libcups2 libdrm2 \
    libxkbcommon0 libxcomposite1 libxdamage1 libxfixes3 libxrandr2 \
    libgbm1 libasound2 libgstreamer-gl1.0-0
```

### Rekomendasi Environment

| Komponen | Rekomendasi |
|---|---|
| Platform | Google Colab (GPU T4) |
| RAM | ≥ 12 GB |
| Python | 3.10+ |
| GPU | CUDA-enabled (opsional, mempercepat inferensi ~10x) |

---

## 🚀 Cara Menjalankan

### 1. Crawling Data (opsional, jika ingin update dataset)

```bash
# Jalankan Tiktok_Crawled_Code__API_Based_.ipynb
# Ganti ms_token dan URL video sesuai kebutuhan
# Output: comments.csv
```

> ⚠️ `ms_token` bersifat sementara dan perlu diperbarui secara berkala dari browser TikTok.

### 2. Pipeline Analisis Utama

Jalankan `UTS_ADTT_Binar.ipynb` secara berurutan:

```
[1] Instalasi Library
[2] Load & Eksplorasi Data
[3] Preprocessing Teks       ← slang normalization + Sastrawi stemmer
[4] Analisis Sentimen        ← IndoBERT RoBERTa inference
[5] Topic Modelling          ← BERTopic fit_transform
[6] Analisis Gabungan        ← Sentimen × Topik cross-tab & heatmap
```

---

## 🤖 Model yang Digunakan

### Sentiment Analysis
**`w11wo/indonesian-roberta-base-sentiment-classifier`**
- Arsitektur: RoBERTa (bidirectional attention)
- Fine-tuned untuk Bahasa Indonesia
- Output: `Positif` / `Negatif` / `Netral`
- Inferensi: batch size 32, truncation 512 token

### Topic Modelling — BERTopic
| Komponen | Model/Konfigurasi |
|---|---|
| Embedding | `paraphrase-multilingual-MiniLM-L12-v2` (SBERT) |
| Dimensionality Reduction | UMAP (`n_neighbors=10`, `n_components=5`, `metric=cosine`) |
| Clustering | HDBSCAN (`min_cluster_size=10`, `min_samples=5`, EOM) |
| Representasi | c-TF-IDF dengan unigram + bigram |
| Stopwords | Sastrawi Indonesian stopwords |

---

## 📊 Hasil Utama

### Distribusi Sentimen

| Sentimen | Jumlah | Persentase |
|---|---|---|
| **Negatif** | 1.114 | 68,5% |
| Netral | 409 | 25,1% |
| Positif | 104 | 6,4% |

### Temuan Penting
- **Dominasi negatif** → kemarahan kolektif terhadap pelaku dan institusi FHUI
- **Komentar netral** memiliki rata-rata likes **tertinggi** (717), mengindikasikan apresiasi publik terhadap diskusi reflektif
- **15 topik** berhasil diidentifikasi: dari reaksi emosional viral, ekspektasi institusi, ironi mahasiswa hukum, hingga narasi etika & adab
- Topik **etika/adab** memiliki proporsi sentimen netral-positif tertinggi
- Topik yang berkaitan langsung dengan kasus memiliki sentimen negatif paling kuat

---

## 🧹 Preprocessing Pipeline

```python
raw_text
  → lowercase
  → hapus URL, mention, hashtag
  → hapus emoji (ASCII encoding)
  → hapus karakter non-alfabet
  → normalisasi slang (kamus 30+ entri: "gak"→"tidak", "yg"→"yang", dst.)
  → hapus stopwords (Sastrawi)
  → [opsional untuk topic modelling] stemming
```

---

## 📝 Laporan

Laporan akademik tersedia di `Template_Laporan_UTS.docx` dalam format dua kolom (paper conference). Laporan mencakup:
- Abstrak bilingual
- Pendahuluan & latar belakang kasus
- Metodologi lengkap dengan formula matematis
- Hasil & pembahasan dengan tabel dan figur
- Kesimpulan dan implikasi kebijakan

---

## ⚠️ Catatan Etika & Privasi

- Data dikumpulkan dari komentar publik TikTok
- Username pengguna tidak diekspos dalam laporan final
- Penelitian bertujuan akademis: analisis opini publik, bukan identifikasi individu
- Kasus yang dianalisis merupakan kejadian nyata yang telah tersebar luas di media publik

---

## 👤 Author

**Muhammad Binar Raffi Lazuardi**  
NIM: 2306228005 · Kelas A  
Program Studi Statistika, Universitas Indonesia  
muhammad.binar@ui.ac.id

---

*Mata kuliah: Analisis Data Tidak Terstruktur (ADTT) · UTS 2025/2026*
