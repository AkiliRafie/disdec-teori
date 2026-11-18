# Materi Minggu 3: Sinkronisasi dan Koordinasi pada Sistem Terdistribusi

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
### Sumber *Atomic Clock* dan Arsitektur GPS Time Server
Waktu paling akurat berasal dari jam atom (*atomic clock*) yang dikelola di stasiun darat (seperti *U.S. Naval Observatory* dan *Schriever Space Force Base*) serta dibawa oleh sekitar 30–31 satelit GPS di orbit bumi (*Medium Earth Orbit*), misi *Deep Space Atomic Clock* (NASA), dan *Atomic Clock Ensemble in Space* (ACES) di ISS.

**Alur Sinkronisasi Waktu dari Satelit ke Klien:**
```text
Master Atomic Clock <-- Satelit GPS <-- USB GPS Dongle (/dev/ttyACM0) <-- GPSD <-- Shared Memory (RAM) <-- NTP/PTP Server <-- Client OS
```

DAN

### Pendekatan Solusi *Mutual Exclusion*

| Algoritma | Mekanisme Kerja | Kelebihan | Kekurangan |
| :--- | :--- | :--- | :--- |
| **1. Centralized Algorithm** | Satu *node* ditunjuk sebagai koordinator. Proses meminta izin ke koordinator; jika CS kosong, izin diberikan. Setelah selesai, proses melapor ke koordinator untuk melepas kunci. | Sederhana, mudah diimplementasikan, biaya pesan rendah (hanya 3 pesan per CS). | *Single Point of Failure* (jika koordinator mati, sistem macet) dan berpotensi menjadi *bottleneck*. |
| **2. Ricart–Agrawala Algorithm** | Terdistribusi penuh tanpa koordinator. Proses yang ingin masuk CS menyiarkan pesan *request* berstempel waktu ke semua *node*, dan baru masuk CS setelah mendapat balasan `OK` dari seluruh $N-1$ *node* lainnya. | Tahan terhadap kegagalan satu titik (*fault-tolerant*), tidak ada *bottleneck* terpusat. | *Overhead* lalu lintas jaringan tinggi: membutuhkan $2 \times (N - 1)$ pesan setiap kali masuk CS ($N-1$ pesan minta + $N-1$ pesan balasan). |
| **3. Token-Based Algorithm** | Sebuah token unik beredar antar-proses (biasanya dalam topologi cincin logis). Hanya proses yang sedang memegang token yang berhak masuk ke CS. | Efisien (hanya 1 pesan per masuk CS) dan bebas *starvation*. | Sulit mendeteksi jika token hilang akibat *node* mati mendadak, sehingga butuh mekanisme regenerasi token. |


---
**MATERI HASIL AI (GEMINI) DIBAWAH INI:**

**TERMASUK YANG SAYA TAMBAHKAN**

## 1. Tujuan Pembelajaran & Agenda Materi

### Tujuan Pembelajaran
Setelah mempelajari modul ini, mahasiswa diharapkan mampu:
1. Memahami dan menjelaskan alasan diperlukannya sinkronisasi untuk koordinasi pada sistem terdistribusi.
2. Menjelaskan berbagai tantangan sinkronisasi dalam sistem terdistribusi.
3. Memahami konsep sinkronisasi waktu fisik beserta algoritma dan protokolnya.
4. Memahami konsep sinkronisasi waktu logis beserta algoritma terkait (*Vector Clocks*).
5. Menjelaskan masalah *mutual exclusion* dan berbagai algoritma solusinya.
6. Menjelaskan masalah pemilihan koordinator (*leader election*) serta algoritma pemecahannya.
7. Memberikan contoh studi kasus nyata di industri terkait sinkronisasi dan koordinasi.

