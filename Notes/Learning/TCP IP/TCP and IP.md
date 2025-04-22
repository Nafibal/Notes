# Transmission Control Protocol (TCP)

**Transmission Control Protocol (TCP)**  adalah sebuah standar komunikasi yang memungkinkan aplikasi dan perangkat komputer untuk bertukar pesan (*message*) dalam sebuah jaringan (*network*). TCP didesain untuk mengirimkan sebuah paket melalui internet dan memastikan keberhasilan pengiriman tersebut.

# Internet Protocol (IP)

**Internet Protocol (IP)** merupakan sebuah cara/metode untuk mengirimkan data dari satu perangkat ke perangkat yang lain melalui internet. IP bertugas untuk mendefinisikan bagaimana cara aplikasi dan perangkat menukarkan paket data antara satu sama lain. Setiap perangkat memiliki alamat IP unik yang memungkinkan mereka untuk berkomunikasi dan bertukar data dengan perangkat lain yang juga terhubung pada internet.

# Perbedaan TCP dan IP

**TCP** dan **IP** merupakan dua protokol berbeda yang bekerjasama untuk memastikan data terkirim ke tempat tujuannya dalam sebuah jaringan. Perbedaan fungsi dari kedua protokol tersebut adalah:
- **IP** berfungsi untuk mendapatkan dan menetapkan alamat IP dari aplikasi/perangkat dimana data harus dikirimkan
- **TCP** berfungsi untuk mengirimkan data dan memastikan data telah dikirimkan ke tujuan yang tepat sesuai dengan 

# TCP Layers

| Layers      | Examples           |
| ----------- | ------------------ |
| Application | HTTP, FTP, SMTP    |
| Transport   | TCP, UDP           |
| Network     | IP, Routers        |
| Data Link   | Ethernet, Switches |
| Physical    | Cables, NIC        |
![[Pasted image 20240419164735.png]]

# Resources

[TCP/IP Model Explained | Cisco CCNA 200-301 (youtube.com)](https://www.youtube.com/watch?v=OTwp3xtd4dg)
[What is TCP/IP in Networking? | Fortinet](https://www.fortinet.com/resources/cyberglossary/tcp-ip)
[What is Transmission Control Protocol (TCP)? | Definition from TechTarget](https://www.techtarget.com/searchnetworking/definition/TCP)
### OSI Layer

| Layers           | Examples                                 | Functions                                                      |
| ---------------- | ---------------------------------------- | -------------------------------------------------------------- |
| Application (7)  | SMTP, FTP, Telnet                        | Where the application and user communicate                     |
| Presentation (6) | Format Data, Encryption                  | Formats data in a way the receiving application can understand |
| Session (5)      | Start & Stop Session                     | Establishinf and terminating connections between devices       |
| Transport (4)    | TCP. UDP, Port Numbers                   | Add the transfer protocol                                      |
| Network (3)      | IP Address, Routers                      | Handles IP addressing and routing                              |
| Data Link (2)    | MAC Address, Switches                    | Physical addresses are added to the data                       |
| Physical (1)     | Cable, Network Interface Cards, Hubs<br> | Carry data through physical hardwares                          |
