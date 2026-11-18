# Materi Minggu 4: Konsistensi dan Replikasi pada Sistem Terdistribusi

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
### Penjelasan Masing-Masing Model Konsistensi:

1. **Strong Consistency (Konsistensi Kuat):**
   * Setelah data diperbarui, semua *node* langsung melihat versi terbaru. Setiap pembacaan (*read*) setelah penulisan (*write*) pasti mendapatkan nilai terbaru yang sama.
   * Umumnya menggunakan protokol konsensus seperti *Two-Phase Commit* atau *Paxos*.
   * **Contoh:** Sistem perbankan (saat saldo berubah, semua server wajib langsung mengetahui nilai terbarunya).

2. **Linearizability (Linearizabilitas):**
   * Varian paling kuat dari *strong consistency*. Operasi tampak dieksekusi secara **atomik** dan langsung (*real-time*), dengan urutan global yang sama bagi seluruh *node*. Sistem terdistribusi tampak seolah-olah hanya berupa satu *node* tunggal.
   * **Implementasi:** `etcd`, Apache ZooKeeper, Google Spanner.
   * **Contoh:** Pengecekan keunikan ID atau *username* agar dua pengguna tidak bisa mendaftarkan nama yang sama secara bersamaan.

3. **Sequential Consistency (Konsistensi Sekuensial):**
   * Semua operasi tampak dieksekusi dalam urutan yang sama oleh semua *node*, meskipun tidak harus terjadi secara *real-time* (boleh ada sedikit jeda waktu asalkan urutannya seragam).
   * **Contoh:** Semua pengguna media sosial melihat urutan *posting* yang sama, meskipun kemunculannya mengalami sedikit *delay*.

4. **Causal Consistency (Konsistensi Kausal):**
   * Jika suatu operasi memiliki hubungan sebab-akibat dengan operasi lain, urutan tersebut wajib dijaga. Namun, operasi yang tidak saling berhubungan boleh dilihat dalam urutan berbeda oleh *node* lain.
   * **Contoh:** Di media sosial, komentar atau reaksi terhadap sebuah *posting* tidak boleh muncul sebelum *posting* aslinya tampil.

5. **Read-Your-Writes Consistency:**
   * Setelah seorang pengguna menulis/mengubah data, pembacaan berikutnya oleh **pengguna tersebut** dijamin akan melihat hasil tulisannya sendiri (meskipun pengguna lain belum tentu langsung melihatnya).
   * **Contoh:** Saat mengirim pesan di aplikasi *chatting*, pesan langsung tampil di layar pengirim walaupun penerima belum menerimanya.

6. **Monotonic Read Consistency:**
   * Jika seorang pengguna membaca data secara berulang kali, pengguna tersebut dijamin tidak akan pernah melihat versi data yang lebih lama daripada versi yang sudah pernah dilihatnya sebelumnya.
   * **Contoh:** *Timeline* media sosial tidak akan "mundur" kembali ke unggahan lama ketika halaman di-*refresh*.

7. **Eventual Consistency (Konsistensi Akhir):**
   * Tidak menjamin data langsung sama di semua *node* seketika, tetapi menjamin bahwa semua replika pada akhirnya akan memiliki data yang sama apabila tidak ada pembaruan baru dalam jangka waktu tertentu.
   * Dikenal dengan konsep **BASE**:
     * **Basically Available:** Sistem menjamin ketersediaan akses baca/tulis meskipun terjadi kegagalan sebagian.
     * **Soft State:** Status/keadaan replika tidak harus selalu konsisten setiap detik dan dapat berubah seiring waktu tanpa input baru.
     * **Eventual Consistency:** Data pada akhirnya akan konsisten di seluruh *node*.
   * **Contoh:** Amazon DynamoDB, Apache Cassandra.
---
**MATERI HASIL AI (CHATGPT) DIBAWAH INI:**

**TERMASUK YANG SAYA TAMBAHKAN**


## 1. Tujuan Pembelajaran & Agenda Materi

