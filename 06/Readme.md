# Materi Minggu 6: Distributed File System (DFS)

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

### Perbandingan Arsitektur Penyimpanan: DAS, NAS, SAN, dan DFS
1. **DAS (Direct-Attached Storage):**
   * Media penyimpanan yang terhubung langsung ke satu komputer/server tertentu.
2. **NAS (Network-Attached Storage):**
   * Penyimpanan bersama (*shared storage*) melalui jaringan bersama (*shared network*) standar (TCP/IP).
   * Bekerja di tingkat *file system* (seperti NFS atau SMB) dan memiliki manajemen yang lebih mudah.
3. **SAN (Storage Area Network):**
   * Penyimpanan bersama melalui jaringan khusus berkecepatan tinggi (*dedicated network*, seperti Fibre Channel atau iSCSI).
   * Menyajikan penyimpanan sebagai sekumpulan **block-level devices** yang dihubungkan langsung ke server dan diformat dengan FS sendiri oleh server tersebut.
   * Karakteristik: Sangat cepat tetapi mahal. Umumnya digunakan sebagai *backend* untuk database berskala besar, *video-audio editing*, dan virtualisasi berskala besar.
4. **DFS vs SAN:**
   * DFS merupakan abstraksi di level perangkat lunak (*software-level*) yang mengelola data dalam bentuk file dan direktori yang didistribusikan ke banyak *node*.
   * Secara konseptual SAN bisa dijadikan penyimpanan untuk DFS meskipun **tidak disarankan**. DFS lebih mengarah pada pemanfaatan **DAS** yang terpasang di masing-masing *node*.

### Berbagai Jenis DFS yang Umum Digunakan
* **Google File System (GFS):** Sistem file terdistribusi milik Google.
* **Colossus:** Generasi penerus Google File System yang digunakan pada layanan berskala raksasa seperti YouTube, Gmail, dan Google Docs.
* **CephFS:** Sistem file terdistribusi dari ekosistem Ceph (`ceph.io`).
* **Gluster (GlusterFS):** Sistem file terdistribusi *open-source* yang dapat diskalakan (`gluster.org`).
* **HDFS (Hadoop Distributed File System):** Komponen penyimpanan utama pada ekosistem Apache Hadoop (`hadoop.apache.org`).
* **Lustre:** Sistem file paralel terdistribusi yang sering digunakan pada komputasi kinerja tinggi/superkomputer (`lustre.org`).
---
**MATERI HASIL AI (GEMINI) DIBAWAH INI:**

**TERMASUK YANG SAYA TAMBAHKAN**
## 1. Tujuan Pembelajaran & Agenda Materi

### Tujuan Pembelajaran
Setelah mempelajari modul ini, mahasiswa diharapkan mampu:
1. Memahami dan menjelaskan pengertian *File System* (FS).
2. Menyebutkan berbagai jenis *File System*.
3. Menjelaskan cara kerja *File System* secara umum.
4. Memahami dan menjelaskan pengertian *Distributed File System* (DFS).
5. Menyebutkan berbagai jenis DFS.
6. Menjelaskan cara kerja DFS secara umum.
7. Melakukan instalasi salah satu DFS untuk memahami cara kerjanya.
8. Memahami dan menjelaskan *Cloud Storage* serta hubungannya dengan DFS.
9. Melakukan instalasi salah satu *object storage* sebagai *cloud storage*.

### Agenda Pembelajaran
* Pengertian File System, Berbagai FS, dan Cara Kerja FS
* Pengertian Distributed File System, Berbagai DFS yang Umum, dan Cara Kerja DFS
* Studi Kasus: HDFS (Hadoop Distributed File System)
* Pengertian Cloud Storage, Relasi Cloud Storage dengan DFS, Tipe Cloud Storage, dan Studi Kasus

---

## 2. File System (FS)

