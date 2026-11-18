# Materi Minggu 5: Fault Tolerance 

| Nama | NIM |
| --- | --- |
| Akili Rafie Hendarto | 255410007 |

Prompt:

Saya akan mengunggah sebuah modul PDF. Tolong baca dan pahami seluruh isi modul dari awal hingga akhir, kemudian buat rangkuman yang lengkap, sistematis, dan mudah dipahami.
Ketentuan rangkuman:
1. Susun materi sesuai urutan bab dan subbab dalam modul.
2. Jelaskan setiap konsep, definisi, fungsi, dan istilah penting dengan bahasa Indonesia yang sederhana dan jelas.
3. Jangan melewatkan materi penting, termasuk rumus, tabel, gambar, diagram, dan contoh.
4. Jika terdapat kode program, jelaskan fungsi, cara kerja, dan output-nya.
5. Sertakan contoh jika diperlukan untuk memperjelas materi.
6. Gunakan judul, subjudul, poin-poin, atau tabel agar rangkuman mudah dipelajari.
7. Hindari pengulangan dan jangan menambahkan informasi yang tidak relevan.
Pada bagian akhir, sertakan kesimpulan, daftar istilah penting, dan beberapa pertanyaan latihan beserta jawabannya.

Penting:
- Utamakan kelengkapan dan ketepatan materi, bukan sekadar membuat rangkuman singkat.
- Pastikan seluruh isi PDF sudah tercakup agar dapat digunakan sebagai bahan belajar dan persiapan ujian.
- Sajikan seluruh hasil rangkuman langsung dalam format Markdown (.md) yang rapi dan siap disalin atau disimpan sebagai file Markdown.
- Hapus seluruh penanda sitasi, referensi otomatis, dan kode kutipan seperti [CITE:...] dari hasil akhir.
- Jangan menampilkan sitasi pada setiap kalimat atau paragraf. Sajikan rangkuman sebagai teks bersih tanpa penanda sitasi.
---

Bagian yang disesuaikan dari keluaran prompt ada dibawah ini (namun udah saya masukan di Rangkuman):
## 5. Teknik Desain untuk Mencapai Fault Tolerance (Design Patterns)
Terdapat 5 teknik utama yang digunakan untuk meningkatkan ketahanan terhadap kesalahan dalam sistem terdistribusi:

### 1. Redundansi dan Replikasi (*Redundancy & Replication*)
* Sumber daya tidak disimpan di satu tempat saja, melainkan direplikasi ke berbagai node di dalam jaringan.
* Tujuannya agar tidak ada titik kegagalan tunggal (*single point of failure* / SPOF).

### 2. Penyeimbangan Beban (*Load Balancing*)
* Proses memperluas kapasitas (*scaling*) aplikasi menjadi lebih dari satu instans aktif (*instance* merujuk pada satu proses aplikasi penuh yang berjalan).
* Menggunakan penyeimbang beban (*load balancer* seperti HAProxy atau Nginx) untuk mendistribusikan trafik ke berbagai instans aplikasi.

### 3. Checkpointing dan Rollback Recovery
* **Checkpointing:** Proses menyimpan status sistem secara berkala ke media penyimpanan.
* **Rollback Recovery:** Proses mengembalikan sistem ke titik *checkpoint* sebelumnya setelah terjadi kegagalan.
* Teknik ini memungkinkan sistem pulih dengan melanjutkan proses dari titik simpan terakhir (bukan dari awal), sehingga meminimalkan kehilangan data dan waktu.

### 4. Heartbeat dan Failure Detection
* **Heartbeat:** Sinyal periodik yang dikirim antar-komponen untuk menyatakan bahwa komponen tersebut masih aktif.
* **Failure Detection:** Proses memantau *heartbeat*. Jika suatu komponen pemantau tidak menerima *heartbeat* dalam batas waktu tertentu, komponen tersebut diasumsikan telah gagal atau tidak responsif.
* Diimplementasikan melalui skrip pemeriksaan server sederhana maupun pustaka manajemen koneksi seperti **tenacity** untuk menangani percobaan ulang (*retries*) saat koneksi luar gagal.

### 5. Pola Pemutus Sirkuit (*Circuit Breaker Pattern*)
* Pola desain yang digunakan dalam arsitektur *microservices* untuk mencegah kegagalan berantai (*cascading failure*) dengan cara memblokir sementara panggilan ke layanan yang sedang bermasalah.
* Bekerja sebagai *state machine* dengan 3 status utama:
  * **Closed:** Permintaan diizinkan lewat dan terus dipantau. Jika tingkat kegagalan melampaui batas, sistem beralih ke status *open*.
  * **Open:** Permintaan langsung ditolak tanpa memanggil layanan tujuan, memberikan waktu bagi layanan tersebut untuk pulih.
  * **Half-Open:** Setelah batas waktu tertentu, sistem mengizinkan sejumlah kecil permintaan pengujian untuk mengecek apakah layanan sudah pulih. Jika berhasil, status kembali ke *closed*; jika gagal, kembali ke *open*.