### Agenda Pembelajaran
* Pengertian dan Pentingnya Sinkronisasi (*Data Race*, *Race Condition*, *Deadlock*)
* Tantangan dari Sinkronisasi
* Sinkronisasi Waktu Fisik (UTC, Atomic Clock, GPS, NTP/Chrony, PTP/LinuxPTP)
* Sinkronisasi Waktu Logis (*Happens-Before* dan *Vector Clocks*)
* Tantangan *Mutual Exclusion* dan Algoritma Solusinya
* Pemilihan Koordinator (*Coordinator Election*)

---

## 2. Konsep Sinkronisasi dan Permasalahan Konkurensi

### Pengertian Sinkronisasi
* **Sinkronisasi** adalah proses koordinasi proses, data, dan sumber daya di berbagai *node* dalam jaringan untuk memastikan operasi berjalan konsisten, koheren, dan andal. Mekanisme sinkronisasi yang efektif mencegah konflik data akibat eksekusi konkuren (bersamaan), seperti *data race*, *race condition*, dan *deadlock*.

### Mengapa Sinkronisasi Penting?
1. **Integritas Data:** Mencegah terjadinya *data race* maupun *race condition*.
2. **Sinkronisasi State:** Memastikan kondisi aplikasi sama di semua *node* (contoh: posisi pemain dan inventaris pada *online multiplayer game*, atau keranjang belanja *e-commerce* lintas perangkat).
3. **Koordinasi Tugas & Pengelolaan Sumber Daya:** Mencegah operasi redundan dan mendukung *load balancing*.
4. **Mekanisme Pemulihan (*Recovery*):** Memelihara *state* yang valid untuk pemulihan saat terjadi kegagalan.
5. **Mencegah *Deadlock* & Mendukung Skalabilitas:** Menjaga kelancaran eksekusi saat sistem diperbesar.

### Studi Kasus Masalah: *Data Race* atau *Race Condition*

#### 1. Ilustrasi E-Commerce (Stok Produk)
* Produk X memiliki stok awal $10$. User A dan User B membeli 1 produk X secara bersamaan.
* Keduanya membaca stok $10$ sebelum salah satunya selesai mengurangi stok, lalu masing-masing menulis sisa stok $10 - 1 = 9$.
* **Realitas:** Terjadi 2 pembelian (stok seharusnya $8$), tetapi sistem mencatat stok $9$.

#### 2. Ilustrasi *Data Race* (Penambahan Variabel $x = 10$)
* *Thread 1* dan *Thread 2* sama-sama membaca nilai $x = 10$ dari memori.
* Keduanya menghitung $10 + 1 = 11$, lalu menulis $x = 11$ kembali ke memori.
* **Hasil Akhir:** $x = 11$ (seharusnya $x = 12$, satu operasi penambahan hilang).

#### 3. Ilustrasi *Race Condition* (Penarikan Saldo Bank = $\$100$)
* *Thread A* dan *Thread B* masing-masing ingin menarik uang sebesar $\$80$.
* Keduanya mengecek saldo secara bersamaan ($\$100 \ge \$80 \rightarrow \text{YA}$).
* *Thread A* mengurangi $\$80$ (sisa $\$20$), kemudian *Thread B* juga mengurangi $\$80$ (sisa $-\$60$).
* **Hasil Akhir:** Saldo menjadi $-\$60$ (*double spending*).

#### Tabel Perbandingan *Data Race* vs *Race Condition*
| Parameter | Data Race | Race Condition |
| :--- | :--- | :--- |
| **Fokus Utama** | Akses memori secara bersamaan (*concurrent memory access*). | Urutan dan waktu eksekusi instruksi (*timing & execution sequence*). |
| **Level** | *Low-level* (instruksi CPU / akses memori fisik). | *High-level* (alur logika aplikasi / *business logic*). |
| **Syarat Terjadi** | 1. Minimal 2 *thread* mengakses memori yang sama.<br>2. Minimal 1 akses berupa operasi tulis (*write*).<br>3. Tidak ada mekanisme penguncian (*mutex/lock*). | Kebenaran hasil program bergantung pada urutan eksekusi yang tidak dapat diprediksi. |
| **Dampak Utama** | *Undefined behavior*, korupsi memori, data tidak konsisten. | Output logika salah, *bug* bisnis (misalnya saldo minus, transaksi ganda). |
| **Solusi** | Menggunakan *Mutex / Lock* atau tipe data *Atomic*. | Desain ulang alur kontrol (misalnya *Atomic Check-and-Act*, *Database Transaction Lock*). |

