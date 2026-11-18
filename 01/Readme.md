# Materi Minggu 1: Pengenalan Sistem Terdistribusi dan Terdesentralisasi

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
## 4. Arsitektur Sistem Dasar
Secara fundamental, komunikasi antar-node dalam jaringan dan sistem terdistribusi terbagi menjadi tiga model arsitektur dasar:

### A. Client-Server
* **Cara Kerja:** Terdapat pemisahan peran yang jelas antara **Server** (penyedia layanan/sumber daya) dan **Clients** (perangkat seperti laptop, ponsel, dan komputer desktop yang meminta layanan melalui internet).
* **Kelebihan & Kekurangan:** Mudah dikelola secara terpusat, namun server pusat berpotensi menjadi titik kemacetan (*bottleneck*) atau titik kegagalan tunggal (*single point of failure*).

### B. Peer-to-Peer (P2P)
* **Cara Kerja:** Seluruh komputer (*node*) dalam jaringan memiliki kedudukan dan hak yang setara (*equal peers*). Setiap node dapat bertindak sekaligus sebagai *client* (pengunduh/peminta data) dan *server* (penyedia data) tanpa memerlukan server pusat.
* **Contoh Penerapan:** Protokol berbagi file **BitTorrent**, serta berbagai jaringan aset kripto dan blockchain seperti **Bitcoin, Ethereum, Ripple, Uniswap, Cardano, Dogecoin, Chainlink, Litecoin, dan Theta**.

### C. Hybrid (Kombinasi Client-Server dan P2P)
* **Cara Kerja:** Menggabungkan efisiensi koordinasi arsitektur *Client-Server* dengan kecepatan distribusi *Peer-to-Peer*. Dalam diagram arsitektur *Hybrid*, terdapat node khusus berkapasitas tinggi yang disebut **Supernodes** yang saling terhubung (*Supernode Connection*) dan mengatur kelompok klien di bawahnya.
* **Alur Komunikasi:**
  * Klien mengirim permintaan (*Client Request*) ke *Supernode* dan menerima respons (*Server Response*) layaknya pola *Client-Server*.
  * Untuk pertukaran data berkapasitas besar, antar-klien dapat melakukan transfer langsung secara *Peer-to-Peer File-sharing*.
* **Contoh Penerapan:** Aplikasi pesan instan modern seperti **WhatsApp, Telegram, dan Signal**.

---

**MATERI HASIL AI (MANUS) ADA DISINI:**

**(Termasuk dengan yang saya tambahin)**

## 1. Tujuan Pembelajaran
Setelah mempelajari modul ini, mahasiswa diharapkan mampu:
1. Menjelaskan konsep dasar **sistem terdistribusi**.
2. Menjelaskan konsep dasar **sistem terdesentralisasi**.
3. Menjelaskan **perbedaan** antara sistem terdistribusi dan sistem terdesentralisasi.
4. Menyebutkan berbagai **contoh penerapan nyata** dari kedua sistem tersebut.
5. Menyebutkan **tantangan** serta **tren terbaru** di dunia sistem terdistribusi dan terdesentralisasi.

---

## 2. Arti Penting Sistem Terdistribusi dan Terdesentralisasi
Sistem terdistribusi dan terdesentralisasi menjadi fondasi utama infrastruktur teknologi modern karena beberapa alasan mendasar:
* **Pertumbuhan data dan pengguna yang sangat besar:** Skala data global dan jumlah pengguna internet meningkat eksponensial sehingga satu mesin tunggal (*single server*) tidak lagi sanggup menanganinya.
* **Kebutuhan skalabilitas layanan modern:** Sistem harus mampu beradaptasi saat terjadi lonjakan trafik pengguna.
* **Ketahanan terhadap kegagalan (*Fault-Tolerant*):** Layanan tidak boleh mati total hanya karena satu komponen rusak.
* **Performa dan ketersediaan 24/7:** Layanan digital modern dituntut aktif terus-menerus tanpa henti dengan kecepatan akses tinggi.
* **Fondasi platform raksasa:** Tanpa arsitektur distribusi dan desentralisasi, layanan berskala global seperti Google, Netflix, maupun teknologi Blockchain tidak mungkin beroperasi.

