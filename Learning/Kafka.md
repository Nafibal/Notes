**Tags:**
#programming #tools #kafka #event-stream #message-broker 
**Reference:**
[Kafka Docs]([https://github.com/charmbracelet/bubbletea](https://kafka.apache.org/42/getting-started/introduction/))
___
# Event Streaming
Event streaming adalah praktik:
- menangkap data secara real-time dari sumber event (database, sensor, mobile device, cloud, dan lain2) dalam bentuk stream of events
- menyimpan event stream sehingga dapat diambil di lain waktu
- manipulasi, memroses, dan bereaksi secara langsung (realtime) terhadap event stream
- routing event stream ke beberapa tujuan berbeda sesuai kebutuhan
 
---

Secara teknis, _Event Streaming_ adalah praktik menangkap data secara **real-time** dari berbagai sumber (database, sensor, aplikasi) dalam bentuk aliran peristiwa (_streams of events_).

Bayangkan sebuah sungai yang terus mengalir. Setiap tetes air adalah satu "event" (misal: klik tombol, transaksi masuk, suhu sensor naik).

- **Capture:** Mengambil data saat itu juga.
- **Store:** Menyimpan aliran data tersebut dengan aman (durabel).
- **Process:** Mengolah atau bereaksi terhadap data tersebut (misal: jika ada transaksi mencurigakan, blokir otomatis).
- **Route:** Mengalirkan data tersebut ke tempat yang membutuhkan (database lain, dashboard analitik, dsb).

# Kegunaan
Event streaming mengubah cara sistem bekerja dari pasif menjadi proaktif. Beberapa contoh relevan:

- **Fintech & Perbankan:** Memproses pembayaran dan mendeteksi penipuan (_fraud detection_) dalam hitungan milidetik sebelum transaksi selesai.
- **Logistik & IoT:** Melacak posisi armada truk secara _live_ atau memantau kesehatan mesin pabrik melalui sensor untuk mencegah kerusakan sebelum terjadi.
- **Retail:** Memberikan rekomendasi produk yang dipersonalisasi tepat setelah user melihat barang tertentu di aplikasi.
- **Microservices:** Menjadi "jembatan" komunikasi antar layanan agar sistem tetap sinkron tanpa harus saling menunggu (_decoupled_).

# Apache Kafka
Kafka bukan sekadar _message broker_ (seperti RabbitMQ), tapi platform lengkap karena menggabungkan tiga kemampuan utama secara bersamaan:

1. **Publish & Subscribe:** Menulis dan membaca aliran data dengan performa tinggi.
2. **Storage:** Berbeda dengan sistem pesan lama yang menghapus data setelah dibaca, Kafka bisa menyimpan data tersebut secara permanen dan aman di dalam disk. Kamu bisa "memutar ulang" (_replay_) data dari masa lalu.
3. **Real-time Processing:** Mengolah data saat ia mengalir, bukan menunggu data dikumpulkan dulu (batching).

### Mengapa ini relevan untuk workflow modern?

Karena Kafka bersifat **distributed** dan **fault-tolerant**, ia sangat cocok dengan infrastruktur cloud atau container (seperti Docker/Kubernetes). Jika kamu sedang mendalami backend dengan Go atau Next.js, memahami _event-driven architecture_ (EDA) akan membantu kamu membangun sistem yang sangat _scalable_. Data tidak lagi diam di database, tapi terus bergerak dan memberi nilai secara instan.

Misalnya, jika kamu membangun fitur _real-time analytics_ untuk dashboard, kamu tidak perlu melakukan _query_ berat ke database setiap detik. Cukup "dengarkan" _stream_ dari Kafka dan update state dashboard-mu.

# Cara Kerja
Secara sederhana, **Apache Kafka** adalah platform _event streaming_ terdistribusi. Jika diibaratkan, Kafka itu seperti sebuah **"pusat logistik data"** yang sangat besar, cepat, dan tahan banting, di mana data (event) dikirim, disimpan, dan diambil secara _real-time_ oleh berbagai sistem.

Berikut adalah penjelasan ringkas mengenai komponen utamanya:

### 1. Servers (The Backbone / Brokers)

Di dalam klaster Kafka, _server_ bertindak sebagai "gudang" data yang disebut **Brokers**.

- **Penyimpanan:** Mereka menyimpan data dalam bentuk _Topic_ (seperti folder atau kategori pesan).
- **Fault Tolerance:** Karena sistemnya terdistribusi, data tidak disimpan di satu tempat saja. Jika salah satu server mati, data masih tersedia di server lain (replikasi), sehingga sistem tetap berjalan lancar (_high availability_).
- **Kafka Connect:** Ini adalah fitur tambahan untuk "menjemput" atau "mengirim" data ke sistem luar. Misalnya, jika kamu ingin memindahkan perubahan data dari database (seperti PostgreSQL) ke Kafka secara otomatis, kamu menggunakan Kafka Connect.
### 2. Clients (The Application Layer)

_Clients_ adalah aplikasi yang kamu buat. Dalam ekosistem Kafka, mereka biasanya terbagi menjadi dua peran utama:

- **Producers:** Aplikasi yang mengirim (menulis) _event_ ke Kafka.
- **Consumers:** Aplikasi yang membaca (mengolah) _event_ dari Kafka.
### Mengapa ini penting bagi arsitektur software?

Sebagai seorang _backend developer_, Kafka memberikan keuntungan besar dalam **Decoupling (Pemisahan Sistem)**:

- **Asynchronous Processing:** Aplikasi kamu tidak perlu menunggu sistem lain selesai memproses data. _Producer_ cukup kirim data ke Kafka, dan _Consumer_ akan memprosesnya kapan pun mereka siap.
- **Scalability:** Karena Kafka bisa berjalan di klaster yang tersebar, kamu bisa menangani jutaan _event_ per detik dengan menambah jumlah server atau membagi _partition_ data.
- **Reliability:** Jika sistem _Consumer_ kamu _down_ karena ada masalah, data tidak akan hilang. Begitu _Consumer_ kamu menyala kembali, ia bisa melanjutkan pembacaan data dari titik terakhir sebelum _down_.
 
---
### Contoh Skenario:

Bayangkan kamu sedang membangun sistem _e-commerce_.

1. **Producer:** Saat pengguna klik "Beli", layanan _Order_ mengirimkan _event_ `ORDER_CREATED` ke Kafka.
2. **Kafka:** Menyimpan event tersebut dengan aman.
3. **Consumers:**
    - Layanan _Inventory_ membaca event tersebut untuk mengurangi stok.
    - Layanan _Notification_ membaca event tersebut untuk mengirim email konfirmasi.
    - Layanan _Analytics_ membaca event tersebut untuk statistik penjualan.

Ketiga layanan ini tidak saling tahu satu sama lain; mereka hanya "mendengar" (listen) ke topik yang sama di Kafka.

# Konsep dan Terminologi
### 1. Event (Data/Pesan)

Event adalah catatan tentang sesuatu yang sudah terjadi. Di Kafka, event bukan cuma teks biasa, tapi sebuah objek yang punya:

- **Key:** Identitas (misal: ID User "Alice"). Ini penting untuk menentukan ke mana data pergi.
- **Value:** Isinya (misal: "Bayar $200 ke Bob").
- **Timestamp:** Kapan itu terjadi.
- **Metadata:** Informasi tambahan (seperti header di HTTP).

### 2. Producers vs Consumers (Decoupling)

Ini adalah kunci skalabilitas Kafka.

- **Producers:** Aplikasi yang menulis data.
- **Consumers:** Aplikasi yang membaca data.
- **Decoupled:** Keduanya tidak saling kenal. Producer tidak peduli siapa yang baca atau apakah si pembaca lagi lemot/mati. Dia cuma kirim ke Kafka, lalu lanjut ke tugas berikutnya. Ini sangat efisien untuk sistem yang sibuk.

### 3. Topics (Folder Data)

Jika Kafka adalah sistem file, maka **Topic adalah Folder**.

- Kamu bisa punya topic `transaksi_bank`, `log_aplikasi`, atau `update_lokasi`.
- Berbeda dengan _message queue_ tradisional yang menghapus pesan setelah dibaca, Kafka menyimpan data di Topic sesuai durasi yang kamu set (misal: simpan selama 7 hari atau selamanya). Data bisa dibaca berulang kali oleh banyak orang.

### 4. Partitions (Rahasia Kecepatan Kafka)

Ini konsep yang paling krusial. Sebuah Topic tidak disimpan dalam satu file besar, tapi dipecah menjadi beberapa **Partitions** (seperti laci-laci dalam satu lemari).

- **Scalability:** Karena datanya mencar di beberapa partition (yang bisa berada di server/broker berbeda), banyak Producer dan Consumer bisa bekerja secara paralel di waktu yang sama.
- **Order Guarantee:** Kafka menjamin urutan data **tetap konsisten di dalam satu partition**. Jika kamu pakai Key yang sama (misal ID User yang sama), Kafka akan selalu memasukkan data user tersebut ke partition yang sama agar urutan transaksinya tidak tertukar.

### 5. Replication (Anti-Mati)

Kafka sangat peduli dengan keamanan data. Setiap partition biasanya punya cadangan (**Replica**).

- Jika kamu punya `replication factor: 3`, artinya ada 3 salinan data yang tersebar di broker yang berbeda.
- Kalau satu server meledak atau _down_ saat kamu melakukan maintenance di Fedora server-mu, data tetap aman karena masih ada 2 salinan lainnya di server lain.

---

![[Pasted image 20260417180906.png]]

> **Topic** (Lemari) -> **Partition** (Laci) -> **Event** (Kertas catatan di dalam laci).
> 
> **Producers** menaruh catatan ke laci, **Consumers** membaca catatan dari laci tersebut. Semuanya diatur oleh **Brokers** (Server).