**Tags:**
#programming #programming-note #programming-concept #go #go-package 

---
# Terdapat 2 komponen awal:
## http.Server 
Melakukan listening port dan menerima koneksi TCP
## ServeMux 
Berfungsi sebagai router. Mencocokkan url yang diminta (contoh: "/user") dengan fungsi yang bertugas menanganinya

# Interface http.Handler
hampir semua hal di Go http adalah sebuah handler.
handler adalah sebuah inteface dengan bentuk
```go
type Handler interface {
    ServeHTTP(ResponseWriter, *Request)
}
```

semua hal yang memiliki method ServeHTTP dengan parameter di atas adalah sebuah `Handler`

# ServeHTTP
Saat ada request masuk, `ServeHTTP` dipanggil dengan dua parameter:
## *http.Request*
`http.Request` adalah objek yang berisi data client (request):
- http method
- URL & query param
- Header
- Body
## http.ResponseWriter
`http.ResponseWriter` adalah sebuah interface yang berfungsi membuat response untuk client:
- **`Header()`**: Untuk memanipulasi header sebelum dikirim.
- **`WriteHeader(int)`**: Untuk mengirim status code (misal 200, 404).
- **`Write([]byte)`**: Untuk menulis data body ke koneksi klien.

# Middleware
Dalam go, middleware adalah sebuah fungsi yang menerima `http.Handler` dan mengembalikan `http.Handler` baru.

contoh alur:
Request → Middleware 1 → Middleware 2 → Actual Handler → Response
```go
func Logger(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        // 1. Lakukan sesuatu sebelum handler (misal catat waktu)
        log.Println("Ada request masuk")
        
        // 2. Oper ke handler berikutnya
        next.ServeHTTP(w, r)
        
        // 3. Lakukan sesuatu setelah handler (misal catat durasi)
    })
}
```