### Pengertian File System
* **File System** (sering ditulis *filesystem* dan disingkat **FS** atau **fs**) adalah metode yang digunakan oleh sistem operasi untuk mengelola, mengorganisasi, dan mengatur file pada media penyimpanan (seperti *harddisk*, USB *flashdisk*, SSD, dll.).
* **Perbedaan FS dengan Hirarki FS:** 
  * *File System* adalah sistem pengelola data di tingkat penyimpanan fisik/partisi.
  * *Hirarki FS* adalah pengaturan struktur file dan direktori (*folder*) pada suatu sistem operasi. Pengguna umumnya hanya melihat dan berinteraksi dengan hirarki FS tanpa perlu mempedulikan jenis FS yang berjalan di baliknya.

### Berbagai Jenis File System Berdasarkan Sistem Operasi
| Sistem Operasi | Contoh File System |
| :--- | :--- |
| **Linux** | `ext4`, `XFS`, `btrfs`, `ReiserFS`, dll. |
| **DOS / Windows** | `FAT 16`, `FAT 32`, `NTFS`, dll. |
| **macOS** | `APFS` (*Apple File System*), dll. |

### Cara Kerja File System
Suatu *File System* diakses melalui **driver file system** yang dikelola oleh **kernel**. Kernel menyediakan lapisan **Virtual File System (VFS)** agar dapat diakses secara seragam oleh aplikasi di ruang pengguna (*User Space*).

**Alur Akses File System:**
```text
Aplikasi (User Space) ──▶ System Call Interface ──▶ Virtual FS (VFS) ──▶ Driver Kernel ──▶ Hardware (Media Penyimpan)
```

**Penjelasan Arsitektur Kernel Linux & Hirarki Direktori (FHS):**
1. **User Space / Applications:** Berisi *System Utilities*, *Shell/CLI*, dan *GUI Applications*. Ketika aplikasi ingin membaca/menulis file, ia memanggil antarmuka sistem.
2. **System Call Interface:** Jembatan antara *User Space* dan *Kernel Space* melalui *system calls* seperti `read()` dan `write()`.
3. **Kernel Space:** Terdiri dari subsistem seperti *Process Management*, *Memory Management (MMU)*, *Inter-Process Communication (IPC)*, *Network Stack (TCP/IP)*, dan **File Systems** (misalnya EXT4, XFS). Di bawahnya terdapat **Device Drivers** (*Storage*, *Network*, *Input/Output*) yang mengontrol perangkat keras secara langsung.
4. **Hardware:** Perangkat keras fisik meliputi CPU, RAM, *Disk Storage*, dan *Network Interface*.
5. **Filesystem Hierarchy Standard (FHS):** Standar penempatan file dan direktori pada sistem operasi mirip UNIX (versi 3.0) agar aplikasi dan skrip dapat berjalan seragam. Contoh struktur direktori root (`/`):
   * `/bin`: Biner perintah pengguna esensial.
   * `/boot`: File statis untuk *boot loader*.
   * `/dev`: File perangkat (*device files*).
   * `/etc`: Konfigurasi sistem spesifik *host*.
   * `/home`: Direktori *home* pengguna.
   * `/lib`, `/lib64`: Pustaka bersama (*shared libraries*) dan modul kernel esensial.
   * Direktori lainnya: `/mnt`, `/opt`, `/proc`, `/root`, `/run`, `/sbin`, `/srv`, `/sys`, `/tmp`, `/usr`, `/var`.

---

## 3. Distributed File System (DFS)

### Pengertian Distributed File System
* Secara konseptual, **DFS** bukan sekadar *file system* yang tersebar di lebih dari satu mesin, melainkan **sistem abstraksi terkoordinasi** dari sekumpulan media penyimpanan yang menutupi heterogenitas (perbedaan perangkat), kegagalan (*failure*), dan konkurensi (akses bersamaan).
* Tujuannya adalah menghasilkan jaringan penyimpanan yang terintegrasi, andal (*reliable*), dan berkinerja tinggi.
* Meskipun secara fisik terdiri dari banyak media penyimpanan di berbagai mesin, dari sisi pengguna DFS terlihat dan diakses layaknya **satu media penyimpanan tunggal**.

