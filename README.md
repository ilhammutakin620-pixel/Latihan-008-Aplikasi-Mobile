# Integrasi Firebase Authentication - Flutter App
> **Tugas Praktikum Pertemuan 8 - Pemrograman Mobile Lanjutan**  
> Dosen Pengampu: **IKetut Gunawan**  
> Program Studi: **Teknik Informatika (TI 24 P SE 1)** - Institute Technology and Business Binasarana Global

---

## 👤 Informasi Mahasiswa / Tim
* **Nama**: Muhammad Ilham Muttaqim
* **NIM**: `1124160073`
* **Kelas**: `TI 24 P SE 1`

---

## 📋 Pembagian Tugas Tim (Team Task List)

| No | Anggota Tim | Peran | Rincian Tugas yang Dikerjakan | Status |
| :---: | :--- | :--- | :--- | :---: |
| 1 | **Muhammad Ilham Muttaqim** (Lead Dev) | Fullstack & Integration | Setup Firebase Console, Testing REST API Postman (`signUp`, `signInWithPassword`, `lookup`, `update`), FlutterFire CLI Config | Selesai |
| 2 | **Tim UI/UX** | Frontend Designer | Desain & implementasi `SplashScreen` animasi serta Card `LoginPage` modern (Material 3) | Selesai |
| 3 | **Tim Backend/Auth** | Security & Logic | Penanganan error autentikasi Firebase (`FirebaseAuthException`), `HomePage` Dashboard, dan Sign Out | Selesai |
| 4 | **Tim Dokumentasi** | Technical Writer | Penyusunan dokumentasi `README.md`, Git branching, dan manajemen repository GitHub | Selesai |

---

## 🚀 Ringkasan Langkah Integrasi Firebase (Step-by-Step)

### 1. Konfigurasi Awal di Firebase Console
1. Buat project baru di [Firebase Console](https://console.firebase.google.com/) dengan nama project: `flutter-projectse1-87fc2`.
2. Masuk ke menu **Build $\rightarrow$ Authentication $\rightarrow$ Sign-in method**, lalu aktifkan provider **Email/Password**.

### 2. Pengujian REST API Autentikasi via Postman
Sebelum masuk ke Flutter, 4 endpoint Firebase Identity Toolkit diuji di Postman:
* **Register (`signUp`)**: `POST https://identitytoolkit.googleapis.com/v1/accounts:signUp?key=[API_KEY]` $\rightarrow$ **200 OK**
* **Login (`signInWithPassword`)**: `POST https://identitytoolkit.googleapis.com/v1/accounts:signInWithPassword?key=[API_KEY]` $\rightarrow$ **200 OK**
* **Cek Profil (`lookup`)**: `POST https://identitytoolkit.googleapis.com/v1/accounts:lookup?key=[API_KEY]` $\rightarrow$ **200 OK**
* **Ubah Akun (`update`)**: `POST https://identitytoolkit.googleapis.com/v1/accounts:update?key=[API_KEY]` $\rightarrow$ **200 OK**

### 3. Konfigurasi FlutterFire CLI di Project Flutter
1. Pindahkan file `google-services.json` ke dalam direktori `android/app/`.
2. Login Firebase CLI melalui terminal:
   ```bash
   firebase login
   ```
3. Aktifkan FlutterFire CLI:
   ```bash
   dart pub global activate flutterfire_cli
   ```
4. Hubungkan project Flutter ke Firebase (pilih platform Android & Web):
   ```bash
   dart pub global run flutterfire_cli:flutterfire configure --project=flutter-projectse1-87fc2
   ```
   *(Menghasilkan file `lib/firebase_options.dart` secara otomatis).*

### 4. Instalasi Dependensi Resmi
Tambahkan package pendukung Firebase ke dalam `pubspec.yaml`:
```bash
flutter pub add firebase_core firebase_auth
```

### 5. Inisialisasi Firebase di `main.dart`
```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );
  runApp(const MyApp());
}
```

### 6. Implementasi Fitur Autentikasi
* **Pengecekan State Login di SplashScreen**:
  ```dart
  final user = FirebaseAuth.instance.currentUser;
  ```
* **Proses Login Menggunakan Email & Password**:
  ```dart
  final credential = await FirebaseAuth.instance.signInWithEmailAndPassword(
    email: emailController.text.trim(),
    password: passwordController.text,
  );
  ```
* **Sign Out / Logout Akun**:
  ```dart
  await FirebaseAuth.instance.signOut();
  ```

---

## 📱 Fitur Utama Aplikasi
1. **Animated SplashScreen**: Animasi pembuka otomatis dengan pengecekan status session user.
2. **Modern Login Page**: Form login responsif (Web & Android) dengan validasi regex email, show/hide password, dan penanganan error spesifik.
3. **Student Dashboard (`HomePage`)**: Menampilkan badge status autentikasi, Email aktif, UID Firebase unik, dan tombol Sign Out.