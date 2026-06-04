# dashboard-m-banking

1. Deskripsi Project: Proyek E-Wallet merupakan sebuah aplikasi dompet digital yang dirancang untuk memudahkan pengguna dalam melakukan transaksi keuangan secara elektronik. Sistem ini memungkinkan pengguna untuk menyimpan saldo, melakukan pengisian saldo (top-up), transfer dana, serta melihat riwayat transaksi dengan lebih cepat dan praktis dibandingkan penggunaan uang tunai.

2. Anggota Kelompok:
- Fahrul Firmansyah (25051204128) 
- Pandji Laras (25051204131) 
- Muhammad Hisyam (25051204169) 
- Raisya Azelia Huwaida (25051204198)

3. Fitur Utama:
- Login akun 
- Membuat akun 
- Fitur User : Top up saldo, Transfer saldo, Cek saldo, Cek History.
- Fitur Admin : Melihat data user, Mengedit data user, Memblokir user, Melihat data admin, Mengedit data admin, Menambahkan akun admin baru, Melihat aktivitas login, Melihat history transaksi user, Melakukan search dengan filter, Search user berdasarkan username/email, Menyetujui dan menolak top up user.

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
- Role Admin : (user: admin, password: admin 123)
- Role User : (user: user, password: user123)

5. Penjelasan Implementasi OOP:  
1.Inheritance / Pewarisan
Inheritance adalah konsep ketika sebuah class mewarisi sifat atau fungsi dari class lain. Pada project ini, inheritance diterapkan di beberapa bagian.

Contoh 1: Custom User

File:
accounts/models.py

Class User mewarisi AbstractUser dari Django.

Kode:  
class User(AbstractUser):

Artinya:
- User tetap memiliki fitur bawaan Django seperti username, password, login, dan authentication.
- Lalu ditambahkan field khusus aplikasi e-wallet seperti full_name, phone, pin, role, balance, status, blocked_reason, dan blocked_at.

Jadi, class User mewarisi fitur bawaan Django, lalu dikembangkan sesuai kebutuhan aplikasi.


Contoh 2: Form Django

File:  
accounts/forms.py  
wallet/forms.py  
reports/forms.py  

Beberapa form mewarisi class bawaan Django.

Contoh:  
class RegisterForm(forms.ModelForm):  
class LoginForm(forms.Form):  
class TransferForm(forms.Form):  
class TopUpForm(forms.Form):  
class ReportForm(forms.Form):  

Artinya:  
- Form project mewarisi kemampuan validasi dari Django Form.
- Kita tinggal menambahkan field dan aturan validasi sendiri.
- Contohnya validasi password, konfirmasi password, PIN, nominal transfer, dan data user.


Contoh 3: Class Transaksi

File:  
wallet/transaction_types.py  
wallet/abstractions.py  

Class transaksi seperti TopUpTransaction, TransferInTransaction, dan TransferOutTransaction mewarisi BaseTransaction.

Contoh:  
class TopUpTransaction(BaseTransaction):  
class TransferInTransaction(BaseTransaction):  
class TransferOutTransaction(BaseTransaction):  

Artinya:
- Semua transaksi mewarisi atribut dan method dasar dari BaseTransaction.
- Contohnya user, amount, target_user, description, dan generate_code().
- Setiap class transaksi tinggal membuat prosesnya masing-masing melalui method process().


Kesimpulan Inheritance: Inheritance diterapkan agar class baru bisa memakai fitur class induk, sehingga kode lebih rapi dan tidak perlu menulis ulang fungsi yang sama.


2. Encapsulation / Enkapsulasi
Encapsulation adalah konsep membungkus data dan proses di dalam class, sehingga data tidak diubah sembarangan dari luar. Pada project ini, encapsulation diterapkan pada service layer.

Contoh utama:

File:  
wallet/services.py  

Class:  
WalletService  

Kode:  
class WalletService:  
    def __init__(self, user):  
        self.__user = user  

    def get_balance(self):
        return self.__user.balance

    def increase_balance(self, amount):
        self.__user.balance += Decimal(amount)
        self.__user.save(update_fields=["balance"])

    def decrease_balance(self, amount):
        amount = Decimal(amount)
        if self.__user.balance < amount:
            raise ValueError("Saldo tidak mencukupi.")
        self.__user.balance -= amount
        self.__user.save(update_fields=["balance"])

Penjelasan:
- Data user disimpan dalam atribut private __user.
- Saldo tidak diubah langsung dari view.
- Perubahan saldo dilakukan melalui method increase_balance() dan decrease_balance().
- Jika saldo tidak cukup, method decrease_balance() akan menolak transaksi.

Contoh penggunaan:  
TransferService tidak langsung mengubah saldo dengan cara asal-asalan.  
TransferService memanggil:  

WalletService(sender).decrease_balance(amount)  
WalletService(receiver).increase_balance(amount)  

Artinya:  
- Saldo pengirim dikurangi lewat method khusus.
- Saldo penerima ditambah lewat method khusus.
- Logic validasi saldo tetap aman di dalam class.