### Definisi Penting
* **Skalabilitas (*Scalability*):** Kemampuan suatu sistem, jaringan, proses, atau organisasi untuk berkembang dan menangani peningkatan beban kerja, jumlah pengguna, atau permintaan tanpa mengorbankan kinerja maupun kualitas layanan.
* **Sistem *Fault-Tolerant*:** Sistem yang memiliki kemampuan untuk tetap beroperasi secara normal meskipun terjadi kegagalan atau malfungsi pada sebagian komponennya, baik pada perangkat keras (*hardware*) maupun perangkat lunak (*software*).

---

## 3. Pengertian Sistem Terdistribusi
Menurut **Andrew S. Tanenbaum dan Maarten van Steen (2007)**, sistem terdistribusi didefinisikan sebagai:
> *"A collection of independent computers that appears to its users as a single coherent system."*
> (Sekumpulan komputer independen yang tampak bagi penggunanya sebagai satu kesatuan sistem yang utuh/koheren).

### Pilar Utama Sistem Terdistribusi
1. **Komponen Independen:** Terdiri atas banyak komputer/node otonom yang masing-masing memiliki prosesor dan memori sendiri.
2. **Berkomunikasi melalui Jaringan:** Antar-komponen saling bertukar pesan (*message passing*) melalui jaringan komputer.
3. **Transparansi:** Menyembunyikan kompleksitas arsitektur terdistribusi dari pengguna sehingga terasa seperti satu sistem tunggal. Jenis transparansi meliputi:
   * **Transparansi Lokasi:** Pengguna tidak perlu tahu di mana letak fisik server atau data berada.
   * **Transparansi Akses:** Cara mengakses sumber daya lokal dan sumber daya jarak jauh (*remote*) terasa identik.
   * **Transparansi Replikasi:** Pengguna tidak menyadari bahwa data sebenarnya digandakan (*replicated*) di banyak server demi keandalan.
   * **Transparansi Kegagalan:** Ketika salah satu server gagal dan tugas dialihkan ke server cadangan, pengguna tidak merasakan gangguan.

### Penjelasan Diagram Arsitektur Aplikasi Modern (Microservices)
Pada modul terdapat ilustrasi arsitektur e-commerce berbasis layanan terdistribusi:
* **Klien (*Mobile App* & *Browser*):** Pengguna mengakses sistem melalui aplikasi ponsel atau peramban web.
* **API Gateway & Storefront WebApp:** Permintaan dari *Mobile App* masuk melalui *REST API* ke **API Gateway**, sedangkan *Browser* mengakses **Storefront WebApp** melalui protokol Web.
* **Layanan Independen (*Microservices*):** *API Gateway* dan *Storefront WebApp* meneruskan permintaan melalui *REST API* ke layanan-layanan terpisah sesuai fungsinya, yaitu **Account Service**, **Inventory Service**, dan **Shipping Service**.
* **Basis Data Terpisah:** Masing-masing layanan mengelola basis datanya sendiri secara independen (**Account DB**, **Inventory DB**, dan **Shipping DB**).

---

## 4. Arsitektur Sistem Dasar
Secara fundamental, komunikasi antar-node dalam jaringan dan sistem terdistribusi terbagi menjadi tiga model arsitektur dasar:

### A. Client-Server
* **Cara Kerja:** Terdapat pemisahan peran yang jelas antara **Server** (penyedia layanan/sumber daya) dan **Clients** (perangkat seperti laptop, ponsel, dan komputer desktop yang meminta layanan melalui internet).
* **Kelebihan & Kekurangan:** Mudah dikelola secara terpusat, namun server pusat berpotensi menjadi titik kemacetan (*bottleneck*) atau titik kegagalan tunggal (*single point of failure*).

### B. Peer-to-Peer (P2P)
* **Cara Kerja:** Seluruh komputer (*node*) dalam jaringan memiliki kedudukan dan hak yang setara (*equal peers*). Setiap node dapat bertindak sekaligus sebagai *client* (pengunduh/peminta data) dan *server* (penyedia data) tanpa memerlukan server pusat.
* **Contoh Penerapan:** Protokol berbagi file **BitTorrent**, serta berbagai jaringan aset kripto dan blockchain seperti **Bitcoin, Ethereum, Ripple, Uniswap, Cardano, Dogecoin, Chainlink, Litecoin, dan Theta**.

