### **1. Jelaskan Kodenya**

Notebook ini memanfaatkan **YOLOv8** untuk melakukan deteksi bahasa isyarat dari gambar atau video. Berikut tahapannya:

* **Instalasi & Setup:**

  * Instal `ultralytics` (YOLOv8) dan beberapa tools pendukung seperti `opencv-python` dan `ipython`.

* **Dataset:**

  * Dataset disimpan di Google Drive, berisi gambar gesture tangan per huruf.
  * Struktur dataset mengikuti format YOLO:

    ```
    /images/train, /images/val
    /labels/train, /labels/val
    data.yaml
    ```

* **Training:**

  * Menggunakan model YOLOv8n (versi nano) untuk efisiensi dan kecepatan.
  * Fine-tune dilakukan dengan memanggil:

    ```python
    model.train(data='data.yaml', epochs=50, imgsz=640, batch=16)
    ```
  * Parameter yang digunakan:

    * `epochs=50`: jumlah epoch pelatihan
    * `imgsz=640`: ukuran input gambar
    * `batch=16`: ukuran batch per iterasi

* **Evaluation & Inference:**

  * Model diuji pada gambar/video untuk mengenali huruf bahasa isyarat berdasarkan bounding box dan label hasil prediksi.

---

### **2. Cara Training Modelnya**

**Singkatnya:**

* Gunakan YOLOv8 dari Ultralytics.
* Dataset gambar dilabeli dalam format YOLO (label `.txt` berisi kelas dan bounding box).
* File konfigurasi `data.yaml` berisi path dataset dan nama-nama kelas gesture.
* Jalankan `model.train(...)` di Google Colab dengan GPU untuk melatih model deteksi gesture tangan.

---

### **3. Gabungkan Model ke Web-nya Gimana?**

**Pipeline-nya seperti ini:**

1. **Frontend (React):**

   * Kamera terbuka dan menangkap input gesture.
   * Gambar/video dikirim via Axios ke backend Flask.

2. **Flask (Model Inference):**

   * YOLOv8 digunakan untuk deteksi gesture dari input.
   * Model diload dari file hasil training (`best.pt`).
   * Model memproses dan mengembalikan:

     * Label gesture
     * Confidence score
     * Bounding box

3. **Respons ke Frontend:**

   * Frontend menampilkan hasil deteksi gesture secara real-time.

---

### **4. Kenapa Web-nya Pakai FE dan BE Terpisah**

**Alasan kuatnya:**

* **Modular dan scalable:** Proyek dibagi menjadi frontend (React), backend (NestJS untuk user API), dan model server (Flask) → pengembangan lebih fleksibel.
* **Kinerja:** Model inference (YOLO) berat, jadi lebih baik ditangani backend tersendiri (Flask).
* **Deployment terpisah:** React bisa di-deploy di Vercel, Flask di Cloud Run, NestJS di Docker → efisien & bebas scaling.

---

### **5. Kenapa Dataset-nya Tidak Banyak**

**Kemungkinan penyebabnya:**

* **Dataset dikumpulkan sendiri:** Proyek ini tidak menggunakan dataset publik besar seperti ASL Alphabet atau RWTH-BOSTON.
* **Fokus MVP:** Untuk tahap awal, hanya beberapa huruf/gesture dilatih agar sistem berjalan dengan baik.
* **Keterbatasan resource:** Mengumpulkan dan melabeli data gesture tangan sangat memakan waktu.
* **Model ringan:** YOLOv8n dilatih untuk cepat, bukan untuk klasifikasi dengan jutaan contoh.

---

Jika kamu ingin bantu membuat **dataset lebih banyak atau memperluas gesture-nya**, aku bisa bantu rancang format data dan strategi augmentasi juga. Mau lanjut ke situ?
