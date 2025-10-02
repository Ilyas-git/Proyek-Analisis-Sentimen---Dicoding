# Proyek-Analisis-Sentimen---Dicoding

# Analisis Sentimen Ulasan Aplikasi Duolingo (ID)

Proyek ini melakukan **analisis sentimen** pada ulasan aplikasi **Duolingo** berbahasa Indonesia. Pipeline dibangun dari notebook `File_pelatihan_model.ipynb` dan mencakup pembersihan teks, pelabelan sentimen dengan leksikon Indonesia, ekstraksi fitur **TF–IDF**, serta pemodelan menggunakan beberapa algoritma pembelajaran mesin.

> **Model terbaik:** *Logistic Regression* dengan **akurasi data uji ≈ 89,37%**.

---

## 🔍 Ringkasan Proyek
- **Dataset**: `ulasan_aplikasi_duolingo.csv` (13.500 baris)
- **Distribusi label** (hasil pelabelan leksikon):  
  - `negative`: 7.185  
  - `positive`: 6.129
- **Split data**: 80% latih, 20% uji (`test_size=0.2`, `random_state=42`)
- **Ekstraksi fitur**: `TfidfVectorizer(max_features=200, min_df=17, max_df=0.8)`
- **Algoritma yang dicoba**: Bernoulli Naive Bayes, Decision Tree, Random Forest, Logistic Regression
- **Evaluasi utama**: akurasi

### Hasil Akurasi
| Model                 | Akurasi Train | Akurasi Test |
|----------------------|---------------|--------------|
| Naive Bayes          | 81,63%        | 80,77%       |
| Decision Tree        | 98,89%        | 81,07%       |
| Random Forest        | 98,89%        | 85,02%       |
| **Logistic Regression** | 90,27%     | **89,37%**   |

> Catatan: angka dibulatkan dari output notebook.

---

## 🧰 Dependensi
Library utama yang digunakan:
- `pandas`, `numpy`
- `matplotlib`, `seaborn`
- `nltk`
- `sastrawi` (stemming & stopword Indonesia)
- `scikit-learn`

### Instalasi Cepat
```bash
# (opsional) buat environment baru
conda create -n duolingo-sentimen python=3.10 -y
conda activate duolingo-sentimen

# install dependensi via pip
pip install pandas numpy matplotlib seaborn nltk sastrawi scikit-learn
```

> Di notebook aslinya ada perintah `!pip install sastrawi`. Pastikan paket tersebut terpasang sebelum menjalankan sel preprocessing.

---

## 🧹 Tahapan Preprocessing
Dilakukan berurutan sebagai berikut (fungsi tersedia di notebook):
1. **`cleaningText`**: hapus mention, hashtag, “RT”, tautan, angka, tanda baca, newline.
2. **`casefoldingText`**: ubah ke huruf kecil.
3. **Normalisasi slang** (**`fix_slangwords`**): ganti kata gaul/singkatan menjadi bentuk baku (kamus slang disediakan di notebook).
4. **Tokenisasi** (**`tokenizingText`**).
5. **Stopword removal** (**`filteringText`**): memakai daftar stopword (NLTK/Sastrawi).
6. **Stemming** (**`stemmingText`**): stemmer Sastrawi.
7. **Rekonstruksi kalimat** (**`toSentence`**) → `text_akhir`.

---

## 🏷️ Pelabelan Sentimen
Menggunakan pendekatan **leksikon Indonesia** melalui fungsi **`sentiment_analysis_lexicon_indonesia`** yang:
- Menjumlahkan skor kata positif/negatif dari kamus leksikon,
- Menghasilkan **`polarity_score`** dan label **`polarity`** (`positive`/`negative`).

Label inilah yang dipakai sebagai target saat pelatihan model.

---

## 🧠 Pemodelan
1. **Fitur**: TF–IDF dari kolom `text_akhir` → `X_tfidf`
2. **Target**: `polarity`
3. **Split**: `train_test_split(X_tfidf, y, test_size=0.2, random_state=42)`
4. **Model**:
   - **Bernoulli Naive Bayes**
   - **Decision Tree**
   - **Random Forest**
   - **Logistic Regression**
5. **Evaluasi**: akurasi pada data latih & uji. (Dapat ditambah `confusion_matrix`/`classification_report` bila perlu.)

---

## ▶️ Cara Menjalankan
1. Letakkan **`ulasan_aplikasi_duolingo.csv`** di direktori proyek yang sama dengan notebook.
2. Buka notebook `File_pelatihan_model.ipynb` (Jupyter/Lab/VS Code) dan jalankan sel secara berurutan.
3. Lihat ringkasan hasil akurasi di bagian akhir notebook.

### Menjalankan Cepat (VS Code / JupyterLab)
- Buka folder proyek → buka notebook → **Run All**.
- Jika memakai JupyterLab: `jupyter lab` lalu buka notebook.

---

## 📂 Struktur Direktori (saran)
```
.
├─ File_pelatihan_model.ipynb
├─ ulasan_aplikasi_duolingo.csv
├─ data/                 # (opsional) sumber data mentah
├─ models/               # (opsional) simpan model terlatih
└─ README.md
```

---

## 📈 Ide Pengembangan Lanjutan
- Tambah **`max_features`** TF–IDF dan lakukan **pencarian hiperparameter** (Grid/Random Search).
- Coba model lain: **Linear SVM**, **XGBoost**, atau **Logistic Regression** dengan regularisasi & class weight.
- Tambahkan metrik: **confusion matrix**, **precision/recall/F1**, **ROC-AUC**.
- Lakukan **kros-validasi** untuk estimasi generalisasi yang lebih stabil.
- Eksperimen **lemmatization** bahasa Indonesia (bila tersedia) atau **word embeddings**.

---

## ✍️ Lisensi
Gunakan lisensi sesuai kebutuhan (mis. MIT). Jika dataset berasal dari pihak ketiga, pastikan mematuhi ketentuan penggunaan datanya.

---

_Diperbarui: 2025-10-02 11:03 _
