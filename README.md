# Home Credit Default Risk Prediction

Proyek ini bertujuan untuk membangun model prediksi risiko gagal bayar (default) bagi pemohon pinjaman di Home Credit. Fokus utama adalah menggunakan berbagai sumber data alternatif untuk memastikan nasabah yang mampu membayar tidak ditolak dan mendapatkan layanan keuangan yang adekuat.

## 4. Tujuan, Sasaran, dan Metrik
*   **Tujuan:** Memprediksi probabilitas nasabah mengalami kesulitan pembayaran pinjaman (TARGET = 1).
*   **Sasaran:**
    *   Meningkatkan efisiensi proses persetujuan kredit.
    *   Meminimalkan risiko kredit macet (*Non-Performing Loan*).
    *   Mendukung inklusi keuangan bagi nasabah yang minim riwayat kredit formal.
*   **Metrik Evaluasi:** 
    *   **ROC-AUC Score:** Sebagai metrik utama untuk mengukur kemampuan model membedakan kelas pada data yang tidak seimbang (*imbalanced*).
    *   **Precision & Recall:** Untuk menyeimbangkan antara risiko salah tolak nasabah baik dan salah terima nasabah buruk.

## 5. Eksplorasi Data Awal (EDA)
Melakukan pemeriksaan mendalam terhadap dataset utama (`application_train.csv`) dan tabel pendukung lainnya untuk memahami:
*   Distribusi variabel target (menemukan bahwa data bersifat *imbalanced*).
*   Analisis nilai yang hilang (*missing values*) dan anomali data (seperti nilai `365243` pada kolom `DAYS_EMPLOYED`).
*   Korelasi antar variabel terhadap target.

## 6. Pembersihan dan Pengolahan Data
Tahapan ini meliputi:
*   **Handling Missing Values:** Menggunakan strategi imputasi median untuk fitur numerik.
*   **Handling Anomalies:** Memperbaiki nilai-nilai tidak masuk akal pada data pekerjaan dan umur.
*   **Encoding:** 
    *   *Label Encoding* untuk variabel kategorikal dengan 2 kategori.
    *   *One-Hot Encoding* untuk variabel kategorikal dengan lebih dari 2 kategori.
*   **Feature Scaling:** Standardisasi fitur agar memiliki skala yang seragam untuk model linear.

## 7. Rekayasa Fitur (Feature Engineering)
Membuat fitur baru berdasarkan wawasan bisnis untuk meningkatkan daya prediksi model, seperti:
*   `CREDIT_INCOME_PERCENT`: Rasio jumlah pinjaman terhadap pendapatan.
*   `ANNUITY_INCOME_PERCENT`: Rasio cicilan bulanan terhadap pendapatan.
*   `DAYS_EMPLOYED_PERCENT`: Persentase lama bekerja dibandingkan umur nasabah.

## 8. Pemodelan
Melakukan eksperimen dengan beberapa algoritma untuk mendapatkan performa terbaik:
*   **Regresi Logistik:** Sebagai model dasar (*baseline*) untuk melihat linearitas data.
*   **LightGBM:** Algoritma *gradient boosting* yang efisien untuk menangani dataset besar dan fitur kategorikal dalam jumlah banyak.
*   **Hyperparameter Tuning:** Melakukan optimasi pada parameter seperti `learning_rate`, `n_estimators`, dan `max_depth`.

## 9. Evaluasi
*   Membandingkan hasil skor AUC dari setiap model.
*   Menganalisis *Confusion Matrix* untuk melihat performa model dalam mendeteksi nasabah berisiko (True Positives).
*   Visualisasi kurva ROC untuk menilai stabilitas model pada berbagai ambang batas (*threshold*).

## 10. Dampak Bisnis dan Rekomendasi
*   **Dampak:** Penggunaan model diprediksi dapat menurunkan tingkat kerugian akibat gagal bayar hingga X% dengan cara mendeteksi calon peminjam berisiko tinggi lebih awal.
*   **Rekomendasi:**
    *   Nasabah dengan skor probabilitas tinggi (>0.5) harus diarahkan ke proses verifikasi manual yang lebih ketat.
    *   Integritas data pada tabel `bureau.csv` perlu ditingkatkan untuk memperkuat prediksi berdasarkan riwayat eksternal.

---

### Daftar File Data:
- `application_train/test.csv`: Data aplikasi utama (statis).
- `bureau.csv`: Riwayat kredit dari institusi finansial lain.
- `previous_application.csv`: Riwayat aplikasi pinjaman sebelumnya di Home Credit.
- `POS_CASH_balance.csv`: Snapshot saldo bulanan pinjaman kas/POS.
- `installments_payments.csv`: Riwayat pembayaran cicilan.
- `credit_card_balance.csv`: Snapshot saldo bulanan kartu kredit.