### Perbandingan Arsitektur Penyimpanan: DAS, NAS, SAN, dan DFS
1. **DAS (Direct-Attached Storage):**
   * Media penyimpanan yang terhubung langsung ke satu komputer/server tertentu.
2. **NAS (Network-Attached Storage):**
   * Penyimpanan bersama (*shared storage*) melalui jaringan bersama (*shared network*) standar (TCP/IP).
   * Bekerja di tingkat *file system* (seperti NFS atau SMB) dan memiliki manajemen yang lebih mudah.
3. **SAN (Storage Area Network):**
   * Penyimpanan bersama melalui jaringan khusus berkecepatan tinggi (*dedicated network*, seperti Fibre Channel atau iSCSI).
   * Menyajikan penyimpanan sebagai sekumpulan **block-level devices** yang dihubungkan langsung ke server dan diformat dengan FS sendiri oleh server tersebut.
   * Karakteristik: Sangat cepat tetapi mahal. Umumnya digunakan sebagai *backend* untuk database berskala besar, *video-audio editing*, dan virtualisasi berskala besar.
4. **DFS vs SAN:**
   * DFS merupakan abstraksi di level perangkat lunak (*software-level*) yang mengelola data dalam bentuk file dan direktori yang didistribusikan ke banyak *node*.
   * Secara konseptual SAN bisa dijadikan penyimpanan untuk DFS meskipun **tidak disarankan**. DFS lebih mengarah pada pemanfaatan **DAS** yang terpasang di masing-masing *node*.

### Berbagai Jenis DFS yang Umum Digunakan
* **Google File System (GFS):** Sistem file terdistribusi milik Google.
* **Colossus:** Generasi penerus Google File System yang digunakan pada layanan berskala raksasa seperti YouTube, Gmail, dan Google Docs.
* **CephFS:** Sistem file terdistribusi dari ekosistem Ceph (`ceph.io`).
* **Gluster (GlusterFS):** Sistem file terdistribusi *open-source* yang dapat diskalakan (`gluster.org`).
* **HDFS (Hadoop Distributed File System):** Komponen penyimpanan utama pada ekosistem Apache Hadoop (`hadoop.apache.org`).
* **Lustre:** Sistem file paralel terdistribusi yang sering digunakan pada komputasi kinerja tinggi/superkomputer (`lustre.org`).

### Cara Kerja DFS
DFS bekerja dengan menyatukan berbagai *namespace* penyimpanan yang terpisah secara fisik ke dalam satu **Global Namespace**.
* Pengguna dan aplikasi mengakses data melalui satu pintu gerbang (**Global Namespace** pada DFS) tanpa perlu mengetahui di mana data tersebut berada secara fisik.
* Di balik layar, DFS mengoordinasikan dan memetakan akses tersebut ke berbagai sumber penyimpanan heterogen, seperti perangkat NAS di *Data Center* (`Namespace 1`, `Namespace 2`), server Linux NFS dan Windows SMB di lokasi jarak jauh (`Namespace 3`, `Namespace 4`), hingga *Public Cloud* (`Namespace 5`).

---

## 4. Studi Kasus: HDFS (Hadoop Distributed File System)

### Pengertian dan Karakteristik HDFS
* **HDFS** adalah *file system* terdistribusi yang bersifat *open-source*, *scalable* (mudah diperbesar kapasitasnya), dan *fault-tolerant* (tahan terhadap kegagalan perangkat).
* Dirancang khusus untuk mengelola dan menyimpan dataset berukuran sangat besar di atas klaster **commodity hardware** (perangkat keras komputer standar yang diproduksi massal, berbiaya murah, mudah dipertukarkan, dan tersedia luas dari berbagai vendor).
* Cara kerjanya adalah dengan memecah file berukuran besar menjadi blok-blok kecil (*blocks*), mendistribusikannya ke beberapa server, dan membuat salinan (*replikasi*) antar-server untuk menjamin ketersediaan tinggi (*high-availability*).
* **Spesifikasi Versi (Apache Hadoop 3.5.0):** Dikompilasi menggunakan JDK 17. Dapat dijalankan menggunakan Java 17 untuk sisi *server*, serta Java 17 atau Java 21 untuk sisi *client*.