### C. Hybrid (Kombinasi Client-Server dan P2P)
* **Cara Kerja:** Menggabungkan efisiensi koordinasi arsitektur *Client-Server* dengan kecepatan distribusi *Peer-to-Peer*. Dalam diagram arsitektur *Hybrid*, terdapat node khusus berkapasitas tinggi yang disebut **Supernodes** yang saling terhubung (*Supernode Connection*) dan mengatur kelompok klien di bawahnya.
* **Alur Komunikasi:**
  * Klien mengirim permintaan (*Client Request*) ke *Supernode* dan menerima respons (*Server Response*) layaknya pola *Client-Server*.
  * Untuk pertukaran data berkapasitas besar, antar-klien dapat melakukan transfer langsung secara *Peer-to-Peer File-sharing*.
* **Contoh Penerapan:** Aplikasi pesan instan modern seperti **WhatsApp, Telegram, dan Signal**.

---

## 5. Karakteristik Sistem Terdistribusi
Sebuah sistem terdistribusi yang baik memiliki lima karakteristik utama:
1. **Transparansi Lokasi (*Location Transparency*):** Pengguna maupun aplikasi dapat mengakses sumber daya tanpa mengetahui posisi geografis atau alamat fisik perangkat kerasnya.
2. **Skalabilitas (*Scalability*):** Mampu diperluas kapasitasnya (misalnya menambah jumlah server secara horizontal) ketika beban trafik meningkat.
3. **Fault Tolerance:** Mampu menoleransi kerusakan sebagian node/jaringan dan tetap memberikan layanan tanpa terhenti.
4. **Kinerja Tinggi (*High Performance*):** Memanfaatkan pemrosesan paralel di banyak mesin untuk mempercepat eksekusi tugas dan waktu respons.
5. **Heterogenitas (*Heterogeneity*):** Mampu beroperasi di atas komponen yang beragam, baik dari segi perangkat keras, sistem operasi, bahasa pemrograman, maupun jenis jaringan.

---

## 6. Pengertian Sistem Terdesentralisasi
* **Sentralisasi:** Sistem di mana seluruh wewenang, pemrosesan, dan pengambilan keputusan berada pada **1 titik kontrol pusat**.
* **Desentralisasi:** Sistem yang melakukan **distribusi kontrol dan wewenang** ke banyak pihak/node sehingga **tidak ada *single point of failure*** (titik kegagalan tunggal) maupun otoritas tunggal yang mendominasi.
* **Keputusan Kolektif melalui Konsensus:** Karena tidak ada bos/server pusat, kesepakatan mengenai status data yang valid dicapai secara kolektif oleh seluruh anggota jaringan menggunakan mekanisme/algoritma konsensus.
* **Contoh:** Blockchain, DNS (*Domain Name System*), dan BitTorrent.

### Penjelasan Diagram Topologi Jaringan Paul Baran (1964)
Modul merujuk pada diagram klasik karya **Paul Baran (1964)** dalam *On Distributed Communications*:
* **(A) Centralized (Tersentralisasi):** Berbentuk pola bintang (*star*). Semua node pinggiran (*station*) terhubung langsung ke **satu node pusat**. Jika node pusat mati, seluruh jaringan lumpuh.
* **(B) Decentralized (Terdesentralisasi):** Berbentuk kumpulan beberapa klaster bintang yang saling terhubung. Terdapat beberapa node koordinator lokal (pusat-pusat kecil), sehingga jika satu pusat klaster mati, klaster lain masih tetap berjalan.
* **(C) Distributed (Terdistribusi):** Berbentuk jaring (*mesh*) di mana setiap node terhubung dengan banyak node tetangganya secara merata tanpa ada satu pun pusat hierarki. Jalur komunikasi sangat fleksibel dan tahan terhadap pemutusan jalur.

---

## 7. Perbandingan: Sistem Terdistribusi vs Sistem Terdesentralisasi
Meskipun sering digunakan bersamaan, keduanya memiliki fokus arsitektur yang berbeda:

| Aspek Perbandingan | Sistem Terdistribusi | Sistem Terdesentralisasi |
| :--- | :--- | :--- |
| **Fokus Utama** | **Pembagian beban kerja** (*workload*) dan pemrosesan data agar cepat dan efisien. | **Pembagian kontrol/otoritas** agar tidak ada pihak tunggal yang menguasai sistem. |
| **Kendali / Otoritas** | Dapat dikendalikan oleh satu organisasi/perusahaan tunggal (secara logis terpusat, secara fisik tersebar). | Dikelola secara kolektif oleh banyak pihak independen melalui mekanisme konsensus. |
| **Tujuan Desain** | Performa tinggi, skalabilitas kapasitas, dan keandalan layanan (*high availability*). | Otonomi, ketahanan terhadap sensor, kepercayaan tanpa perantara (*trustless*), dan tanpa *single point of failure*. |
| **Contoh Teknologi** | Google File System (GFS), MinIO Object Storage (sekarang di-*fork* menjadi **Silo**). | Blockchain (Bitcoin, Ethereum), jaringan P2P murni. |