### Analisis Kode Program *Multithreading* (`multithreads-example.py`)
* **Fungsi & Cara Kerja:** Program membuat 5 *thread* (`A` hingga `E`) yang masing-masing menjalankan fungsi `task()` dengan jeda tidur selama 5 detik (`time.sleep(5)`). Metode `.start()` memulai eksekusi secara paralel/konkuren, sedangkan `.join()` menahan *thread* utama hingga kelima *thread* selesai.
* **Output & Analisis:** Pada eksekusi pertama, urutan selesainya adalah `A, B, D, E, C`, sedangkan pada eksekusi kedua urutannya menjadi `A, B, E, C, D`. Hal ini membuktikan bahwa dalam eksekusi konkuren tanpa sinkronisasi eksplisit, urutan penyelesaian tugas bersifat **non-deterministik** (berubah-ubah tergantung penjadwalan sistem operasi).

### *Deadlock* dan Tantangan Sinkronisasi
* **Deadlock:** Kondisi kebuntuan di mana sekumpulan proses saling menunggu sumber daya yang sedang dipegang oleh proses lain dalam siklus tertutup (contoh: *Process 1* memegang `R1` dan `R2` sambil menunggu `R3`, sementara `R3` dipegang *Process 2* yang menunggu `R2`).
* **Enam Tantangan Utama Sinkronisasi:**
  1. *Network latency* (keterlambatan jaringan) dan *partitioning* (terputusnya jaringan).
  2. Skalabilitas sistem saat jumlah *node* bertambah besar.
  3. Membangun *fault-tolerant system* (tahan terhadap kegagalan komponen).
  4. *Concurrency control* (pengendalian akses bersamaan).
  5. *Data consistency* (menjaga kesamaan data lintas *node*).
  6. Sinkronisasi waktu antar-mesin.

---

## 3. Sinkronisasi Waktu Fisik

### Pengertian dan Standar Waktu Dunia
* **Sinkronisasi Waktu Fisik** adalah proses menyelaraskan *real-time clock* pada seluruh *node* dalam sistem terdistribusi dengan standar waktu global untuk keperluan korelasi *log*, penjadwalan *event*, dan konsistensi transaksi.
* **Standar Waktu:**
  * **GMT (*Greenwich Mean Time*):** Standar lama yang sudah tidak lagi digunakan sebagai acuan teknis utama.
  * **UTC (*Coordinated Universal Time*):** Standar waktu internasional yang ditetapkan oleh ITU/IAU sejak tahun 1963/1967 (singkatan kompromi netral bahasa antara *CUT* dalam bahasa Inggris dan *TUC* dalam bahasa Prancis).
  * UTC didasarkan pada **TAI** (*Temps Atomique International* / *International Atomic Time*).
  * **LTC (*Coordinated Lunar Time*):** Standar waktu untuk misi bulan yang sedang dalam tahap pengembangan.

### Sumber *Atomic Clock* dan Arsitektur GPS Time Server
Waktu paling akurat berasal dari jam atom (*atomic clock*) yang dikelola di stasiun darat (seperti *U.S. Naval Observatory* dan *Schriever Space Force Base*) serta dibawa oleh sekitar 30–31 satelit GPS di orbit bumi (*Medium Earth Orbit*), misi *Deep Space Atomic Clock* (NASA), dan *Atomic Clock Ensemble in Space* (ACES) di ISS.

