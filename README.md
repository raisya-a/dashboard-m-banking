# dashboard-m-banking

1. Deskripsi Project: Proyek E-Wallet merupakan sebuah aplikasi dompet digital yang dirancang untuk memudahkan pengguna dalam melakukan transaksi keuangan secara elektronik. Sistem ini memungkinkan pengguna untuk menyimpan saldo, melakukan pengisian saldo (top-up), transfer dana, serta melihat riwayat transaksi dengan lebih cepat dan praktis dibandingkan penggunaan uang tunai.

2. Anggota Kelompok:
- Fahrul Firmansyah (25051204128) 
- Pandji Laras (25051204131) 
- Muhammad Hisyam (25051204169) 
- Raisya Azelia Huwaida (25051204198)

3. Fitur Utama: Login Akun, Top up saldo, Transfer saldo, Cek saldo, Cek History

4. Cara Menjalankan Project:
1) Buka Vscode
2) Open folder project : ProjectAkhir Ewallet
3) Run code
4) Install : pip install -r requirements.txt
5) Jalankan server : python manage.py runserver
6) Copy http://127.0.0.1:8000/ yang muncul setelah menjalankan server
7) Paste http://127.0.0.1:8000/ di browser
8) Dashboard sudah bisa dijalankan
9) Akun yang tersedia :
- Role Admin :
  user: admin
  password: admin 123
- Role User :
  user: user
  password: user123

5. Penjelasan Implementasi OOP:
- Enkapsulasi: enkapsulasi pada program diterapkan dengan membungkus data sensitif seperti saldo, nomor akun, dan riwayat transaksi ke dalam kelas model tertentu (misalnya kelas Wallet di models.py), di mana data tersebut tidak boleh diubah secara sembarangan dari luar tanpa melalui metode validasi yang aman. Segala perubahan saldo atau pembaruan status akun wajib melewati fungsi internal yang telah ditentukan di dalam kelas tersebut atau melalui berkas services.py guna menjaga integritas dan keamanan data finansial pengguna.
- Inheritance: ineritance digunakan secara intensif di seluruh struktur Django, di mana kelas-kelas yang dibuat di models.py (seperti kelas pengguna atau kelas transaksi) mewarisi (inherit) properti dan fungsi dari kelas induk bawaan Django yaitu models.Model. Dengan memanfaatkan konsep ini, tidak perlu lagi menulis ulang kode dasar untuk terhubung ke database, melakukan pencarian data, atau menyimpan riwayat transaksi baru, karena semua kemampuan database tersebut otomatis diwarisi oleh kelas anak yang telah dibuat.
- Polimorfisme: polimorfisme terjadi ketika berbagai objek dalam aplikasi E-Wallet merespons perintah yang sama dengan cara atau hasil yang berbeda sesuai dengan karakteristiknya masing-masing. Sebagai contoh, fungsi standar __str__() atau metode pemrosesan transaksi dapat diterapkan pada kelas TransferTransaction maupun TopUpTransaction; meskipun nama fungsinya sama, sistem akan menghasilkan format teks atau logika perhitungan yang berbeda secara otomatis tergantung pada jenis objek transaksi yang sedang diproses.
- Abstraksi: abstraksi pada program diimplementasikan melalui penggunaan Service Layer pada berkas services.py, yang berfungsi menyembunyikan detail logika pemrograman yang rumit dan hanya menyediakan fungsi sederhana untuk digunakan oleh bagian lain. Ketika pengguna melakukan transfer, bagian tampilan halaman web (views.py) hanya perlu memanggil satu baris fungsi abstrak seperti perintah transfer dana, tanpa perlu mengetahui rumitnya proses pengecekan kecukupan saldo, penguncian database, hingga kalkulasi pengurangan dan penambahan saldo di balik layar.

6. Screenshot Tampilan Program:
- Dashboard User
<img width="1600" height="795" alt="WhatsApp Image 2026-06-03 at 21 17 40" src="https://github.com/user-attachments/assets/ed0397eb-29e9-4641-875e-12771caf286d" />

