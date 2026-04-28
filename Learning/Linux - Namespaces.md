**Tags:**
#linux #container
**Reference:**
 - [Digging into Linux namespaces - part 1](https://blog.quarkslab.com/digging-into-linux-namespaces-part-1.html "Permalink to Digging into Linux namespaces - part 1")
 - [Digging into Linux namespaces - part 2](https://blog.quarkslab.com/digging-into-linux-namespaces-part-2.html "Permalink to Digging into Linux namespaces - part 2")
 
___

# Apa itu Linux Namespaces

Secara teknis, Namespaces adalah fitur kernel Linux yang mengisolasi dan memvirtualisasikan _system resources_ untuk sekumpulan proses.

Saat kita memasukkan proses ke dalam sebuah _namespace_, sumber daya tertentu (seperti IP address, daftar proses, atau _mount points_) akan terlihat berbeda untuk proses tersebut dibandingkan dengan proses di luar.

---

# Jenis-jenis Namespaces

Linux punya beberapa jenis isolasi. Ibarat sebuah apartemen, masing-masing ini adalah sekat yang berbeda:

1. **Mount (mnt):** Isolasi _mount points_. Proses di dalam `mnt` namespace punya struktur _file system_ sendiri. Lo bisa _mount/unmount_ sesuatu di sini tanpa ganggu _host_ utama.
2. **Process ID (pid):** Ini yang paling sakti. Proses di dalam namespace ini bakal ngerasa dirinya punya **PID 1** (kayak _systemd_ atau _init_), padahal di _host_ aslinya dia mungkin punya PID 1234.
3. **Network (net):** Isolasi _stack_ jaringan. Di dalamnya, proses punya IP address, _routing table_, dan _firewall_ (iptables) sendiri.
4. **Inter-process Communication (ipc):** Isolasi memori bareng (_shared memory_) dan antrean pesan.
5. **UTS (Unix Timesharing System):** Sederhana tapi penting. Ini isolasi **hostname** dan **domain name**. Lo bisa ganti nama komputer di dalam namespace tanpa ngerubah nama komputer aslinya.
6. **User (user):** Isolasi User ID dan Group ID. Lo bisa jadi `root` (UID 0) di dalam container, tapi di mata OS _host_, lo cuma _user_ biasa tanpa hak akses spesial. Ini krusial buat keamanan.
7. **Cgroup:** Isolasi tampilan _Control Groups_. Ini ngebatesin apa yang bisa dilihat proses soal penggunaan sumber daya (CPU/RAM).

---

# PID Namespace

Linux PID Namespace merupakan fitur kernel yang memungkinkan virtualisasi hierarki proses dengan mengisolasi identitas proses (PID) di dalam lingkup yang berbeda. Secara historis, Linux beroperasi pada satu pohon proses tunggal di mana setiap proses memiliki ID unik dan dikelola oleh `systemd` (PID 1) sebagai akar dari seluruh operasi sistem. Melalui PID Namespace, kernel dapat menciptakan pohon proses bertingkat (_nested process trees_) yang memungkinkan sebuah proses baru menganggap dirinya sebagai PID 1 di dalam sub-ekosistemnya sendiri, tanpa mengganggu stabilitas identitas proses pada _host_ utama.

Dalam implementasi internal kernel, sebuah proses tidak lagi diwakili oleh satu integer statis, melainkan melalui struktur data `struct pid` dan `struct upid`. Struktur ini menyimpan array identitas yang memungkinkan sebuah proses memiliki beberapa PID sekaligus, tergantung dari tingkat kedalaman namespace-nya. Sebagai contoh, sebuah proses dapat terlihat sebagai PID 1 di dalam kontainer, namun tetap tercatat sebagai PID 13512 pada _root namespace_. Meskipun demikian, isolasi ini bersifat searah; proses di dalam _nested namespace_ tidak memiliki visibilitas maupun kemampuan untuk berinteraksi dengan proses di tingkat yang lebih tinggi (_parent namespace_).

Secara teknis, pembuatan PID Namespace dilakukan melalui sistem panggilan `clone()` dengan flag `CLONE_NEWPID`. Salah satu karakteristik unik dari PID Namespace dibandingkan jenis namespace lainnya adalah ketergantungannya pada mekanisme _forking_. Perintah `unshare` memerlukan parameter `--fork` agar proses baru dapat menempati posisi PID 1 di namespace tersebut. Selain itu, untuk mencapai isolasi visual yang sempurna, sistem memerlukan sinkronisasi dengan _Mount Namespace_ guna memasang (_mounting_) ulang sistem berkas virtual `/proc`. Tanpa langkah ini, alat pemantau sistem seperti `ps` akan tetap menampilkan daftar proses dari _host_ utama karena masih membaca data dari `procfs` milik _root namespace_.

## Ringkasan

- **Isolasi Hierarkis:** PID Namespace membagi satu pohon proses global menjadi beberapa sub-pohon yang terisolasi.
- **Virtualisasi PID 1:** Memungkinkan proses di dalam namespace memiliki PID 1 dan berperan sebagai _init process_ bagi anak-anak proses di bawahnya.
- **Identitas Ganda (Multi-PID):** Sebuah proses dapat memiliki ID berbeda di setiap level namespace yang ditempatinya (dapat diperiksa melalui `/proc/[PID]/status` pada baris `NSpid`).
- **Keterbatasan Interaksi:** Proses di dalam _nested namespace_ hanya dapat melihat dan berinteraksi dengan proses di dalam namespace yang sama atau di bawahnya, namun tidak ke atas (_parent_).
- **Ketergantungan pada `/proc`:** Isolasi PID tidak akan terlihat oleh _userspace tools_ (seperti `ps` atau `top`) kecuali direktori `/proc` dipasang ulang (_remount_) secara khusus untuk namespace tersebut.
- **Mekanisme Lifecycle:** Jika proses dengan PID 1 di dalam suatu namespace berhenti/mati, maka kernel akan menghentikan seluruh proses lain di dalam namespace tersebut secara otomatis.

---

# NET Namespaces

Linux Network Namespace adalah mekanisme isolasi pada level kernel yang memungkinkan sekumpulan proses memiliki _network stack_ yang sepenuhnya independen dari sistem _host_. Isolasi ini mencakup seluruh aspek jaringan, mulai dari antarmuka fisik/virtual (_interfaces_), tabel routing, aturan _firewall_ (iptables/nftables), hingga soket jaringan. Saat sebuah namespace jaringan baru diciptakan, ia berada dalam kondisi "steril"—hanya memiliki antarmuka _loopback_ (`lo`) dalam status non-aktif, sehingga proses di dalamnya secara _default_ tidak memiliki konektivitas sama sekali dengan dunia luar maupun dengan _host_.

Untuk membangun komunikasi antar-namespace yang terisolasi, Linux menggunakan perangkat virtual yang disebut **Veth Pair** (_Virtual Ethernet Pair_). Mekanisme ini bekerja layaknya kabel jaringan virtual yang menghubungkan dua ujung antarmuka; apa yang dikirimkan pada satu ujung akan diterima di ujung lainnya. Dengan menempatkan satu ujung di _root namespace_ dan ujung lainnya di dalam _target namespace_, administrator dapat menjembatani isolasi tersebut. Agar komunikasi dapat berjalan, setiap antarmuka pada kedua ujung "kabel" ini harus diberikan alamat IP yang berada dalam segmen jaringan yang sama dan diaktifkan secara manual.

Dalam skenario yang lebih kompleks, seperti simulasi Jaringan Area Lokal (LAN) untuk banyak kontainer, Linux memanfaatkan entitas **Bridge**. Bridge berfungsi sebagai _virtual switch_ pada Layer 2 yang menghubungkan berbagai _veth pairs_ dari berbagai namespace ke dalam satu _broadcast domain_ yang sama. Agar namespace tersebut dapat menjangkau jaringan luar (Internet), bridge harus dikonfigurasi sebagai _default gateway_. Namun, karena jaringan internal ini bersifat privat dan tidak dikenali oleh jaringan fisik di luar _host_, diperlukan penerapan aturan **IP Masquerade (NAT)** melalui iptables. Hal ini memastikan paket data yang keluar dari namespace menggunakan alamat IP publik milik _host_ agar dapat menerima paket balasan dari jaringan eksternal.

### **Poin-Poin Ringkasan (Key Takeaways)**

- **Isolasi Stack Total:** Setiap namespace memiliki salinan mandiri dari _routing table_, _interface list_, dan aturan _firewall_.
- **Veth Pair (Virtual Cable):** Alat utama untuk menghubungkan dua namespace secara _point-to-point_.
- **Linux Bridge:** Bertindak sebagai _switch_ virtual untuk menghubungkan lebih dari dua namespace ke dalam satu jaringan LAN virtual.
- **Konektivitas Eksternal:** Membutuhkan pengaturan _Default Gateway_ di dalam namespace yang mengarah ke alamat IP Bridge pada _host_.
- **IP Masquerade/NAT:** Syarat mutlak bagi namespace untuk mengakses internet agar alamat IP privat internal dapat ditranslasikan menjadi alamat IP _host_ yang valid di jaringan luar.
- **Simulasi Jaringan:** NET Namespace memungkinkan pengujian topologi jaringan yang kompleks di dalam satu mesin fisik tanpa mengganggu konfigurasi jaringan utama.

---