**Alur Sinkronisasi Waktu dari Satelit ke Klien:**
```text
Master Atomic Clock <-- Satelit GPS <-- USB GPS Dongle (/dev/ttyACM0) <-- GPSD <-- Shared Memory (RAM) <-- NTP/PTP Server <-- Client OS
```

### Protokol Sinkronisasi Waktu Fisik

#### 1. NTP (*Network Time Protocol* - RFC 5905)
* Dikembangkan oleh David L. Mills dkk. (1985), bekerja pada *application layer* dengan struktur hierarki bertingkat yang disebut **Stratum**:
  * **Stratum 0:** Sumber referensi waktu presisi tinggi (*Atomic Clock*, GPS).
  * **Stratum 1:** Server yang terhubung langsung ke perangkat Stratum 0.
  * **Stratum 2, 3, dst.:** Server dan klien di bawahnya yang menyinkronkan waktu secara berjenjang.
* **Akurasi:** Beberapa milidetik melalui Internet, dan puluhan mikrodetik pada jaringan LAN.
* **Implementasi di Linux (`chrony`):**
  * Terdiri dari daemon latar belakang **`chronyd`** dan utilitas baris perintah **`chronyc`**.
  * Dirancang tangguh menghadapi koneksi jaringan terputus-putus, perubahan suhu (yang memengaruhi osilator kristal jam komputer), dan mesin virtual.
  * Perintah pemantauan: `chronyc -n sources` (melihat daftar server sumber waktu dan nilai *stratum*) serta `chronyc -n tracking` (melihat selisih waktu sistem terhadap waktu NTP/UTC).

#### 2. PTP (*Precision Time Protocol* - IEEE 1588)
* Standar IEEE 1588 (versi 2002, 2008/PTPv2, dan 2019; serta adaptasi **gPTP / IEEE 802.1AS** untuk *Audio Video Bridging* dan *Time-Sensitive Networking*).
* Mampu mencapai presisi hingga tingkat **sub-mikrodetik / nanodetik** karena menggunakan pencatatan waktu langsung di tingkat perangkat keras kartu jaringan (*hardware timestamping*).
* **Implementasi di Linux (`linuxptp`):**
  * **`ptp4l`:** Mengimplementasikan PTP *Boundary/Ordinary/Transparent Clock* pada antarmuka jaringan (mendukung mekanisme *End-to-End* `-E` dan *Peer-to-Peer* `-P`).
  * **`phc2sys`:** Menyinkronkan jam sistem operasi (`CLOCK_REALTIME`) dengan *PTP Hardware Clock* (PHC) pada kartu jaringan.
  * **Syarat Perangkat Keras:** Kartu jaringan (NIC) wajib mendukung *hardware timestamping*, yang dapat diperiksa menggunakan perintah `sudo ethtool -T eth0`.

---

## 4. Sinkronisasi Waktu Logis (*Logical Clocks*)

### Konsep Dasar
* **Sinkronisasi Waktu Logis**  tidak bergantung pada jam dinding fisik, melainkan bertujuan menetapkan **urutan sebab-akibat kejadian (*causal order of events*)**.
* **Sifat Transitivitas (*Happens-Before*):** Jika kejadian $A$ terjadi sebelum $B$ ($A \rightarrow B$) dan $B$ terjadi sebelum $C$ ($B \rightarrow C$), maka $A$ pasti terjadi sebelum $C$ ($A \rightarrow C$).

### *Vector Clocks*
* Setiap proses dalam sistem yang terdiri dari $N$ proses memelihara sebuah vektor (*array*) berukuran $N$ berisi bilangan bulat yang diinisialisasi dengan nilai `0` (contoh untuk 3 proses $A, B, C$: `[0, 0, 0]`, di mana indeks `0` milik $A$, indeks `1` milik $B$, dan indeks `2` milik $C$).