---

## 8. Studi Kasus Penerapan Nyata

### A. Studi Kasus Sistem Terdistribusi
1. **Google File System (GFS) / HDFS (*Hadoop Distributed File System*):** Sistem penyimpanan file terdistribusi untuk menangani data berukuran sangat besar (*big data*) dengan memecah file ke banyak server.
2. **Apache Kafka dan Apache Pulsar:** Platform *event streaming* dan *message broker* terdistribusi untuk memproses aliran data *real-time* bervolume tinggi.
3. **Platform Streaming Video (Netflix, Twitch, YouTube):** Menggunakan jaringan server distribusi konten (CDN) di seluruh dunia agar video dapat diputar lancar tanpa *buffering* oleh jutaan penonton bersamaan.
4. **Media Sosial (Instagram, Threads, Facebook, TikTok):** Mengelola miliaran unggahan, interaksi, dan rekomendasi konten secara *real-time* menggunakan ribuan server terdistribusi.

### B. Studi Kasus Sistem Terdesentralisasi
1. **Blockchain (Bitcoin, Ethereum, Algorand, Sui, Near, Aptos, dll.):** Buku besar digital terdistribusi yang dicatat dan diverifikasi bersama oleh ribuan node di seluruh dunia tanpa otoritas bank sentral.
2. **DNS (*Domain Name System*):** Sistem penamaan domain internet yang hierarkis dan terdesentralisasi di berbagai server DNS global sehingga tidak bergantung pada satu komputer tunggal untuk menerjemahkan nama domain ke alamat IP.
3. **BitTorrent:** Protokol distribusi file secara P2P di mana potongan file diunduh dan diunggah langsung antar-pengguna.

---

## 9. CAP Theorem (*Brewer's Theorem*)
Diperkenalkan oleh **Eric Brewer pada tahun 2000**, *CAP Theorem* adalah prinsip fundamental dalam perancangan penyimpanan data terdistribusi (*distributed data store*).

### Tiga Properti Utama CAP
1. **C — *Consistency* (Konsistensi):** Setiap operasi pembacaan data (*read*) selalu mengembalikan hasil penulisan data terbaru (*most recent write*) atau mengembalikan pesan *error*. Semua node melihat data yang sama pada waktu yang bersamaan.
2. **A — *Availability* (Ketersediaan):** Setiap permintaan (*request*) yang dikirim ke node yang tidak rusak (*non-failing node*) pasti akan menerima respons berhasil (bukan *error*), meskipun data yang dikembalikan belum tentu merupakan versi terbaru.
3. **P — *Partition Tolerance* (Toleransi Partisi):** Sistem tetap dapat beroperasi dan berfungsi meskipun terjadi gangguan jaringan yang menyebabkan komunikasi antar-node terputus atau terfragmentasi (*network partition*).

### Trade-Off dalam CAP Theorem
* Pada sistem terdistribusi yang berjalan di atas jaringan nyata, gangguan jaringan (*network partition*) **pasti akan terjadi**. Oleh karena itu, properti **Partition Tolerance (P)** bersifat wajib.
* Akibatnya, sebuah *distributed data store* **tidak mungkin mendapatkan Consistency (C) dan Availability (A) secara bersamaan** saat terjadi partisi jaringan. Perancang sistem harus memilih salah satu *trade-off* selama partisi berlangsung (meskipun nantinya sistem dapat melakukan pemulihan otomatis / *auto-healing* setelah jaringan normal kembali):

| Pilihan Arsitektur | Properti yang Dipertahankan | Properti yang Dikorbankan | Cara Kerja Saat Terjadi *Network Partition* | Contoh Database |
| :--- | :--- | :--- | :--- | :--- |
| **AP** | *Availability* + *Partition Tolerance* | *Consistency* | Permintaan data tetap dilayani oleh node yang aktif, namun ada risiko data antar-node tidak sinkron sementara waktu. | **MongoDB** |
| **CP** | *Consistency* + *Partition Tolerance* | *Availability* | Sistem menolak permintaan (*read* maupun *write*) pada node yang terisolasi demi menjamin data tidak mengalami konflik/inkonsistensi. | **Apache Cassandra** |

---

