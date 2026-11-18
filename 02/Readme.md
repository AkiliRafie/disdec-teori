# Materi Minggu 2: Proses dan Komunikasi Antar Proses pada Sistem Terdistribusi

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
## 5. IPC (Inter-Process Communication) pada Sistem Terdistribusi
Terdapat 4 jenis mekanisme IPC secara umum:
1. **Shared Memory:** Tidak cocok untuk sistem terdistribusi karena proses berjalan pada node terpisah yang tidak memiliki akses memori fisik yang sama.
2. **Message Passing:** Proses berkomunikasi dengan mengirim dan menerima pesan melalui tautan komunikasi (*communication link*). 
3. **Remote Procedure Call (RPC):** Memungkinkan program memanggil prosedur di komputer lain seolah-olah berjalan secara lokal menggunakan bantuan komponen *Stub* di sisi klien dan server.
4. **Message-Oriented Middleware (MOM):** Infrastruktur perangkat lunak yang memfasilitasi pertukaran data asinkron melalui pesan mandiri (*message*) melalui perantara *messaging provider*.
   * **Standar dan Protokol MOM:** XATMI (X/Open group), AMQP (untuk aplikasi enterprise), HLA IEEE 1516 (simulasi), MQTT (untuk perangkat dengan sumber daya terbatas / IoT), DDS (OMG), dan XMPP.

DAN

### Lamport Logical Clock
* **Masalah Real Clock:** Mengandalkan *real clock timestamp* berisiko mengalami kekacauan urutan kejadian akibat latensi atau gangguan perangkat keras.
* **Aturan Lamport Logical Clock:**
  1. Setiap terjadi kejadian lokal, angka *logical clock* bertambah 1.
  2. Saat mengirim pesan, node menyertakan angka *logical clock*-nya.
  3. Saat menerima pesan, node membandingkan angka pada pesan dengan *logical clock* lokalnya, mengambil nilai terbesar, lalu menambahkan 1.
* **Contoh Ilustrasi Alur Logis:**
  * *Sales Service* mencatat transaksi (clock = 1) dan mengirim request ke *Product Service*.
  * *Product Service* menerima pesan (clock = 1), membandingkan dengan clock lokal (0), mengambil nilai 1, lalu menaikkan clock menjadi 2 sebelum meneruskan ke *Invoice Service*.
  * *Invoice Service* menerima pesan (clock = 2), memperbarui clock lokal menjadi 3, lalu menerbitkan faktur. Hal ini menjamin urutan kejadian tetap valid secara kausal tanpa bergantung pada jam fisik.
* **Pengembangan Lanjutan:** Model *scalar clock* oleh Leslie Lamport kemudian disempurnakan oleh peneliti lain menjadi *Vector clocks* (Fidge & Mattern, 1988), *Plausible clocks*, *Chain Clocks*, *Interval Tree Clocks*, hingga *Blum Clocks* (2019).


---
**MATERI HASIL AI (CHATGPT) DIBAWAH INI:**

**TERMASUK YANG SAYA TAMBAHKAN**
## 1. Tujuan Pembelajaran
Setelah mempelajari modul ini, mahasiswa diharapkan mampu:
1. Menjelaskan **konsep proses** pada sistem terdistribusi.
2. Menjelaskan berbagai **jenis komunikasi antar proses (IPC)**.
3. Menganalisis isu **sinkronisasi, reliabilitas, dan keamanan** pada sistem terdistribusi.
4. Mengaitkan konsep dengan contoh nyata seperti *microservices*, *cloud*, maupun *distributed messaging*.

---

## 2. Apa Itu Proses?
* **Definisi Program vs Proses:** *Program* adalah sekumpulan instruksi statis yang tersimpan di dalam disk (penyimpanan), sedangkan *Proses* adalah instansiasi aktif dari program yang dimuat ke dalam memori RAM dan sedang dieksekusi oleh CPU.
* **Abstraksi Proses & Thread:** Sistem operasi mengelola proses menggunakan *Process Control Block (PCB)*, register, dan *stack*. Di dalam sebuah proses, terdapat pembagian tugas ke dalam unit yang lebih kecil disebut *thread* (memiliki prioritas tinggi, sedang, atau rendah).
* **Scheduler (Penjadwal):** Bagian dari sistem operasi yang bertugas memilih thread untuk dieksekusi oleh CPU, melakukan *Context Switch*, serta menangani *preemption*.