### Tujuan Pembelajaran
Setelah mempelajari modul ini, mahasiswa diharapkan mampu:
1. Memahami dan menjelaskan konsep konsistensi dalam sistem terdistribusi.
2. Menjelaskan berbagai model konsistensi beserta karakteristiknya.
3. Memahami dan menjelaskan konsep replikasi data.
4. Mengetahui dan menjelaskan berbagai protokol serta strategi replikasi.
5. Melakukan analisis studi kasus replikasi pada sistem nyata yang banyak digunakan di industri.

### Agenda Pembelajaran
* Pengertian dan Pentingnya Konsistensi pada Sistem Terdistribusi
* Teorema CAP (*CAP Theorem*)
* Model-Model Konsistensi (*Consistency Models*)
* *Trade-off* antara Latensi dan Konsistensi
* Pengertian, Tujuan, dan Jenis Replikasi Data
* Strategi Sinkronisasi Replika dan Penanganan Konflik
* Protokol Replikasi
* Studi Kasus (PostgreSQL 18 & Apache Ignite 3) dan Bahan Diskusi

---

## 2. Konsep Konsistensi pada Sistem Terdistribusi

### Apa itu Konsistensi?
* **Konsistensi** menjamin bahwa semua *node* (komputer/server) dalam sistem terdistribusi memiliki data yang sama atau tampak sama pada waktu tertentu, meskipun data tersebut tersebar di banyak lokasi fisik dan dapat diubah secara bersamaan oleh pengguna atau proses yang berbeda.
* **Ilustrasi:** Jika sebuah aplikasi memiliki beberapa salinan data di beberapa server dan terjadi perubahan data di satu server, sistem harus memastikan bahwa server-server lainnya pada akhirnya memiliki versi data yang sama.

### Mengapa Konsistensi Penting?
* Karena data disimpan di banyak *node*, selalu ada potensi ketidaksamaan data antar-node. Konsistensi bertujuan agar pengguna selalu melihat data yang benar dan mutakhir (*up-to-date*).
* **Contoh Kasus (Transfer Perbankan):**
  * Seorang nasabah mentransfer uang dari rekening A ke rekening B.
  * Server 1 sudah mengurangi saldo rekening A, tetapi Server 2 belum menambahkan saldo ke rekening B.
  * Akibatnya, ketika nasabah ingin melakukan transaksi menggunakan rekening B melalui Server 2, transaksi gagal karena saldo dianggap belum mencukupi. Kondisi ini menunjukkan bahwa sistem **tidak konsisten**.

---

## 3. Teorema CAP (*CAP Theorem*)

Teorema CAP menjelaskan kompromi (*trade-off*) mendasar dalam perancangan sistem terdistribusi yang terdiri dari tiga aspek utama:

1. **Consistency (C):** Data bersifat konsisten; semua *node* melihat data yang sama pada saat yang bersamaan.
2. **Availability (A):** Ketersediaan layanan; setiap permintaan (*request*) selalu mendapatkan respons (bukan pesan gagal/error) saat pengguna memerlukan data.
3. **Partition Tolerance (P):** Toleransi partisi; sistem tetap dapat berjalan dan beroperasi meskipun terjadi gangguan atau putusnya koneksi jaringan antar-*node* (*network partition*).

### Realita Teorema CAP dan Contoh Kasus
* Dalam jaringan nyata, putusnya koneksi (*partition*) adalah hal yang wajar dan pasti terjadi. Oleh karena itu, kondisi ideal yang memenuhi ketiga syarat (**CAP**) secara bersamaan **tidak mungkin terjadi**; sistem harus memilih antara **AP** atau **CP** saat partisi jaringan terjadi.
* **Contoh Kasus (Aplikasi E-Commerce):**
  * **Server A** melayani wilayah Indonesia Barat, dan **Server B** melayani wilayah Indonesia Timur. Tiba-tiba koneksi jaringan antara kedua server terputus (*partition*).
  * **Jika memilih Availability (AP):** Pengguna di kedua wilayah tetap dapat berbelanja dengan lancar, tetapi informasi stok barang antar-wilayah bisa sementara tidak sinkron (risiko menjual barang yang stoknya sebenarnya habis).
  * **Jika memilih Consistency (CP):** Sistem menolak transaksi sementara waktu hingga jaringan pulih demi menjaga keakuratan stok barang, sehingga mengorbankan *Availability*.

