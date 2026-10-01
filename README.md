# Conditional GAN — Ship & Helicopter Image Generation

Project ini dibuat untuk mencoba **Conditional Generative Adversarial Network (GAN)** untuk menghasilkan gambar berdasarkan dua kelas, yaitu **ship** dan **helicopter**.

Di project ini saya membandingkan dua model:

* **Baseline GAN (3B)**
* **Modified GAN (3C)**

Tujuannya adalah melihat apakah perubahan pada arsitektur dan konfigurasi training dapat menghasilkan gambar yang lebih mendekati data asli.

---

## Dataset

Dataset yang digunakan terdiri dari dua kelas:

| Class      | Number of Images |
| ---------- | ---------------: |
| Ship       |            8,012 |
| Helicopter |            5,906 |
| **Total**  |       **13,918** |

Gambar diubah menjadi ukuran **28 × 28 × 3** sebelum digunakan untuk training.

Dataset dibagi menjadi:

* Training: 11,134 images
* Validation: 1,392 images
* Testing: 1,392 images

---

## How the GAN Works

GAN terdiri dari dua bagian utama:

**Generator**
Membuat gambar baru dari random noise dan label kelas.

**Discriminator**
Mencoba membedakan apakah gambar yang diberikan merupakan gambar asli atau gambar hasil generator.

Kedua model tersebut dilatih secara bersamaan. Generator berusaha menghasilkan gambar yang semakin sulit dibedakan dari gambar asli, sedangkan discriminator berusaha membedakan keduanya.

Karena project ini menggunakan dua kelas, yaitu ship dan helicopter, model dibuat menjadi **Conditional GAN** sehingga generator juga menerima informasi mengenai kelas gambar yang ingin dibuat.

---

## Baseline Model

Baseline digunakan sebagai model pembanding sebelum dilakukan modifikasi.

Beberapa konfigurasi yang digunakan:

* Image size: 28 × 28 × 3
* Batch size: 64
* Epochs: 300
* Learning rate: 0.0002
* Optimizer: Adam
* `beta_1`: 0.5
* Loss function: Binary Cross-Entropy
* Conditional labels: Ship / Helicopter

Pada baseline, generator menggunakan noise dan informasi label sebagai input untuk menghasilkan gambar.

---

## Modified Model

Pada bagian 3C, arsitektur dan beberapa konfigurasi training dimodifikasi.

Perubahan yang digunakan antara lain:

* Conditional label embedding
* Generator menggunakan beberapa Dense layer dan Batch Normalization
* LeakyReLU digunakan pada beberapa layer
* Generator menghasilkan gambar berukuran 28 × 28 × 3
* Discriminator menggunakan label sebagai informasi tambahan
* Dropout digunakan pada discriminator
* Learning rate Generator: **0.0001**
* Learning rate Discriminator: **0.0001**
* Batch size: 64
* Epochs: 300
* Adam `beta_1`: 0.5
* Real label smoothing: 0.9

Perubahan ini dilakukan untuk mencoba membuat proses training lebih stabil dan menghasilkan gambar yang lebih baik.

---

## Evaluation

Untuk membandingkan kedua model, digunakan **Fréchet Inception Distance (FID)**.

Secara sederhana, FID membandingkan distribusi fitur dari gambar hasil generate dengan distribusi fitur dari gambar asli.

**Semakin kecil nilai FID, semakin dekat hasil generate dengan data asli.**

### FID Results

| Model             |      FID |
| ----------------- | -------: |
| Baseline GAN (3B) | 433.6005 |
| Modified GAN (3C) | 409.7688 |

Modified GAN menghasilkan FID yang lebih rendah dibandingkan baseline.

* FID reduction: **23.8317**
* Percentage reduction: **5.49%**

Jadi, berdasarkan FID, modified GAN menghasilkan distribusi gambar yang lebih dekat dengan data asli dibandingkan baseline pada setup evaluasi yang digunakan.

---

## Results

Hasil generate dari model digunakan untuk melihat apakah Generator sudah dapat menghasilkan gambar yang sesuai dengan kelas yang diberikan.

Selain melihat hasil visual, FID digunakan sebagai evaluasi kuantitatif untuk membandingkan baseline dan modified model.

Hasilnya menunjukkan bahwa modified model mendapatkan FID yang lebih rendah:

**433.6005 → 409.7688**

atau turun sekitar **5.49%**.

Namun, FID tidak digunakan sebagai satu-satunya pertimbangan. Hasil gambar yang dihasilkan dan proses training juga perlu dilihat untuk mendapatkan gambaran yang lebih lengkap mengenai performa GAN.

---

## Project Structure

```text
Conditional-GAN-Ship-Helicopter/
│
├── DL_GAN.ipynb
├── README.md
│
└── results/
    ├── generated_samples.png
    └── fid_comparison.png
```

---

## Tools & Libraries

* Python
* TensorFlow / Keras
* NumPy
* Pandas
* OpenCV
* Matplotlib
* Scikit-learn

---

## What I Learned

Dari project ini, saya belajar bagaimana GAN bekerja dari sisi Generator dan Discriminator, serta bagaimana membuat Conditional GAN menggunakan informasi label.

Saya juga belajar bahwa mengevaluasi GAN tidak cukup hanya dengan melihat loss selama training. Hasil gambar perlu dilihat secara langsung dan dapat dibandingkan menggunakan metric seperti FID.

Project ini juga membantu saya memahami bagaimana perubahan arsitektur dan hyperparameter dapat memengaruhi hasil training model generatif.

---

## Author

**Felicia Tiffany**

Data Science Student — BINUS University