## 10. Model Konsistensi Data
Dalam sistem terdistribusi, replikasi data ke banyak server memunculkan dua pendekatan utama konsistensi:
1. **Strong Consistency (Konsistensi Kuat):**
   * **Konsep:** Setelah data diperbarui, setiap pembacaan berikutnya dari node mana pun akan langsung menghasilkan nilai terbaru yang sama.
   * **Contoh Kasus:** **Sistem Perbankan / Keuangan.** Saldo rekening harus selalu akurat seketika agar tidak terjadi penarikan ganda (*double spending*) atau selisih saldo.
2. **Eventual Consistency (Konsistensi Akhir):**
   * **Konsep:** Jika tidak ada pembaruan baru dalam rentang waktu tertentu, seluruh replika data pada akhirnya akan sinkron dan memiliki nilai yang sama ("lama-lama konsisten").
   * **Contoh Kasus:** **Media Sosial.** Jika jumlah *like* atau komentar pada unggahan berbeda tipis selama beberapa detik antara pengguna di Indonesia dan pengguna di Eropa, hal tersebut tidak membahayakan sistem.

---

## 11. Tantangan dalam Sistem Terdistribusi dan Terdesentralisasi
1. **Latency dan Throughput:**
   * **Latency (Latensi):** Seberapa lama waktu yang dibutuhkan untuk memproses atau mengirim satu unit data/permintaan dari pengirim ke penerima.
   * **Throughput:** Seberapa banyak jumlah data atau transaksi yang dapat diproses oleh sistem dalam satu satuan waktu tertentu.
2. **Keamanan Siber:**
   * **DDoS (*Distributed Denial of Service*):** Membanjiri node/server dengan trafik palsu secara masif agar layanan lumpuh.
   * **Sybil Attack:** Jenis serangan siber di mana penyerang membuat banyak identitas palsu (akun, node, atau pseudonim—bahkan hingga ribuan) secara bersamaan untuk mendominasi pengaruh, suara, atau reputasi dalam jaringan. Serangan ini umum menargetkan sistem terdesentralisasi seperti P2P atau blockchain untuk memanipulasi konsensus, merusak integritas sistem, maupun mencuri informasi sensitif. *(Catatan historis: Istilah "Sybil" berasal dari nama tokoh perempuan dengan gangguan identitas disosiatif dalam buku berjudul Sybil terbitan tahun 1973).*
3. **Kompleksitas Manajemen Node:** Mengelola, memantau, memperbarui perangkat lunak, dan melakukan *debugging* pada ratusan hingga ribuan node yang tersebar sangat rumit.
4. **Governance (Tata Kelola) pada Sistem Terdesentralisasi:** Mengambil keputusan perubahan aturan atau pembaruan protokol pada sistem tanpa pemilik tunggal memerlukan mekanisme tata kelola yang adil dan sering kali memakan waktu lama.

---

## 12. Tren Masa Depan
Perkembangan teknologi terdistribusi dan terdesentralisasi terus bergerak ke arah berikut:
1. **Cloud-Native (Kubernetes, Docker):** Pemanfaatan kontainerisasi (*Docker*) dan orkestrasi otomatis (*Kubernetes*) untuk menjalankan aplikasi terdistribusi secara elastis di lingkungan *cloud*.
2. **Edge & Fog Computing:** Memindahkan proses komputasi dan penyimpanan data lebih dekat ke lokasi sumber data (perangkat IoT/pengguna akhir) untuk menekan latensi.
3. **Web3 & DeFi (*Decentralized Finance*):** Pengembangan ekosistem internet dan layanan keuangan berbasis blockchain yang berjalan tanpa perantara institusi terpusat.
4. **Federated Data & Federated Learning (AI Terdistribusi):** Melatih model kecerdasan buatan (AI) secara terdistribusi di banyak perangkat/server lokal tanpa perlu mengumpulkan data mentah pengguna ke satu server pusat, sehingga menjaga privasi data.

---

## 13. Ringkasan / Kesimpulan
* **Sistem terdistribusi** berfokus pada **pembagian beban kerja** di antara banyak komputer independen agar tampak seperti satu sistem tunggal yang cepat, skalabel, dan tahan gangguan.
* **Sistem terdesentralisasi** berfokus pada **pembagian kontrol dan wewenang** agar tidak ada satu pihak atau titik tunggal yang menguasai sistem.
* Keduanya merupakan konsep yang saling terkait erat namun berbeda fokus utama, serta menjadi fondasi wajib bagi infrastruktur teknologi informasi modern.