---

## 4. Model Konsistensi (*Consistency Models*)

**Model Konsistensi** adalah aturan atau jaminan mengenai bagaimana dan kapan perubahan data pada sistem terdistribusi akan terlihat oleh *node* atau klien lain.

**Spektrum Model Konsistensi (dari yang paling kuat ke paling lemah):**
```text
Strong ──▶ Linearizable ──▶ Sequential ──▶ Causal ──▶ Read-your-writes ──▶ Monotonic read ──▶ Eventual ──▶ Weak
```

### Penjelasan Masing-Masing Model Konsistensi:

1. **Strong Consistency (Konsistensi Kuat):**
   * Setelah data diperbarui, semua *node* langsung melihat versi terbaru. Setiap pembacaan (*read*) setelah penulisan (*write*) pasti mendapatkan nilai terbaru yang sama.
   * Umumnya menggunakan protokol konsensus seperti *Two-Phase Commit* atau *Paxos*.
   * **Contoh:** Sistem perbankan (saat saldo berubah, semua server wajib langsung mengetahui nilai terbarunya).

2. **Linearizability (Linearizabilitas):**
   * Varian paling kuat dari *strong consistency*. Operasi tampak dieksekusi secara **atomik** dan langsung (*real-time*), dengan urutan global yang sama bagi seluruh *node*. Sistem terdistribusi tampak seolah-olah hanya berupa satu *node* tunggal.
   * **Implementasi:** `etcd`, Apache ZooKeeper, Google Spanner.
   * **Contoh:** Pengecekan keunikan ID atau *username* agar dua pengguna tidak bisa mendaftarkan nama yang sama secara bersamaan.

3. **Sequential Consistency (Konsistensi Sekuensial):**
   * Semua operasi tampak dieksekusi dalam urutan yang sama oleh semua *node*, meskipun tidak harus terjadi secara *real-time* (boleh ada sedikit jeda waktu asalkan urutannya seragam).
   * **Contoh:** Semua pengguna media sosial melihat urutan *posting* yang sama, meskipun kemunculannya mengalami sedikit *delay*.

4. **Causal Consistency (Konsistensi Kausal):**
   * Jika suatu operasi memiliki hubungan sebab-akibat dengan operasi lain, urutan tersebut wajib dijaga. Namun, operasi yang tidak saling berhubungan boleh dilihat dalam urutan berbeda oleh *node* lain.
   * **Contoh:** Di media sosial, komentar atau reaksi terhadap sebuah *posting* tidak boleh muncul sebelum *posting* aslinya tampil.

5. **Read-Your-Writes Consistency:**
   * Setelah seorang pengguna menulis/mengubah data, pembacaan berikutnya oleh **pengguna tersebut** dijamin akan melihat hasil tulisannya sendiri (meskipun pengguna lain belum tentu langsung melihatnya).
   * **Contoh:** Saat mengirim pesan di aplikasi *chatting*, pesan langsung tampil di layar pengirim walaupun penerima belum menerimanya.

6. **Monotonic Read Consistency:**
   * Jika seorang pengguna membaca data secara berulang kali, pengguna tersebut dijamin tidak akan pernah melihat versi data yang lebih lama daripada versi yang sudah pernah dilihatnya sebelumnya.
   * **Contoh:** *Timeline* media sosial tidak akan "mundur" kembali ke unggahan lama ketika halaman di-*refresh*.

7. **Eventual Consistency (Konsistensi Akhir):**
   * Tidak menjamin data langsung sama di semua *node* seketika, tetapi menjamin bahwa semua replika pada akhirnya akan memiliki data yang sama apabila tidak ada pembaruan baru dalam jangka waktu tertentu.
   * Dikenal dengan konsep **BASE**:
     * **Basically Available:** Sistem menjamin ketersediaan akses baca/tulis meskipun terjadi kegagalan sebagian.
     * **Soft State:** Status/keadaan replika tidak harus selalu konsisten setiap detik dan dapat berubah seiring waktu tanpa input baru.
     * **Eventual Consistency:** Data pada akhirnya akan konsisten di seluruh *node*.
   * **Contoh:** Amazon DynamoDB, Apache Cassandra.