- Kartu Wallet Virtual
<img width="1600" height="792" alt="WhatsApp Image 2026-06-03 at 21 17 40 (1)" src="https://github.com/user-attachments/assets/b9a34399-61e3-47d0-9b93-70094b48e618" />

- Transfer Dana
<img width="1600" height="795" alt="WhatsApp Image 2026-06-03 at 21 17 41" src="https://github.com/user-attachments/assets/52dbbfae-9f23-416a-af1b-124e172593be" />

- Top Up Saldo
<img width="1600" height="792" alt="WhatsApp Image 2026-06-03 at 21 17 41 (1)" src="https://github.com/user-attachments/assets/1ca6f70b-9b90-46a1-9211-a341215ec8e4" />

- Riwayat Transfer
<img width="1600" height="787" alt="WhatsApp Image 2026-06-03 at 21 17 41 (2)" src="https://github.com/user-attachments/assets/3804816d-17ac-48f9-a9d2-d53bdb489f19" />

- Notifikasi
<img width="1600" height="799" alt="WhatsApp Image 2026-06-03 at 21 17 42" src="https://github.com/user-attachments/assets/197349d7-e7db-438a-9f25-e9a2b60d2f5e" />

- Report (User)
<img width="1600" height="774" alt="WhatsApp Image 2026-06-03 at 21 17 42 (1)" src="https://github.com/user-attachments/assets/b8c7737f-cb51-474d-83b6-dee5597983ec" />

- Halaman Login
<img width="1600" height="780" alt="WhatsApp Image 2026-06-03 at 21 17 42 (3)" src="https://github.com/user-attachments/assets/db4b11b8-41d4-4e98-ad4c-f802b649fbf7" />

- Daftar Akun User
<img width="1600" height="777" alt="WhatsApp Image 2026-06-03 at 21 17 43" src="https://github.com/user-attachments/assets/a2f9f867-2e3c-46a5-b842-96f939b76209" />

- Dashboard Admin
<img width="1600" height="795" alt="WhatsApp Image 2026-06-03 at 21 17 43 (1)" src="https://github.com/user-attachments/assets/c73aaaee-1bc0-47c8-8e51-6f3c164c2f1b" />

- Data User
<img width="1600" height="797" alt="WhatsApp Image 2026-06-03 at 21 17 43 (2)" src="https://github.com/user-attachments/assets/6bafbbb7-1dc9-4973-866f-8a5862a2b19d" />

- Data Admin
<img width="1600" height="798" alt="WhatsApp Image 2026-06-03 at 21 17 44" src="https://github.com/user-attachments/assets/046359e0-f992-4492-a433-ee0ea1235e6c" />

- Aktivitas Login/Logout
<img width="1600" height="799" alt="WhatsApp Image 2026-06-03 at 21 17 44 (1)" src="https://github.com/user-attachments/assets/bda5edc7-e578-4cdd-97c3-0c7ab6e0475b" />

- History Transaksi User
<img width="1600" height="796" alt="WhatsApp Image 2026-06-03 at 21 17 44 (2)" src="https://github.com/user-attachments/assets/fe219dc5-8c5b-40f7-90c4-ae174ec201f5" />

- Top Up Pending
<img width="1600" height="797" alt="WhatsApp Image 2026-06-03 at 21 17 45" src="https://github.com/user-attachments/assets/5b29c70d-a193-4f84-9b65-ff2d076a6c2f" />

- Report User
<img width="1600" height="796" alt="WhatsApp Image 2026-06-03 at 21 17 45 (1)" src="https://github.com/user-attachments/assets/dadda303-ae66-4a02-856b-b54696ddfb27" />

- Tambah Akun User
<img width="1600" height="781" alt="WhatsApp Image 2026-06-03 at 21 17 45 (2)" src="https://github.com/user-attachments/assets/2a5af775-6eca-4b48-b0ea-cbfd303519b5" />

- Tambah Akun Admin
<img width="1600" height="789" alt="WhatsApp Image 2026-06-03 at 21 17 54" src="https://github.com/user-attachments/assets/2557ef60-d81a-4d48-a846-40dd782b2a02" />
