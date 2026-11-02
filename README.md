# 🌾 Proyek Analisis Sentimen Ulasan Hay Day

Proyek ini berfokus pada analisis sentimen ulasan pengguna game **Hay Day** di Google Play Store. Proyek ini mengimplementasikan pipeline pemrosesan teks bahasa Indonesia terstruktur dan membandingkan performa berbagai metode *Machine Learning* klasik serta *Deep Learning* (Bidirectional LSTM).

---

## 📊 Hasil Eksperimen & Performa Model

Berikut adalah perbandingan akurasi model dari 3 skema pelatihan utama yang dievaluasi:

| Skema Pelatihan | Train Accuracy | Test Accuracy |
| :--- | :---: | :---: |
| **Skema 1: Bidirectional LSTM (80/20)** | **94.55%** | **85.55%** |
| **Skema 2: LinearSVC + TF-IDF (80/20)** | 90.93% | 85.28% |
| **Skema 3: LinearSVC + TF-IDF (70/30)** | 90.88% | 85.12% |

### ⚖️ Penanganan Imbalance Class (SMOTE + LinearSVC 80/20)
Setelah menerapkan **SMOTE** untuk menyeimbangkan kelas minoritas (Negatif), model **LinearSVC** menghasilkan distribusi performa yang lebih seimbang antara presisi dan recall:
* **Akurasi Keseluruhan**: **81%**
* **F1-Score (Negatif)**: **0.69** (Recall naik drastis menjadi **83%**)
* **F1-Score (Positif)**: **0.86**

---

## 🛠️ Alur Pipeline Pra-Pemrosesan Teks
Setiap ulasan mentah melewati tahap pembersihan intensif sebelum masuk ke tahap pemodelan:
1. **Cleaning**: Menghapus mention (@), hashtag (#), RT, URL, angka, dan tanda baca.
2. **Case Folding**: Mengubah seluruh teks menjadi huruf kecil (*lowercase*).
3. **Slangwords Correction**: Memetakan kata-kata gaul/singkatan ke kata baku (contoh: *bgt* -> *banget*, *yg* -> *yang*).
4. **Tokenization**: Pemotongan kalimat menjadi token kata individual.
5. **Stopwords Filtering**: Mengeliminasi kata-kata tidak bermakna dengan tetap mempertahankan kata kunci sentimen (seperti *tidak, kurang, bagus, jelek*).
6. **Stemming (Opsional)**: Reduksi kata ke bentuk dasar menggunakan pustaka `Sastrawi`.

---

## 📂 Struktur Proyek
```directory
├── ulasan_hayday_mentah.csv  # Dataset ulasan mentah (~12.000 baris)
├── README.md                 # Dokumentasi Proyek
└── Proyek_Analisis_Sentimen.ipynb # Notebook Utama Alur Eksperimen
```

---

## 🚀 Panduan Instalasi & Penggunaan

### 1. Prasyarat Sistem
Pastikan Anda telah menginstal pustaka yang diperlukan:
```bash
pip install google-play-scraper Sastrawi pandas numpy matplotlib seaborn scikit-learn tensorflow imbalanced-learn wordcloud nltk
```

### 2. Cara Menjalankan Pipeline & Inference
Untuk melakukan pengujian prediksi dengan teks baru secara mandiri menggunakan model LSTM yang telah dilatih:

```python
# Contoh melakukan inference prediksi
texts = ["Gamenya seru banget, grafiknya juga luar biasa!", "Sering crash setelah update baru, kecewa."]
cleaned_texts = [preprocess_text_inference(t) for t in texts]
sequences = tokenizer.texts_to_sequences(cleaned_texts)
padded = pad_sequences(sequences, maxlen=80)

predictions = (model_lstm.predict(padded) > 0.5).astype(int)
```