* Dapat diimplementasikan di Python menggunakan pustaka seperti `pybreaker`.
---
**MATERI HASIL AI (MANUS) DIBAWAH INI:**

**TERMASUK YANG SAYA TAMBAHKAN**

## 1. Tujuan Pembelajaran
Setelah mempelajari modul ini, mahasiswa diharapkan mampu:
1. Menjelaskan mengapa *fault tolerance* diperlukan pada sistem terdistribusi dan terdesentralisasi.
2. Menjelaskan pengertian dari *fault tolerance* dan *fault tolerant systems*.
3. Menyebutkan dan menjelaskan berbagai jenis *fault* dalam suatu sistem teknologi informasi.
4. Menjelaskan klasifikasi *fault* berdasarkan sumbernya.
5. Memahami berbagai pola desain (*design patterns*) untuk mencapai *fault tolerance* serta mengaplikasikannya menggunakan Python.
6. Memahami implementasi *fault tolerance* pada berbagai bahasa pemrograman dan peranti pengembangan.

---

## 2. Pengertian dan Arti Penting Fault Tolerance

### A. Pengertian
* **Fault Tolerance:** Kemampuan suatu sistem untuk mempertahankan operasional sistem dengan tepat meskipun terjadi kegagalan atau kesalahan pada satu atau lebih komponennya.
* Kemampuan ini sangat krusial diterapkan pada sistem dengan ketersediaan tinggi (*high availability*), sistem kritis misi (*mission-critical*), maupun sistem yang berkaitan dengan keselamatan nyawa (*life-critical system*).

### B. Arti Penting pada Sistem Terdistribusi & Terdesentralisasi
* Sistem terdistribusi dan terdesentralisasi memiliki banyak komponen yang saling terhubung melalui jaringan.
* Karena jalur koneksi dan komponen yang beragam, potensi kegagalan selalu ada. Semakin krusial suatu aplikasi, semakin tinggi pula tuntutan untuk tetap berfungsi tanpa henti (*high availability*), sehingga risiko kegagalan wajib diantisipasi sejak tahap perancangan.

---

## 3. Jenis-Jenis Fault (Kesalahan)
Dalam sistem komputer dan jaringan, kesalahan dikategorikan ke dalam empat jenis utama:
1. **Transient Fault:** Kesalahan yang bersifat sementara dan hanya terjadi sesaat, misalnya gangguan sinyal akibat sambaran petir.
2. **Intermittent Fault:** Kesalahan yang muncul dan hilang secara berulang, misalnya kegagalan komponen akibat perangkat keras mengalami *overheat* (panas berlebih).
3. **Permanent Fault:** Kesalahan yang bersifat tetap dan permanen sampai komponen yang rusak diperbaiki atau diganti.
4. **Byzantine Fault:** Kesalahan kompleks di mana komponen gagal secara tidak terduga dan memberikan hasil acak, salah, atau tidak konsisten kepada bagian sistem lainnya.
   * *Byzantine Fault Tolerance (BFT):* Mekanisme untuk mengatasi komponen pengkhianat (*traitor*) di dalam jaringan terdesentralisasi agar sistem tetap mencapai konsensus yang benar.

---

## 4. Klasifikasi Fault Berdasarkan Sumbernya
1. **Hardware Faults:** Kerusakan fisik perangkat keras, seperti kerusakan *disk crash* atau kegagalan catu daya (*power failure*).
2. **Software Faults:** Kesalahan pada kode program, seperti *bugs*, kebocoran memori (*memory leak*), atau *race condition*.
3. **Communication Faults:** Gangguan pada transmisi jaringan, seperti kehilangan paket data (*packet loss*) atau terputusnya sebagian jaringan (*network partition*).

---

## 5. Teknik Desain untuk Mencapai Fault Tolerance (Design Patterns)
Terdapat 5 teknik utama yang digunakan untuk meningkatkan ketahanan terhadap kesalahan dalam sistem terdistribusi:

### 1. Redundansi dan Replikasi (*Redundancy & Replication*)
* Sumber daya tidak disimpan di satu tempat saja, melainkan direplikasi ke berbagai node di dalam jaringan.
* Tujuannya agar tidak ada titik kegagalan tunggal (*single point of failure* / SPOF).

