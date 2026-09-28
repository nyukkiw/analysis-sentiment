# analysis-sentiment

Analisis sentimen ulasan aplikasi **MyPertamina** menggunakan terjemahan mesin (Indonesia → Inggris) dan **VADER**, lalu divalidasi terhadap rating bintang ulasan.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nyukkiw/analysis-sentiment/blob/main/AS.ipynb)

## Alur

1. Baca `myPertamina.csv`, ambil sampel `MAX_ROWS` baris (default 500, `None` = semua).
2. Terjemahkan kolom `content` ke bahasa Inggris dengan `deep-translator` (Google Translate). Hasilnya di-cache di `translation_cache.csv`, jadi kalau proses terhenti bisa dilanjutkan tanpa menerjemahkan ulang.
3. Hitung skor `compound` VADER, lalu beri label `positive` (≥ 0.05) / `negative` (≤ -0.05) / `neutral`.
4. Simpan ke `myPertamina_labeled.csv`.
5. Eksplorasi: distribusi label, ulasan paling positif/negatif.
6. Validasi: bandingkan label VADER dengan rating bintang (1–2 negative, 3 neutral, 4–5 positive) lewat confusion matrix, akurasi, dan classification report.

Teks kosong dan terjemahan yang gagal **tidak** dihitung sebagai `neutral`; statusnya dicatat di kolom `translate_status` (`ok` / `empty` / `failed`) dan labelnya dibiarkan kosong.

## Data

File CSV tidak disimpan di repo (lihat `.gitignore`). Notebook membutuhkan file `myPertamina.csv` dengan minimal kolom:

| Kolom     | Isi                         |
|-----------|-----------------------------|
| `content` | teks ulasan (bahasa Indonesia) |
| `score`   | rating bintang 1–5          |

## Menjalankan

**Colab:** buka lewat badge di atas, upload `myPertamina.csv` ke panel *Files*, lalu *Run all*.

**Lokal:**

```bash
pip install -r requirements.txt jupyter
jupyter notebook AS.ipynb
```

## Keterbatasan

- Error terjemahan mesin (slang, singkatan, typo) ikut terbawa ke VADER.
- VADER berbasis leksikon bahasa Inggris: tidak menangkap sarkasme atau keluhan yang ditulis sopan.
- `deep-translator` memakai endpoint web Google Translate, bukan API resmi; bisa terkena rate limit.
- Rating bintang hanya pendekatan *ground truth*, bukan label sentimen yang sempurna.