### Manajemen Proses oleh Sistem Operasi
Sistem operasi mengelola proses melalui tiga aktivitas utama:
1. **Process Creation (Pembuatan Proses):** OS membuat proses baru, mengalokasikan sumber daya (memori, berkas), dan menginisialisasi lingkungan eksekusi (PCB, register, *stack*) hingga statusnya menjadi *Ready to Run*.
2. **Process Migration (Migrasi Proses):** OS dapat memindahkan proses yang sedang berjalan atau siap dari satu CPU (atau node) ke node lain untuk menyeimbangkan beban (*load balancing*), meningkatkan performa, atau menangani kegagalan.
3. **Process Monitoring (Pemantauan Proses):** OS melacak dan mengobservasi status proses, penggunaan sumber daya (CPU, memori), serta performa untuk memastikan eksekusi berjalan baik dan mengambil tindakan korektif jika diperlukan.

---

## 3. Sistem Terdistribusi dan Komunikasi
* **Karakteristik Komunikasi:** Sistem terdistribusi terdiri atas sekumpulan komputer/node independen yang bekerja sama dalam satu koordinasi logis. 
 Karena setiap node menjalankan prosesnya sendiri dan **tidak memiliki memori bersama (*shared memory*)** secara fisik, agar dapat bekerja sebagai satu kesatuan logis, node-node tersebut wajib saling berkomunikasi melalui jaringan.

### Jenis Komunikasi Antar-Node
* **Request:** Permintaan layanan atau data dari satu node ke node lain.
* **Response:** Hasil atau balasan data dari node yang menerima permintaan.
* **Publish:** Mengirimkan *event* atau pembaruan (*update*) ke dalam sistem.
* **Subscribe:** Menerima notifikasi dari topik atau layanan tertentu.

### Protokol & Peran Pendukung Komunikasi
* **Protokol Umum:** Menggunakan HTTP/HTTPS, gRPC, *Message Queue* (seperti Kafka, RabbitMQ), serta DNS / *Service Discovery*.
* **Fungsi Komunikasi:** Memfasilitasi pertukaran data, koordinasi (sinkronisasi dan konsistensi), keandalan sistem (*fault tolerance*), serta skalabilitas dinamis.
* **Contoh Penerapan Nyata:** Permainan *multiplayer*, aplikasi transportasi online (Gojek/Grab yang menghubungkan antarmuka pengguna dengan layanan *driver*, pelacakan lokasi, dan kalkulasi tarif), serta panggilan suara/video *point-to-point* pada aplikasi pesan instan seperti WhatsApp.

---

## 4. Model Komunikasi Pesan
* **Langsung (Direct):** Menggunakan *socket* jaringan. Contoh implementasi *socket* TCP berbasis Python pada modul:
  * **Server Socket:** Mengikat (*bind*) ke alamat IP *loopback* (`127.0.0.1`) dan port tertentu (`65432`), mendengarkan koneksi masuk (*listen*), menerima data, lalu mengirimkan kembali data tersebut ke klien (*echo back*).
  * **Client Socket:** Terhubung ke server pada host dan port yang sama, mengirim pesan biner (`b"Hello, world!"`), lalu menerima balasan data dari server.
* **Tidak Langsung (Indirect):** Menggunakan perantara seperti *mailboxes*, *message queues* (misalnya perintah *write* di Linux), serta pola **Publish/Subscriber (Pub/Sub)** di mana *Publisher* mengirim pesan ke sebuah *Topic* perantara yang kemudian disalurkan ke banyak *Subscriber*.

---

## 5. IPC (Inter-Process Communication) pada Sistem Terdistribusi
Terdapat 4 jenis mekanisme IPC secara umum:
1. **Shared Memory:** Tidak cocok untuk sistem terdistribusi karena proses berjalan pada node terpisah yang tidak memiliki akses memori fisik yang sama.
2. **Message Passing:** Proses berkomunikasi dengan mengirim dan menerima pesan melalui tautan komunikasi (*communication link*). 
3. **Remote Procedure Call (RPC):** Memungkinkan program memanggil prosedur di komputer lain seolah-olah berjalan secara lokal menggunakan bantuan komponen *Stub* di sisi klien dan server.
4. **Message-Oriented Middleware (MOM):** Infrastruktur perangkat lunak yang memfasilitasi pertukaran data asinkron melalui pesan mandiri (*message*) melalui perantara *messaging provider*.
   * **Standar dan Protokol MOM:** XATMI (X/Open group), AMQP (untuk aplikasi enterprise), HLA IEEE 1516 (simulasi), MQTT (untuk perangkat dengan sumber daya terbatas / IoT), DDS (OMG), dan XMPP.

---

## 6. Studi Kasus: Microservices
* **Konsep:** Arsitektur aplikasi dipecah menjadi layanan-layanan kecil independen (*Microservices*), di mana setiap layanan memiliki antarmuka pengguna (*User Interface*) atau logika tersendiri serta basis data masing-masing (*Database* terisolasi).
* Komunikasi antar-*microservice* dikoordinasikan melalui jaringan menggunakan API, *message broker*, atau gRPC.

---