Kesimpulan Encapsulation: Encapsulation diterapkan agar data penting seperti saldo wallet tidak diubah langsung dari sembarang tempat, tetapi melalui method yang sudah memiliki aturan validasi.


3. Abstraction / Abstraksi
Abstraction adalah konsep menyembunyikan detail proses dan hanya menampilkan fungsi penting yang perlu dipakai. Pada project ini, abstraction diterapkan pada class transaksi.

File:  
wallet/abstractions.py  

Kode:  
from abc import ABC, abstractmethod

class TransactionInterface(ABC):
    @abstractmethod
    def process(self):
        pass

Penjelasan:
- TransactionInterface adalah abstract class.
- Method process() dibuat sebagai abstract method.
- Artinya setiap class transaksi wajib memiliki method process().
- Detail isi process() berbeda-beda tergantung jenis transaksi.

Class BaseTransaction juga menjadi dasar transaksi.

Kode:  
class BaseTransaction(TransactionInterface):  
    prefix = "TRX"  

    def __init__(self, user, amount, target_user=None, description=""):
        self.user = user
        self.amount = amount
        self.target_user = target_user
        self.description = description

    def generate_code(self):
        timestamp = timezone.now().strftime("%Y%m%d%H%M%S%f")
        random_number = random.randint(100, 999)
        return f"{self.prefix}-{timestamp}{random_number}"

Penjelasan:
- BaseTransaction menyimpan data dasar transaksi.
- BaseTransaction menyediakan method generate_code().
- Setiap transaksi tidak perlu membuat kode transaksi dari nol.
- Yang wajib diatur oleh class turunan hanyalah process().

Contoh:  
File:  
wallet/transaction_types.py  

Class:  
TopUpTransaction  
TransferInTransaction  
TransferOutTransaction  

Masing-masing class memiliki method process().

Kesimpulan Abstraction: Abstraction diterapkan agar view/service cukup memanggil method process(), tanpa perlu tahu detail bagaimana transaksi top up, transfer masuk, atau transfer keluar disimpan ke database.


4. Polymorphism / Polimorfisme
Polymorphism adalah konsep ketika beberapa class memiliki method yang sama, tetapi isi dan hasilnya berbeda. Pada project ini, polymorphism diterapkan pada method process() di class transaksi.

File:  
wallet/transaction_types.py  

Class:  
TopUpTransaction  
TransferInTransaction  
TransferOutTransaction  

Ketiganya sama-sama memiliki method:  

process()  

Tetapi isi prosesnya berbeda.

Contoh 1:  
TopUpTransaction.process()

Fungsi:
- Membuat data transaksi top up.
- Jenis transaksi adalah topup.
- Deskripsi transaksi adalah top up saldo berhasil.

Contoh 2:  
TransferOutTransaction.process()  

Fungsi:
- Membuat data transaksi keluar.
- Jenis transaksi adalah transfer_out.
- Digunakan untuk pengirim.
- Saldo setelah transaksi adalah saldo pengirim setelah dikurangi.

Contoh 3:  
TransferInTransaction.process()  

Fungsi:
- Membuat data transaksi masuk.
- Jenis transaksi adalah transfer_in.
- Digunakan untuk penerima.
- Saldo setelah transaksi adalah saldo penerima setelah bertambah.

Walaupun method yang dipanggil sama, yaitu process(), hasilnya berbeda sesuai object transaksi.

Contoh konsep:  
transactions = [
    TopUpTransaction(user, 50000),
    TransferOutTransaction(sender, 25000, receiver),
    TransferInTransaction(receiver, 25000, sender),
]

for transaction in transactions:
    transaction.process()

Penjelasan:
- Semua object dipanggil dengan method yang sama, yaitu process().
- Tetapi setiap object menjalankan proses berbeda sesuai class masing-masing.

Kesimpulan Polymorphism: Polymorphism diterapkan agar beberapa jenis transaksi bisa diproses dengan nama method yang sama, tetapi menghasilkan data transaksi yang berbeda sesuai jenisnya.


KESIMPULAN AKHIR

Project e-wallet ini menerapkan 4 konsep utama OOP:

1. Inheritance
Diterapkan pada:
- User yang mewarisi AbstractUser
- Form yang mewarisi forms.Form atau forms.ModelForm
- Class transaksi yang mewarisi BaseTransaction

2. Encapsulation
Diterapkan pada:
- WalletService
- Saldo user tidak diubah langsung dari view
- Perubahan saldo dilakukan lewat increase_balance() dan decrease_balance()

3. Abstraction
Diterapkan pada:
- TransactionInterface
- BaseTransaction
- Method abstract process()
- Service/view cukup memanggil process() tanpa mengetahui detail transaksi

4. Polymorphism
Diterapkan pada:
- TopUpTransaction.process()
- TransferInTransaction.process()
- TransferOutTransaction.process()
- Method sama, tetapi isi proses berbeda

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