### Arsitektur HDFS (Master-Slave)
HDFS menerapkan arsitektur **Master-Slave** dengan komponen utama sebagai berikut:
1. **Master Node (NameNode & Resource Manager):**
   * **NameNode:** Bertindak sebagai pusat kendali yang mengelola *namespace* sistem file dan menyimpan **Metadata** (seperti nama file, lokasi blok, jumlah replika, izin akses; contoh: `/home/foo/data, 3, ...`). NameNode tidak menyimpan data aktual, melainkan mengatur operasi metadata (*Metadata ops*) dengan Client dan operasi blok (*Block ops*) dengan DataNode.
   * **Resource Manager:** Mengelola alokasi sumber daya komputasi pada klaster.
2. **Slave Nodes (DataNode & Node Manager):**
   * **DataNode:** Bertugas menyimpan blok-blok data fisik yang sebenarnya pada penyimpanan lokal masing-masing node. DataNode melayani permintaan baca (*Read*) dan tulis (*Write*) langsung dari Client, serta melakukan replikasi blok antar-DataNode (termasuk lintas rak server seperti *Rack 1* dan *Rack 2* untuk keamanan data jika satu rak mati).
   * **Node Manager & MapReduce:** Menjalankan tugas pemrosesan data paralel langsung di node tempat data berada.


---

## 5. Cloud Storage dan Relasinya dengan DFS

### Pengertian Cloud Storage
* **Cloud Storage** adalah layanan penyimpanan *online* yang memungkinkan pengguna menyimpan dan mengakses data pada server jarak jauh melalui jaringan internet (bukan di *hard drive* lokal).
* Data dikelola secara *remote* dalam suatu *pool* logis yang terpadu, sementara secara fisik data tersebar di berbagai server (sering kali di beberapa lokasi geografis) yang dimiliki dan dikelola oleh penyedia layanan *cloud computing*.
* **Contoh Layanan Populer:** Amazon S3 (*Simple Storage Service*), Microsoft Azure Blob Storage, dan Google Cloud Storage (GCS).

### Relasi Cloud Storage dengan DFS
* **Cloud Storage** berperan sebagai **tempat/media penyimpanan fisik/logis**, sedangkan **DFS** berperan melakukan **pengorganisasian dan operasi file system** di atasnya.
* **Contoh Implementasi (HDFS + Amazon S3):** HDFS dapat menggunakan Amazon S3 sebagai media penyimpanannya. S3 bertindak sebagai *cloud storage*, sementara seluruh operasi sistem file dikelola oleh HDFS melalui protokol `s3a`.

**Contoh Kode Konfigurasi pada `core-site.xml` (Hadoop):**
```xml
<property>
  <name>fs.s3a.access.key</name>
  <value>YOUR_AWS_ACCESS_KEY_ID</value>
</property>
<property>
  <name>fs.s3a.secret.key</name>
  <value>YOUR_AWS_SECRET_ACCESS_KEY</value>
</property>
<property>
  <name>fs.s3a.endpoint</name>
  <value>s3.amazonaws.com</value> <!-- Atau endpoint region spesifik -->
</property>
```
* **Fungsi & Cara Kerja Kode:** Konfigurasi XML ini mendaftarkan kredensial akses (`access.key` dan `secret.key`) serta alamat server tujuan (`endpoint`) milik Amazon S3 ke dalam konfigurasi inti Hadoop (`core-site.xml`). Dengan konfigurasi ini, aplikasi berbasis Hadoop/HDFS dapat langsung membaca dan menulis data ke *bucket* S3 seolah-olah mengakses sistem file lokalnya.