### 2. Penyeimbangan Beban (*Load Balancing*)
* Proses memperluas kapasitas (*scaling*) aplikasi menjadi lebih dari satu instans aktif (*instance* merujuk pada satu proses aplikasi penuh yang berjalan).
* Menggunakan penyeimbang beban (*load balancer* seperti HAProxy atau Nginx) untuk mendistribusikan trafik ke berbagai instans aplikasi.

### 3. Checkpointing dan Rollback Recovery
* **Checkpointing:** Proses menyimpan status sistem secara berkala ke media penyimpanan.
* **Rollback Recovery:** Proses mengembalikan sistem ke titik *checkpoint* sebelumnya setelah terjadi kegagalan.
* Teknik ini memungkinkan sistem pulih dengan melanjutkan proses dari titik simpan terakhir (bukan dari awal), sehingga meminimalkan kehilangan data dan waktu.

### 4. Heartbeat dan Failure Detection
* **Heartbeat:** Sinyal periodik yang dikirim antar-komponen untuk menyatakan bahwa komponen tersebut masih aktif.
* **Failure Detection:** Proses memantau *heartbeat*. Jika suatu komponen pemantau tidak menerima *heartbeat* dalam batas waktu tertentu, komponen tersebut diasumsikan telah gagal atau tidak responsif.
* Diimplementasikan melalui skrip pemeriksaan server sederhana maupun pustaka manajemen koneksi seperti **tenacity** untuk menangani percobaan ulang (*retries*) saat koneksi luar gagal.

### 5. Pola Pemutus Sirkuit (*Circuit Breaker Pattern*)
* Pola desain yang digunakan dalam arsitektur *microservices* untuk mencegah kegagalan berantai (*cascading failure*) dengan cara memblokir sementara panggilan ke layanan yang sedang bermasalah.
* Bekerja sebagai *state machine* dengan 3 status utama:
  * **Closed:** Permintaan diizinkan lewat dan terus dipantau. Jika tingkat kegagalan melampaui batas, sistem beralih ke status *open*.
  * **Open:** Permintaan langsung ditolak tanpa memanggil layanan tujuan, memberikan waktu bagi layanan tersebut untuk pulih.
  * **Half-Open:** Setelah batas waktu tertentu, sistem mengizinkan sejumlah kecil permintaan pengujian untuk mengecek apakah layanan sudah pulih. Jika berhasil, status kembali ke *closed*; jika gagal, kembali ke *open*.
* Dapat diimplementasikan di Python menggunakan pustaka seperti `pybreaker`.

---

## 6. Consensus and Coordination
* **Consensus (Konsensus):** Proses di mana beberapa node dalam sistem terdistribusi menyepakati satu nilai, status, atau urutan operasi yang sama, meskipun terjadi gangguan jaringan atau penundaan.
* **Coordination (Koordinasi):** Praktik yang lebih luas dalam mengelola interaksi antar-node agar bekerja sama secara konsisten (sering kali melibatkan konsensus sebagai inti).
* **Contoh Penerapan:**
  * *Database leader election:* Pemilihan node pemimpin (*leader*) menggunakan algoritma seperti **Raft** untuk menangani operasi tulis (*write*).
  * *Cloud storage file updates:* Memastikan perubahan file disetujui oleh mayoritas server sebelum dianggap final (*eventual consistency*).
  * *Cryptocurrency & Blockchain:* Validasi transaksi terdesentralisasi.

---

## 7. Actor Model untuk Supervisi Proses
* **Konsep Model Aktor:** Entitas mandiri (*Actor*) saling berkomunikasi secara asinkron dengan mengirimkan pesan (*message*) ke dalam *Mailbox* aktor lain, tanpa memerlukan *shared memory*.
* **Pohon Pengawas (*Supervisor Tree*):** Proses diatur secara hierarkis di mana aktor tingkat atas (*top-level supervisor*) memantau kesehatan aktor di bawahnya (*check if alive*). Jika sebuah aktor pekerja mengalami kegagalan, supervisor dapat segera melakukan pemulihan otomatis (*restart*) tanpa merusak keseluruhan sistem.

---

## 8. Implementasi Fault Tolerance pada Berbagai Bahasa Pemrograman
* **Erlang / Elixir:** Memiliki mekanisme bawaan (*built-in*) untuk model aktor dan arsitektur pengawas (*Supervisor-Worker*).
* **Pony:** Bahasa pemrograman yang dirancang khusus dengan fokus bawaan pada *Actor model* (`ponylang.io`).
* **Python:** Memanfaatkan pustaka pihak ketiga seperti **Xoscar**, **tenacity**, dan **pybreaker**.
* **Java:** Menggunakan **Apache Pekko** (model aktor dan Supervisor-Worker) serta **Resilience4j** untuk *fault tolerance*.
* **Bahasa Lain:** Tersedia berbagai pustaka ekosistem seperti npm untuk JavaScript atau crates.io untuk Rust.