## 7. Komunikasi Sinkron dan Asinkron
* **Synchronous (Sinkron):** Klien mengirim permintaan (*Make Request*) ke server, lalu masuk ke status menunggu (*Waiting for response*) sampai server selesai memproses dan mengembalikan respons.
* **Asynchronous (Asinkron):** Klien mengirim permintaan, langsung melanjutkan pekerjaan lain (*Continue Working*), dan nantinya akan menerima respons serta memprosesnya belakangan (*Get Response and do something*) saat data siap.

---

## 8. Berbagai Masalah pada Komunikasi Antar-Proses
1. **Reliability (Keandalan):** Memastikan pesan terkirim lengkap dan berurutan meski ada paket yang hilang atau *crash*. Solusinya meliputi penggunaan *acknowledgement*, mekanisme transmisi ulang (*retransmission*), dan protokol transport yang handal seperti TCP.
2. **Ordering (Pengurutan):** Memastikan pesan diproses secara logis dan terprediksi menggunakan *Pipes* (FIFO), *Message queues*, *Shared memory*, *Sockets*, *Message passing*, atau *RPC*.
3. **Security (Keamanan):** Melindungi data lewat enkripsi dan autentikasi.
4. **Performance (Performa):** Mengoptimalkan *latency* (waktu tunda) dan *bandwidth* (kapasitas data).

---

## 9. Protokol Komunikasi
* **Protokol Lapisan Transport/Dasar:** TCP, UDP, HTTP, serta protokol modern seperti **QUIC** (berjalan di atas UDP).
* **Protokol Lapisan Aplikasi (di atas HTTP):** REST, GraphQL, dan gRPC.

---

## 10. Konsep Sinkronisasi & Logical Clock
* **Sinkronisasi Jam (Clock Synchronization):** Setiap node memiliki jam lokal (*local clock*) sendiri yang rentan mengalami pergeseran (*skew*) akibat latensi jaringan dan perbedaan perangkat keras. 
* **Metode Sinkronisasi:** Sinkronisasi eksternal menggunakan jam atom (*atomic clock*) menghasilkan *International Atomic Time* (TAI), GPS, dan *Universal Coordinated Time* (UTC), serta protokol jaringan seperti **NTP (Network Time Protocol)** atau **PTP (Precision Time Protocol)** yang jauh lebih presisi.

### Lamport Logical Clock
* **Masalah Real Clock:** Mengandalkan *real clock timestamp* berisiko mengalami kekacauan urutan kejadian akibat latensi atau gangguan perangkat keras.
* **Aturan Lamport Logical Clock:**
  1. Setiap terjadi kejadian lokal, angka *logical clock* bertambah 1.
  2. Saat mengirim pesan, node menyertakan angka *logical clock*-nya.
  3. Saat menerima pesan, node membandingkan angka pada pesan dengan *logical clock* lokalnya, mengambil nilai terbesar, lalu menambahkan 1.
* **Contoh Ilustrasi Alur Logis:**
  * *Sales Service* mencatat transaksi (clock = 1) dan mengirim request ke *Product Service*.
  * *Product Service* menerima pesan (clock = 1), membandingkan dengan clock lokal (0), mengambil nilai 1, lalu menaikkan clock menjadi 2 sebelum meneruskan ke *Invoice Service*.
  * *Invoice Service* menerima pesan (clock = 2), memperbarui clock lokal menjadi 3, lalu menerbitkan faktur. Hal ini menjamin urutan kejadian tetap valid secara kausal tanpa bergantung pada jam fisik.
* **Pengembangan Lanjutan:** Model *scalar clock* oleh Leslie Lamport kemudian disempurnakan oleh peneliti lain menjadi *Vector clocks* (Fidge & Mattern, 1988), *Plausible clocks*, *Chain Clocks*, *Interval Tree Clocks*, hingga *Blum Clocks* (2019).

---

## 11. Koordinasi Proses
* **Definisi:** Mekanisme yang memungkinkan proses-proses independen di berbagai komputer untuk bekerja sama secara harmonis, mengelola komunikasi, sinkronisasi, serta mencegah konflik seperti *deadlock* atau *race condition*.
* **Contoh Teknologi Koordinasi:** Apache ZooKeeper, etcd, Consul, ClickHouse Keeper, Oxia, dan KRaft.

---

## 12. Tantangan, Solusi, dan Current Issues
* **Message Loss:** Diatasi dengan *retransmission* dan *acknowledgement*.
* **Deadlock:** Diatasi dengan *timeout* dan algoritma deteksi khusus.
* **Data Inconsistency:** Diatasi menggunakan protokol konsensus seperti Raft atau Paxos.
* **Security:** Menggunakan SSL/TLS dan OAuth2.
* **Current Issues (Tren Saat Ini):** Penerapan *Edge and Fog Computing*, *Observability and tracing* (telemetri), serta orkestrasi kontainer seperti Kubernetes dan Docker Swarm.