---

## 5. *Trade-off* antara Latensi dan Konsistensi

Dalam sistem terdistribusi terdapat kompromi berbanding lurus antara tingkat konsistensi dan latensi (waktu respons):
* **Konsistensi Tinggi ($\uparrow$) $\rightarrow$ Latensi Tinggi ($\uparrow$):** Sistem harus menunggu proses sinkronisasi lintas jaringan ke semua *node* selesai sebelum memberikan respons kepada pengguna, sehingga waktu tunggu menjadi lebih lama.
* **Konsistensi Rendah ($\downarrow$) $\rightarrow$ Latensi Rendah ($\downarrow$):** Sistem langsung merespons pengguna tanpa menunggu semua *node* sinkron, sehingga respons sangat cepat tetapi berisiko menyajikan data yang belum mutakhir.

**Contoh Kasus (Pusat Data Jakarta & Yogyakarta):**
* Pengguna mengubah data di pusat data **Jakarta**, lalu terjadi permintaan pembacaan data.
* **Jika memilih Strong Consistency:** Sistem menunggu data di Jakarta dan Yogyakarta sinkron sepenuhnya sebelum menjawab pengguna. Data dijamin konsisten, tetapi respons lambat.
* **Jika memilih Eventual Consistency:** Sistem langsung menjawab dari Jakarta atau Yogyakarta, sementara sinkronisasi ke Yogyakarta dilakukan menyusul. Respons cepat, tetapi data antar-kota bisa berbeda sementara.

---

## 6. Replikasi Data

### Pengertian dan Tujuan Replikasi Data
* **Replikasi Data** adalah proses membuat dan memelihara salinan data yang sama di beberapa *node* atau server dalam sistem terdistribusi (misalnya menyimpan salinan database utama Jakarta ke server di Yogyakarta dan Denpasar).
* **Alur Dasar:** Operasi tulis (*writes*) masuk ke *Source* (sumber utama), kemudian direplikasi ke *Replica #1, #2, ..., #N* yang melayani permintaan baca (*reads*).

**Empat Tujuan Utama Replikasi Data:**
1. **Ketersediaan (*Availability*):** Jika satu server mengalami kegagalan, server lain tetap dapat melayani pengguna dengan salinan data yang ada.
2. **Kinerja (*Performance*):** Pengguna dapat membaca data dari server yang lokasi geografisnya paling dekat sehingga akses lebih cepat.
3. **Reliabilitas (*Reliability*):** Risiko kehilangan data berkurang drastis karena memiliki cadangan di banyak tempat.
4. **Skalabilitas (*Scalability*):** Beban pembacaan (*read load*) dapat didistribusikan ke banyak server replika.

### Jenis-Jenis Replikasi
| Jenis Replikasi | Karakteristik & Cara Kerja | Kelebihan & Kekurangan | Contoh Sistem |
| :--- | :--- | :--- | :--- |
| **Master-Slave** (*Primary-Secondary*) | Satu *node* (*Master*) menangani operasi tulis dan baca; *node* lainnya (*Slave*) hanya melayani operasi baca. | Konsistensi kuat, tetapi rentan terjadi *bottleneck* di *Master*. | PostgreSQL *streaming replication* |
| **Multi-Master** (*Peer-to-Peer*) | Semua *node* dapat menerima operasi tulis maupun baca secara bersamaan. | Sangat fleksibel dan tahan gagal, tetapi rawan konflik data. | PostgreSQL *logical replication*, Cassandra, MongoDB, Apache Ignite |
| **Synchronous Replication** | Data ditulis ke semua *node* sekaligus sebelum transaksi dinyatakan selesai (*commit*). | Konsistensi sangat tinggi, tetapi kinerja penulisan lambat. | Google Spanner |
| **Asynchronous Replication** | Pembaruan dikirim ke *node* replika setelah transaksi selesai di *Master*. | Performa penulisan sangat cepat, tetapi berisiko terjadi *data delay* atau kehilangan data jika *Master* mati mendadak. | MySQL *async mode*, DynamoDB |

