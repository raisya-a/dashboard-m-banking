# dashboard-m-banking

1. Deskripsi Project: Proyek E-Wallet merupakan sebuah aplikasi dompet digital yang dirancang untuk memudahkan pengguna dalam melakukan transaksi keuangan secara elektronik. Sistem ini memungkinkan pengguna untuk menyimpan saldo, melakukan pengisian saldo (top-up), transfer dana, serta melihat riwayat transaksi dengan lebih cepat dan praktis dibandingkan penggunaan uang tunai.

2. Anggota Kelompok:
- Fahrul Firmansyah (25051204128) 
- Pandji Laras (25051204131) 
- Muhammad Hisyam (25051204169) 
- Raisya Azelia Huwaida (25051204198)

3. Fitur Utama: Login Akun, Top up saldo, Transfer saldo, Cek saldo, Cek History

4. Cara Menjalankan Project:
1) User Membuat Akun: User akan diminta mengisi data yaitu nama lengkap, no hp, email, username, Password akun, dan pin, 
2) User Login: Setelah user selesai membuat akun, maka user sudah bisa login. (akun dasar: (user: admin
password: admin123), (user: user password: user123)
3) Jika user ingin melakukan transfer:
- Klik tombol transfer 
- Masukkan email/username penerima 
- Ketik nominal 
- Masukkan pin 
- Klik lanjutkan transaksi 
4) Jika user ingin melakukan top up:
- Klik tombol top up 
- Masukkan nominal top up 
- Pilih metode pembayaran 
- Klik ajukan top up 
5) Jika user ingin melihat riwayat riwayat interaksi/penggunaan:
- Klik History untuk melihat riwayat transfer 
- Klik notifikasi untuk melihat notifikasi yg muncul 
- Klik Aktifitas Login untuk melihat riwayat login/logout 
- Klik Transaksi untuk melihat riwayat transaksi

5. Penjelasan Implementasi OOP:
- Enkapsulasi: enkapsulasi pada program diterapkan dengan membungkus data sensitif seperti saldo, nomor akun, dan riwayat transaksi ke dalam kelas model tertentu (misalnya kelas Wallet di models.py), di mana data tersebut tidak boleh diubah secara sembarangan dari luar tanpa melalui metode validasi yang aman. Segala perubahan saldo atau pembaruan status akun wajib melewati fungsi internal yang telah ditentukan di dalam kelas tersebut atau melalui berkas services.py guna menjaga integritas dan keamanan data finansial pengguna.
- Inheritance: ineritance digunakan secara intensif di seluruh struktur Django, di mana kelas-kelas yang dibuat di models.py (seperti kelas pengguna atau kelas transaksi) mewarisi (inherit) properti dan fungsi dari kelas induk bawaan Django yaitu models.Model. Dengan memanfaatkan konsep ini, tidak perlu lagi menulis ulang kode dasar untuk terhubung ke database, melakukan pencarian data, atau menyimpan riwayat transaksi baru, karena semua kemampuan database tersebut otomatis diwarisi oleh kelas anak yang telah dibuat.
- Polimorfisme: polimorfisme terjadi ketika berbagai objek dalam aplikasi E-Wallet merespons perintah yang sama dengan cara atau hasil yang berbeda sesuai dengan karakteristiknya masing-masing. Sebagai contoh, fungsi standar __str__() atau metode pemrosesan transaksi dapat diterapkan pada kelas TransferTransaction maupun TopUpTransaction; meskipun nama fungsinya sama, sistem akan menghasilkan format teks atau logika perhitungan yang berbeda secara otomatis tergantung pada jenis objek transaksi yang sedang diproses.
- Abstraksi: abstraksi pada program diimplementasikan melalui penggunaan Service Layer pada berkas services.py, yang berfungsi menyembunyikan detail logika pemrograman yang rumit dan hanya menyediakan fungsi sederhana untuk digunakan oleh bagian lain. Ketika pengguna melakukan transfer, bagian tampilan halaman web (views.py) hanya perlu memanggil satu baris fungsi abstrak seperti perintah transfer dana, tanpa perlu mengetahui rumitnya proses pengecekan kecukupan saldo, penguncian database, hingga kalkulasi pengurangan dan penambahan saldo di balik layar.

6. Screenshot Tampilan Program:
<img width="1600" height="795" alt="WhatsApp Image 2026-06-03 at 21 17 40" src="https://github.com/user-attachments/assets/ed0397eb-29e9-4641-875e-12771caf286d" />