### Tiga Tipe Cloud Storage
| Aspek Perbandingan | Block Storage | File Storage | Object Storage |
| :--- | :--- | :--- | :--- |
| **Protokol Transport** | FC (*Fibre Channel*) atau iSCSI | TCP/IP | TCP/IP |
| **Antarmuka (Interface)** | Direct Attached atau SAN | NFS, SMB | HTTP, REST API |
| **Kasus Penggunaan (Use Case)** | Latensi rendah (*Low Latency*), sangat cocok untuk data terstruktur (*Structured Data*) seperti database. | Performa baik untuk berbagi file (*File Sharing*) dan penguncian file global (*Global File Locking*). | Skalabilitas sangat mudah tanpa batas (*No Limits*), dapat diakses luas melalui LAN & WAN (contoh: S3). |

### Studi Kasus Sistem Penyimpanan Cloud / Object Storage Modern
1. **Ceph (`ceph.com`):** Sistem penyimpanan terdistribusi *open-source* yang menyediakan penyimpanan terpadu (*unified storage*) meliputi blok, file, dan objek yang andal serta berskala besar.
2. **CubeFS (`cubefs.io`):** Sistem penyimpanan data tidak terstruktur (*unstructured data*) generasi baru berbasis *cloud-native* (proyek CNCF). Mendukung berbagai protokol akses sekaligus (**S3, HDFS, dan POSIX**).
   * **Arsitektur CubeFS:** Terdiri dari **Master** sebagai koordinator utama, **Metadata Subsystem** (*Meta Node* dengan partisi metadata di memori), **Data Subsystem** (*Data Node* untuk partisi data dan *Blob Node* dengan *Erasure Code Stripe*), serta **Object Subsystem** (*Object Node*) yang melayani aplikasi melalui AWS S3 SDK maupun klien (Container, VM, Server) melalui antarmuka POSIX/HDFS.
3. **RustFS (`rustfs.com`):** Sistem *object storage* berperforma tinggi yang dirancang sebagai pengganti langsung (*drop-in replacement*) bagi MinIO dan S3. Dibangun dengan bahasa pemrograman Rust sehingga bebas dari *Garbage Collection* (*Zero GC*) untuk menghasilkan *throughput* maksimal pada arsitektur *multi-cloud*.
4. **PGSTY SILO (`silo.pgsty.com`):** *Object storage* *open-source* yang kompatibel dengan S3/MinIO (merupakan *community fork* dari MinIO yang dikelola oleh PGSTY). Menyediakan S3 API pada port `:9000` untuk lalu lintas aplikasi, Console operator pada port `:9001`, dan menggunakan *Erasure Set* di atas disk terdistribusi (`D01`–`D04`).
5. **Rook (`rook.io`):** Orkestrator penyimpanan *cloud-native* *open-source* khusus untuk lingkungan **Kubernetes**, yang mengotomatiskan manajemen produksi untuk *File*, *Block*, dan *Object Storage* (seperti menjalankan Ceph di atas Kubernetes).

---

## 6. Kesimpulan

* **File System (FS)** mengatur cara sistem operasi menyimpan data secara lokal pada satu mesin melalui abstraksi *Virtual File System* (VFS) di dalam kernel, sedangkan **Distributed File System (DFS)** memperluas konsep tersebut ke banyak mesin (*node*) namun tetap menyajikannya sebagai satu ruang nama tunggal (*Global Namespace*) yang andal dan tahan gangguan.
* Berbeda dengan **SAN** yang menyediakan penyimpanan mentah di level blok (*block-level*) melalui jaringan khusus yang mahal, **DFS** bekerja pada level perangkat lunak dengan memanfaatkan penyimpanan lokal (*DAS*) di atas *commodity hardware* yang murah dan mudah diskalakan—seperti yang diterapkan oleh **HDFS** melalui arsitektur *NameNode* (pengelola metadata) dan *DataNode* (penyimpan blok data ter-replikasi).
* Dalam ekosistem komputasi awan modern, **Cloud Storage** (terutama *Object Storage* berbasis HTTP/REST seperti S3) bersinergi erat dengan DFS: *Cloud Storage* menyediakan kolam penyimpanan berkapasitas tak terbatas, sementara DFS (atau sistem terpadu seperti Ceph dan CubeFS) menyediakan antarmuka sistem file dan pengorganisasian datanya.