---

## 7. Strategi Sinkronisasi Replika & Penanganan Konflik

### 1. Berdasarkan Waktu Sinkronisasi
* **Synchronous:** Sumber menunggu **semua** replika melakukan *commit* data. Konsistensi tertinggi, performa terendah.
* **Semi-synchronous:** Sumber menunggu minimal **satu** replika menerima data sebelum menyelesaikan transaksi. Memberikan keseimbangan antara konsistensi dan performa (digunakan oleh MySQL).
* **Asynchronous:** Sumber langsung melakukan *commit* tanpa menunggu replika. Performa tercepat, tetapi risiko kehilangan data paling tinggi.

### 2. Berdasarkan Arah Replikasi
* **Master-Slave:** *Read-Write* pada *Master*, *Read-Only* pada *Slave*.
* **Master-Master (Symmetric):** Semua *node* dapat melakukan *Read-Write* dengan peran dan kapasitas sumber daya yang setara.
* **Master-Master Asymmetric:** Semua *node* dapat melakukan *Read-Write*, tetapi memiliki perbedaan peran atau kapasitas sumber daya.

### 3. Berdasarkan Cara Sinkronisasi
* **Pull-based:** Replika secara berkala (*periodik*) memeriksa apakah ada pembaruan di *source*; jika ada, replika akan menarik data tersebut.
* **Push-based:** *Source* secara aktif langsung mengirimkan pembaruan ke replika begitu terjadi perubahan data, biasanya memanfaatkan teknologi **CDC (*Change Data Capture*)** dengan jalur komunikasi *WebSockets* atau *SSE (Server-Side Events)*.
* **Hybrid:** Kombinasi antara metode *pull* dan *push*.

### 4. Penanganan Konflik (Ketika Terjadi *Multiple Updates*)
1. **LWW (*Last Write Wins*):** Sistem membandingkan penanda waktu (*timestamp*); pembaruan dengan *timestamp* paling akhir yang akan dipertahankan, sedangkan pembaruan lainnya diabaikan.
2. **Vector Clock:** Menggunakan vektor pencatat urutan kejadian logis di setiap *node* untuk mendeteksi urutan kausalitas dan konflik antar-pembaruan.
3. **CRDT (*Conflict-free Replicated Data Types*):** Menggunakan struktur data khusus pada masing-masing *node* yang secara matematis dijamin dapat digabungkan (*merge*) tanpa konflik tanpa memerlukan koordinasi server pusat (contoh: *library* **Yjs** untuk editor kolaboratif seperti Google Docs).
   * **Contoh Perhitungan CRDT:**
     * Agnes Monica dan Bambang menghitung pengunjung situs web secara lokal dengan status awal `{"Agnes Monica": 0, "Bambang": 0}`.
     * Agnes mencatat 5 pengunjung: `{"Agnes Monica": 5, "Bambang": 0}`.
     * Bambang mencatat 3 pengunjung: `{"Agnes Monica": 0, "Bambang": 3}`.
     * Saat disinkronkan, CRDT menerapkan fungsi $\max$ untuk setiap kunci:
       * Nilai Agnes = $\max(5, 0) = 5$
       * Nilai Bambang = $\max(0, 3) = 3$
     * Hasil akhir: `{"Agnes Monica": 5, "Bambang": 3}` dengan total pengunjung $5 + 3 = 8$.
4. **OT (*Operational Transformation*):** Melakukan transformasi indeks/posisi operasi berdasarkan riwayat perubahan lokal agar hasil akhir tetap identik meskipun urutan eksekusi berbeda.
   * **Contoh Kasus OT:**
     * Teks awal adalah `"abc"` (indeks 0=`a`, 1=`b`, 2=`c`).
     * Secara bersamaan, Agnes menyisipkan `'x'` di indeks `0` (teks lokal menjadi `"xabc"`), sedangkan Bambang menghapus karakter `'c'` di indeks `2` (teks lokal menjadi `"ab"`).
     * Jika tidak ditransformasi: Agnes menghapus indeks `2` pada `"xabc"` $\rightarrow$ menjadi `"xac"` (salah, karena `'b'` yang terhapus).
     * **Solusi OT:** Pada sisi Agnes, perintah Bambang ("hapus indeks 2") ditransformasikan menyesuaikan penyisipan karakter di awal sehingga menjadi **"hapus indeks 3"** pada `"xabc"` $\rightarrow$ menghasilkan **`"xab"`**. Pada sisi Bambang, perintah Agnes ("sisipkan `'x'` di indeks 0") dijalankan pada `"ab"` $\rightarrow$ juga menghasilkan **`"xab"`**. Kedua dokumen berakhir sama persis.