#### Aturan Pembaruan *Vector Clock*:
1. **Kejadian Internal (*Internal Event*):** Proses menambahkan `1` pada elemen miliknya sendiri di dalam vektornya ($VC_i[i] = VC_i[i] + 1$).
2. **Mengirim Pesan (*Send Message*):** Proses menambahkan `1` pada elemen miliknya sendiri terlebih dahulu, lalu melampirkan vektor tersebut bersama pesan yang dikirim.
3. **Menerima Pesan (*Receive Message*):** Proses mengambil nilai maksimum elemen demi elemen (*element-wise max*) antara vektor lokalnya dan vektor yang diterima dari pesan, lalu menambahkan `1` pada elemen miliknya sendiri.

#### Penelusuran Contoh Kasus pada Modul (Proses A, B, C):
1. **Kondisi Awal:** $A = [0, 0, 0]$, $B = [0, 0, 0]$, $C = [0, 0, 0]$.
2. **A melakukan *internal event*:** $A$ menambah indeksnya sendiri menjadi $[1, 0, 0]$.
3. **A mengirim pesan ke B:** $A$ menambah indeksnya menjadi $[2, 0, 0]$ dan mengirimkannya ke $B$.
4. **B menerima pesan dari A:** $B$ menghitung $\max([2, 0, 0], [0, 0, 0]) = [2, 0, 0]$ (sebelum menambah indeksnya untuk aksi berikutnya).
5. **B mengirim pesan ke C:** $B$ menambah indeksnya sendiri menjadi $[2, 1, 0]$ dan mengirimkannya ke $C$.
6. **C menerima pesan dari B & bereaksi:** $C$ menghitung $\max([2, 1, 0], [0, 0, 0]) = [2, 1, 0]$, lalu menambah indeksnya sendiri menjadi $[2, 1, 1]$.
7. **C melakukan *internal event* lagi:** $C$ menambah indeksnya menjadi $[2, 1, 2]$.
* **Posisi Akhir:** $A = [2, 0, 0]$, $B = [2, 1, 0]$, $C = [2, 1, 2]$.


---

## 5. *Mutual Exclusion*

### Pengertian dan Prinsip Dasar
* **Mutual Exclusion (Mutex)** adalah mekanisme yang menjamin hanya **satu proses** yang dapat mengakses sumber daya bersama (*shared resource*, seperti *printer*, file, atau tabel database) dalam satu waktu guna mencegah kerusakan atau inkonsistensi data.
* **Critical Section (CS):** Bagian dari kode program yang mengakses sumber daya bersama. Tujuan utama mutex adalah memastikan tidak ada eksekusi CS yang saling tumpang tindih (*overlap*).

**Empat Prinsip Utama Mutual Exclusion:**
1. **Mutual Exclusion:** Maksimal hanya 1 proses yang berada di dalam *Critical Section* pada satu waktu.
2. **Progress:** Jika tidak ada proses yang sedang menggunakan CS, proses yang membutuhkan akses harus dapat segera masuk.
3. **Bounded Waiting:** Tidak boleh ada proses yang menunggu selamanya (*starvation*).
4. **Fairness:** Permintaan akses dilayani sesuai urutan yang adil dan masuk akal.

### Pendekatan Solusi *Mutual Exclusion*

| Algoritma | Mekanisme Kerja | Kelebihan | Kekurangan |
| :--- | :--- | :--- | :--- |
| **1. Centralized Algorithm** | Satu *node* ditunjuk sebagai koordinator. Proses meminta izin ke koordinator; jika CS kosong, izin diberikan. Setelah selesai, proses melapor ke koordinator untuk melepas kunci. | Sederhana, mudah diimplementasikan, biaya pesan rendah (hanya 3 pesan per CS). | *Single Point of Failure* (jika koordinator mati, sistem macet) dan berpotensi menjadi *bottleneck*. |
| **2. Ricart–Agrawala Algorithm** | Terdistribusi penuh tanpa koordinator. Proses yang ingin masuk CS menyiarkan pesan *request* berstempel waktu ke semua *node*, dan baru masuk CS setelah mendapat balasan `OK` dari seluruh $N-1$ *node* lainnya. | Tahan terhadap kegagalan satu titik (*fault-tolerant*), tidak ada *bottleneck* terpusat. | *Overhead* lalu lintas jaringan tinggi: membutuhkan $2 \times (N - 1)$ pesan setiap kali masuk CS ($N-1$ pesan minta + $N-1$ pesan balasan). |
| **3. Token-Based Algorithm** | Sebuah token unik beredar antar-proses (biasanya dalam topologi cincin logis). Hanya proses yang sedang memegang token yang berhak masuk ke CS. | Efisien (hanya 1 pesan per masuk CS) dan bebas *starvation*. | Sulit mendeteksi jika token hilang akibat *node* mati mendadak, sehingga butuh mekanisme regenerasi token. |

