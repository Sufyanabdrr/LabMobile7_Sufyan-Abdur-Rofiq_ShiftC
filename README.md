# TUGAS 7 PRAKTIKUM PEMROGRAMAN MOBILE PERTEMUAN 8
Nama : Sufyan Abdur Rofiq

NIM : H1D022004

Shift Lama : B

Shift Baru : C

# A. SCREENSHOT TAMPILAN IONIC :

1). Halaman Login :

![image](https://github.com/user-attachments/assets/4ed6a1bd-30f7-488c-a972-ca7ae1225735)

![image](https://github.com/user-attachments/assets/ded0ff32-174b-4823-9c07-b073d4b51629)

2). Halaman Home :

![image](https://github.com/user-attachments/assets/5c56e671-daca-47b3-a3ef-dab617033b4e)

# B. Penjelasan Langkah untuk Proses Login :

# 1. Menyiapkan Database dan API

Database: Buat basis data yang akan menyimpan informasi pengguna, seperti username, email, dan password. Kamu bisa menggunakan tools seperti Laragon untuk setup database lokal.
API: Buat API dengan PHP untuk mengelola autentikasi. Setidaknya dua file diperlukan:
- koneksi.php: Menghubungkan aplikasi Ionic dengan database. File ini bertanggung jawab untuk koneksi antara aplikasi dan database, memastikan aplikasi bisa mengakses data di database.
- login.php: Menangani proses DDL (Data Definition Language) dan DML (Data Manipulation Language), seperti pembuatan pengguna baru dan validasi data login dari pengguna.

# 2. Membuat Project Ionic
Gunakan perintah berikut untuk membuat project Ionic baru:

- ionic start cobalogin blank --type=angular
  
Pilih framework Angular dan template "blank" untuk memulai aplikasi kosong.

# 3. Mengatur Preferences dengan Capacitor
Di direktori proyek, instal modul Capacitor Preferences untuk menyimpan data autentikasi secara lokal di perangkat pengguna:

- npm i @capacitor/preferences
  
# 4. Menambahkan HttpClient ke Project

Buka file app.module.ts dan daftarkan provideHttpClient. Ini memungkinkan aplikasi melakukan komunikasi HTTP (GET, POST, PUT, DELETE) ke API. Berikut adalah contoh cara menambahkan HttpClient ke provider:

- import { HttpClientModule } from '@angular/common/http';

@NgModule({
  imports: [HttpClientModule],
  ...
})
export class AppModule {}

# 5. Membuat Service Authentication

Buat service dengan perintah:

- ionic g service services/authentication

Di dalam file authentication.service.ts, tambahkan fungsi untuk login, menyimpan token, dan memuat data autentikasi:

- saveData: Menyimpan token dan username di perangkat pengguna.
- loadData: Memuat data autentikasi yang tersimpan untuk mengecek status login saat aplikasi dijalankan.
- clearData dan logout: Menghapus data autentikasi yang tersimpan dan mengatur ulang status login.

Contoh fungsi login yang mengirim permintaan ke API:

- login(username: string, password: string) {
  return this.http.post(`${apiUrl}/login.php`, { username, password });

# 6. Membuat AuthGuard dan AutoLoginGuard
   
AuthGuard: Mencegah akses ke halaman tertentu jika pengguna belum login.

- canActivate(): boolean {
  if (this.authService.isAuthenticated()) {
    return true;
  } else {
    this.router.navigate(['/login']);
    return false;
  }

AutoLoginGuard: Mengarahkan pengguna yang sudah login ke halaman beranda tanpa perlu mengakses halaman login lagi.

- canActivate(): boolean {
  if (this.authService.isAuthenticated()) {
    this.router.navigate(['/home']);
    return false;
  } else {
    return true;
  }

# 7. Menambahkan Guards ke Routing
   
Di file app-routing.module.ts, tambahkan guards ke rute. Sebagai contoh:

- const routes: Routes = [
  { path: '', redirectTo: '/login', pathMatch: 'full' },
  { path: 'login', component: LoginPage, canActivate: [AutoLoginGuard] },
  { path: 'home', component: HomePage, canActivate: [AuthGuard] }

# 8. Membuat Login Form dan Implementasi Logika Login
   
Form Login: Pada login.page.html, tambahkan form untuk username dan password menggunakan komponen ion-input.
Di file login.page.ts, buat fungsi login() untuk memproses data input pengguna dan mengirimkannya ke API menggunakan AuthService.

- login() {
  this.authService.login(this.username, this.password).subscribe(response => {
    if (response.status_login === 'berhasil') {
      this.router.navigate(['/home']);
    } else {
      // Tampilkan notifikasi kesalahan
    }
  });

# 9. Halaman Beranda dan Logout

Di home.page.ts, buat fungsi logout() yang akan menghapus data autentikasi dan mengarahkan pengguna kembali ke halaman login.

- logout() {
  this.authService.logout();
  this.router.navigate(['/login']);

# 10. Ringkasan Proses Login dan Akses Rute

- Login: Pengguna memasukkan username dan password di form login, kemudian AuthService memvalidasi data tersebut dengan API. Jika berhasil, data autentikasi disimpan dan pengguna diarahkan ke halaman beranda.
- Akses Rute: AuthGuard memastikan hanya pengguna yang terautentikasi yang bisa mengakses halaman beranda, sementara AutoLoginGuard memastikan pengguna yang sudah login tidak dapat kembali ke halaman login.
Dengan langkah-langkah ini, proses login yang aman dan terstruktur telah diimplementasikan pada aplikasi Ionic.