---

## 8. Protokol Replikasi

1. **Primary-based (Master-Slave):** Satu *node* utama menangani semua operasi tulis dan baca, sementara replika lainnya hanya melayani baca.
2. **Quorum-based Replication:**
   * Operasi tulis (*Write*) dan baca (*Read*) hanya dianggap berhasil jika mendapat konfirmasi (`ACK`) dari sejumlah minimal *node* yang disepakati.
   * **Rumus Syarat Kuorum:**
     $$W + R > N$$
     *(di mana $N$ = total node, $W$ = minimal node konfirmasi tulis, $R$ = minimal node konfirmasi baca).*
   * **Contoh Skenario ($N = 3$: Node A, B, C dengan $W = 2$ dan $R = 2$):**
     * Syarat terpenuhi karena $2 + 2 > 3$.
     * **Operasi Tulis (`x = 100`):** Klien mengirim perintah ke Node A, B, dan C secara paralel. Node A dan B membalas `ACK`, sedangkan Node C lambat/*timeout*. Karena sudah mendapat 2 `ACK` ($W=2$), operasi tulis dinyatakan **berhasil**.
     * **Operasi Baca (`x`):** Klien membaca dari minimal 2 *node* ($R=2$), misalnya Node A dan Node C. Node A mengembalikan versi terbaru ($x = 100$), sedangkan Node C mungkin masih menyimpan versi lama. Karena himpunan baca ($R$) dan tulis ($W$) pasti beririsan pada minimal satu *node* (Node A), sistem dapat mengidentifikasi dan mengembalikan nilai terbaru ($x = 100$).
3. **State Machine Replication:**
   * Menjamin *high-availability* dengan memastikan seluruh replika mengeksekusi perintah yang sama dalam **urutan yang persis sama**.
   * **Alur Kerja:** Awalnya Replika A, B, C memiliki $x = 10$. Klien mengirim `SET x = 20` ke *leader* (Replika A). Replika A memberi nomor urutan pada log dan meneruskannya ke B dan C. Setelah B dan C mengonfirmasi bahwa perintah tercatat di log mereka, perintah dinyatakan *committed*, lalu semua replika mengeksekusi `SET x = 20` secara lokal dan *leader* mengirim konfirmasi ke klien.
4. **Gossip Protocol:**
   * Digunakan pada basis data terdistribusi seperti Apache Cassandra untuk mendeteksi status *node* dan menyebarkan pembaruan data secara desentralisasi.
   * **Alur Kerja:** Setiap detik, sebuah *node* (misalnya Node A) memilih 1–3 *peer* secara acak (misalnya Node B) untuk bertukar informasi status/pembaruan terbaru. Node B memperbarui statusnya lalu meneruskannya secara acak ke Node C. Dalam beberapa putaran penyebaran "gosip", seluruh klaster akan mengetahui pembaruan tersebut.
5. **Chain Replication:**
   * *Node* disusun berurutan membentuk rantai dari **`head`** $\rightarrow$ `A` $\rightarrow$ `B` $\rightarrow$ `C` $\rightarrow$ **`tail`**.
   * **Alur Kerja:** Klien mengirim operasi **tulis (`write`)** hanya ke node **`head`**, yang kemudian meneruskan pembaruan secara berantai hingga ke **`tail`**. Konfirmasi (`ack`) dikembalikan setelah rantai selesai diproses, sedangkan klien melakukan operasi **baca (`read`)** langsung dari node **`tail`** (yang dijamin selalu memiliki data paling mutakhir).

---

## 9. Studi Kasus & Bahan Diskusi

### Studi Kasus Implementasi di Industri
1. **PostgreSQL 18 Streaming Replication:** Implementasi arsitektur *Primary-Standby* (*Master-Slave*) di mana perubahan *Write-Ahead Log* (WAL) dari server utama dialirkan secara berkelanjutan ke server replika.
2. **Apache Ignite 3 Master-Master:** Sistem basis data terdistribusi *in-memory* yang mendukung replikasi *Multi-Master* untuk pemrosesan transaksi dan analitik berskala besar dengan latensi rendah.

### Pembahasan Pertanyaan Diskusi
1. **Apa kelebihan dan kekurangan *Strong Consistency* vs *Eventual Consistency*?**
   * **Strong Consistency:** Kelebihannya adalah akurasi dan integritas data terjamin setiap saat (cocok untuk transaksi finansial); kekurangannya adalah latensi tinggi dan ketersediaan menurun jika terjadi gangguan jaringan.
   * **Eventual Consistency:** Kelebihannya adalah latensi sangat rendah, performa tinggi, dan tetap tersedia (*available*) saat jaringan terputus; kekurangannya adalah adanya jendela waktu di mana pengguna dapat membaca data lama (*stale data*).
2. **Bagaimana jika replikasi gagal di satu *node*?**
   * Sistem akan menandai *node* tersebut tidak sinkron atau *offline* (misalnya terdeteksi melalui *Gossip Protocol* atau *timeout*). Jika menggunakan kuorum ($W+R>N$) atau *Semi-synchronous*, transaksi tetap berjalan selama jumlah *node* aktif memenuhi batas minimal. Ketika *node* yang gagal kembali pulih, mekanisme pemulihan seperti *log replay* (pada *State Machine*), *hinted handoff*, atau *anti-entropy/read-repair* akan menyinkronkan ulang data yang tertinggal.

---

## 10. Kesimpulan

* **Konsistensi** memastikan seluruh salinan data di berbagai *node* dalam sistem terdistribusi tetap akurat dan seragam, namun menurut **Teorema CAP**, sistem tidak dapat memenuhi *Consistency*, *Availability*, dan *Partition Tolerance* secara bersamaan ketika jaringan terputus sehingga perancang harus memilih antara **CP** atau **AP**.
* Pemilihan **Model Konsistensi** (mulai dari *Linearizable* hingga *Eventual Consistency*) berdampak langsung pada kinerja sistem: semakin kuat jaminan konsistensi yang diterapkan, semakin tinggi latensi yang harus ditanggung pengguna.
* **Replikasi Data** meningkatkan ketersediaan, keandalan, dan kecepatan baca sistem melalui berbagai arsitektur (*Master-Slave*, *Multi-Master*), protokol (*Quorum*, *State Machine*, *Gossip*, *Chain Replication*), serta algoritma penyelesaian konflik otomatis seperti **LWW**, **Vector Clock**, **CRDT**, dan **Operational Transformation (OT)**.

---

## 11. Daftar Istilah Penting

* **Konsistensi (*Consistency*):** Jaminan bahwa semua *node* dalam sistem terdistribusi memiliki atau menampilkan data yang sama pada waktu tertentu.
* **Teorema CAP:** Prinsip bahwa sistem terdistribusi hanya dapat menjamin maksimal dua dari tiga sifat secara bersamaan saat terjadi gangguan jaringan: *Consistency*, *Availability*, dan *Partition Tolerance*.
* **Linearizability:** Model konsistensi terkuat di mana setiap operasi tampak terjadi secara atomik dan *real-time* dalam urutan global tunggal.
* **BASE:** Paradigma sistem *Eventual Consistency* yang terdiri dari *Basically Available*, *Soft State*, dan *Eventual Consistency*.
* **Latensi (*Latency*):** Waktu tunda atau waktu respons yang dibutuhkan sistem sejak permintaan dikirim hingga jawaban diterima.
* **Replikasi Data:** Proses menyalin dan menjaga kesamaan data di beberapa server berbeda untuk meningkatkan *availability* dan *reliability*.
* **CDC (*Change Data Capture*):** Mekanisme untuk mendeteksi dan menangkap perubahan data pada sumber database secara *real-time* untuk dikirimkan ke replika.
* **CRDT (*Conflict-free Replicated Data Types*):** Struktur data terdistribusi yang dapat digabungkan dari berbagai *node* secara otomatis tanpa menimbulkan konflik.
* **Operational Transformation (OT):** Teknik penyelesaian konflik pada pengeditan kolaboratif dengan mengubah indeks operasi berdasarkan perubahan konkuren yang terjadi.
* **Kuorum (*Quorum*):** Jumlah minimal *node* ($W$ untuk tulis, $R$ untuk baca, dengan syarat $W + R > N$) yang harus menyetujui operasi agar transaksi dianggap sah.

---

## 12. Pertanyaan Latihan dan Jawaban

**1. Jelaskan mengapa kondisi ideal CAP (memenuhi *Consistency*, *Availability*, dan *Partition Tolerance* sekaligus) tidak dapat dicapai pada sistem terdistribusi nyata!**
* **Jawaban:** Karena kegagalan atau terputusnya koneksi jaringan antar-server (*network partition*) adalah kondisi yang tidak dapat dihindari di dunia nyata. Ketika partisi jaringan terjadi dan pesan antar-*node* tidak dapat dikirim, sistem dihadapkan pada dua pilihan: tetap melayani permintaan pengguna meskipun data antar-*node* belum sinkron (memilih **AP**), atau menolak permintaan demi menjaga agar data tidak berbeda antar-*node* (memilih **CP**).

**2. Apa perbedaan mendasar antara model konsistensi *Read-Your-Writes* dengan *Monotonic Read*?**
* **Jawaban:** *Read-Your-Writes* menjamin bahwa setelah seorang pengguna memperbarui/menulis data, pembacaan berikutnya oleh pengguna tersebut pasti menampilkan hasil tulisannya sendiri. Sementara itu, *Monotonic Read* menjamin bahwa ketika pengguna membaca data berkali-kali, ia tidak akan pernah melihat versi data yang lebih lama dibandingkan versi terbaru yang sudah pernah ia lihat sebelumnya.

**3. Sebuah klaster basis data terdistribusi memiliki 5 node ($N = 5$). Jika sistem menetapkan minimal 3 node harus mengonfirmasi proses penulisan ($W = 3$), berapa jumlah minimal node ($R$) yang harus dikonfirmasi saat proses pembacaan agar konsistensi kuorum terpenuhi?**
* **Jawaban:** Berdasarkan rumus syarat kuorum $W + R > N$:
  $$3 + R > 5 \implies R > 2$$
  Maka, jumlah minimal *node* yang harus dikonfirmasi pada proses pembacaan adalah **$R = 3$** (sehingga $3 + 3 = 6 > 5$, yang menjamin adanya minimal 1 *node* irisan yang menyimpan data terbaru).

**4. Bagaimana cara kerja *Operational Transformation (OT)* menyelesaikan konflik ketika dua pengguna mengedit teks secara bersamaan?**
* **Jawaban:** OT tidak sekadar menimpa data berdasarkan waktu terakhir, melainkan menyesuaikan (mentransformasi) parameter posisi/indeks dari operasi yang diterima dari *node* lain berdasarkan riwayat perubahan lokal yang sudah lebih dulu dieksekusi. Dengan menyesuaikan indeks penyisipan atau penghapusan karakter tersebut, hasil akhir dokumen di kedua *node* dijamin identik.

**5. Jelaskan alur operasi tulis (*write*) dan baca (*read*) pada protokol *Chain Replication*!**
* **Jawaban:** Pada *Chain Replication*, *node* disusun secara berurutan dari *head* hingga *tail*. Semua permintaan **tulis (*write*)** dari klien masuk melalui *node* **`head`**, kemudian diteruskan secara berantai satu per satu melewati setiap *node* perantara hingga mencapai *node* **`tail`**. Sebaliknya, semua permintaan **baca (*read*)** dilayani langsung oleh *node* **`tail`** karena *tail* hanya memproses data yang sudah tervalidasi melewati seluruh rantai.