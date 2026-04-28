- [x] Create Vitest config for  backend
- [x] assign category to create student endpoint
- [x] Add Security Best Practice
## Endpoint
- [x] CRUD for students 
- [ ] CRUD for admin
- [ ] endpoint for auth
- [ ] CRUD for tuition categories (admin)
- [ ] CRUD for tuition categories pricing (admin)
- [ ] CRUD for invoice (admin)
- [ ] Endpoint for payment proof

## Testing
- [ ] Update Unit Test for student CRUD

## CSRF
- [ ] Testing CSRF di production env (di dev dimatikan)
- [ ] set x-csrf-token header di swagger
- [ ] set @ApiSecurity('CSRF-Token') di masing2 controller

# Features
- [ ] Add tahun ajaran untuk santri
- [ ] tambahkan status dari santri (aktif, tidak aktif, lulus)
- [ ] tambahkan field batas waktu bayar pada invoice
- [ ] tambah table invoice period pada module invoice
- [ ] di dashboard waktu diganti jam
- [ ] Search Santri by name dan NIS
- [ ] Kasih template import data
- [ ] tambah record data santri pada invoice
- [ ] tambah keterangan perlu upload ulang bukti pada invoice dengan status rejected
- [ ] Kasih batas ukuran upload

# Frontend
- [x] Di halaman detail santri, ditambahkan data histori tagihan spp
- [x] di halaman detail kategori, ditambahkan list santri yang masuk kategori itu
- [ ] Ganti wording "Lihat Bukti Pembayaran" menjadi "Lihat Pembayaran"
- [ ] di dashboard status pembayaran di kelompokkan by periode + tambah filter periode
- [x] Dashboard di verifikasi pembayaran per row ditambahkan link ke detail, ganti aksi verifikasi dan reject ke lihat detail