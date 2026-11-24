# Menyiapkan sinkronisasi akun Google SignLingo

Halaman SignLingo tetap berjalan sebagai aplikasi statis. Firebase dipakai untuk login Google dan menyimpan progres serta contoh latihan pada akun masing-masing. Tanpa konfigurasi Firebase, aplikasi tetap menyimpan data lokal di browser.

## 1. Buat proyek Firebase

1. Buka [Firebase Console](https://console.firebase.google.com/) dan buat proyek.
2. Di **Authentication → Sign-in method**, aktifkan **Google**.
3. Di **Authentication → Settings → Authorized domains**, tambahkan domain hosting, misalnya `faracitrash.github.io`. Tambahkan `localhost` hanya bila menguji melalui server lokal.
4. Buat database melalui **Firestore Database → Create database**.
5. Tambahkan aplikasi web dari pengaturan proyek (**Project settings → Your apps → Web app**) dan salin konfigurasi aplikasi web.

## 2. Masukkan konfigurasi web ke HTML

Di bagian awal `index.html`, cari:

```html
<script>window.SIGNLINGO_FIREBASE_CONFIG = null;</script>
```

Ganti `null` dengan objek konfigurasi Web App dari Firebase, contohnya:

```html
<script>
  window.SIGNLINGO_FIREBASE_CONFIG = {
    apiKey: "...",
    authDomain: "nama-proyek.firebaseapp.com",
    projectId: "nama-proyek",
    storageBucket: "nama-proyek.firebasestorage.app",
    messagingSenderId: "...",
    appId: "..."
  };
</script>
```

Gunakan konfigurasi **Web App**. Jangan masukkan private key atau file service account ke HTML.

## 3. Pasang aturan Firestore

Aturan di `firestore.rules` hanya mengizinkan akun membaca dan mengubah dokumen di ruang milik UID mereka sendiri. Salin isinya ke **Firestore Database → Rules**, lalu klik **Publish**. Atau, jika Firebase CLI sudah terpasang dan proyek sudah dipilih, jalankan:

```sh
firebase deploy --only firestore:rules
```

`firebase.json` pada folder ini menunjuk ke file aturan tersebut.

## 4. Uji melalui domain web

Buka situs melalui GitHub Pages/hosting HTTPS (atau server lokal), lalu masuk dengan Google dari halaman Profil. Membuka file dengan alamat `file://` tidak cocok untuk alur login Google. Setelah masuk, progres dan contoh latihan disinkronkan; jika sedang offline atau konfigurasi belum aktif, data perangkat tetap digunakan.

Perubahan konfigurasi dan aturan belum dipublikasikan. Unggah file situs dan aturan ke proyek Firebase/GitHub hanya setelah konfigurasi proyek milik Anda siap.