---

## 6. Pemilihan Koordinator (*Coordinator / Leader Election*)

### Mengapa Pemilihan Koordinator Diperlukan?
Dalam sistem terdistribusi, sering kali dibutuhkan satu *node* pemimpin (*leader* / koordinator) untuk mengelola sumber daya bersama, mengoordinasikan sinkronisasi, atau mengambil keputusan global. Apabila koordinator saat ini mengalami kegagalan (*crash*) atau ada *node* baru yang bergabung, sistem harus mampu memilih koordinator pengganti secara otomatis.

### Algoritma Pemilihan Koordinator & Contoh Industri
1. **Bully Algorithm:** *Node* yang mendeteksi koordinator mati akan mengadakan pemilihan dengan menghubungi *node*-*node* yang memiliki nomor ID lebih besar darinya. *Node* aktif dengan **ID tertinggi** akan memenangkan pemilihan ("mengintimidasi" yang lebih kecil) dan mengumumkan dirinya sebagai koordinator baru.
2. **Ring Algorithm:** *Node*-*node* disusun dalam cincin logis. Pesan pemilihan (*election message*) diedarkan mengelilingi cincin sambil mengumpulkan daftar ID *node* yang aktif. Setelah kembali ke pengirim awal, *node* dengan **ID tertinggi** dalam daftar ditetapkan sebagai koordinator.
3. **Algoritma Berbasis Konsensus (Raft & Paxos):** Pemilihan pemimpin dilakukan melalui pemungutan suara mayoritas (*quorum*) dengan jaminan konsistensi log yang kuat.
   * **Contoh Implementasi Nyata di Industri:**
     * **Apache ZooKeeper:** Menggunakan protokol **Zab** (*ZooKeeper Atomic Broadcast*, yang cara kerjanya mirip dengan Paxos).
     * **Kubernetes:** Menggunakan algoritma **Raft** melalui penyimpanan metadata terdistribusi **`etcd`**.

---

## 7. Kesimpulan

* **Sinkronisasi** sangat krusial dalam sistem terdistribusi untuk mencegah anomali konkurensi seperti *data race*, *race condition*, dan *deadlock*, serta menjaga integritas *state* di seluruh *node*.
* **Sinkronisasi Waktu Fisik** menyelaraskan jam perangkat keras dengan standar **UTC** (berbasis *Atomic Clock* dan GPS) menggunakan protokol **NTP (`chrony`)** untuk akurasi milidetik/mikrodetik atau **PTP (`linuxptp`)** dengan *hardware timestamping* untuk akurasi nanodetik; sementara **Sinkronisasi Waktu Logis (*Vector Clocks*)** digunakan ketika sistem lebih membutuhkan urutan sebab-akibat kejadian (*causal ordering*) daripada waktu jam dinding.
* Untuk mengatur akses eksklusif terhadap sumber daya bersama (*Mutual Exclusion*) dan menentukan pemimpin klaster (*Coordinator Election*), sistem terdistribusi modern beralih dari pendekatan terpusat ke algoritma terdistribusi dan konsensus seperti **Ricart–Agrawala**, **Bully/Ring**, **Zab (ZooKeeper)**, dan **Raft (etcd/Kubernetes)**.
