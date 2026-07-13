# MAIDA Esports — Panduan Firebase

Laman kini disambung ke **Firebase**: data disimpan di **Firestore** (dilihat semua pelawat), log masuk admin guna **Firebase Auth**.

Fail: `index.html`, `acara.html`, `admin.html` (config Firebase sudah ditanam di dalamnya).

## 1) Aktifkan Firestore
Firebase Console → **Build → Firestore Database → Create database** → mula dalam *production mode* → pilih lokasi (cth `asia-southeast1`).

## 2) Tetapkan Firestore Rules
Firestore → tab **Rules** → tampal ini → **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /site/{docId} {
      allow read: if true;                 // semua orang boleh baca (laman awam)
      allow write: if request.auth != null; // hanya admin yang log masuk boleh tulis
    }
  }
}
```

## 3) Aktifkan Log Masuk Admin
Firebase Console → **Build → Authentication → Get started** → **Sign-in method** → dayakan **Email/Password**.
Kemudian tab **Users → Add user** → masukkan e-mel & kata laluan admin anda.
Guna e-mel/kata laluan itu untuk log masuk di `admin.html`.

## 4) Cara ia berfungsi
- Buka **admin.html** → log masuk → edit kandungan → **Simpan**.
- Setiap simpan ditulis ke Firestore (`site/content`).
- **index.html** & **acara.html** membaca Firestore secara **langsung** — perubahan muncul serta-merta untuk semua pelawat (tak perlu muat naik JSON lagi).

## 5) Had penting (gambar)
Satu dokumen Firestore maksimum **1 MB**. Gambar yang **dimuat naik** di admin disimpan sebagai data (base64) di dalam dokumen ini.
- OK untuk beberapa gambar kecil (banner, 1–2 poster).
- Untuk banyak/gambar besar (poster acara), lebih baik guna **URL gambar** (letak fail di Firebase Storage / hosting lain, tampal URL) supaya tidak melebihi had 1 MB.

## 6) (Pilihan) Hos di Firebase Hosting
```
npm install -g firebase-tools
firebase login
firebase init hosting      # public dir: folder yang ada index.html
firebase deploy
```

## Nota keselamatan
- Kunci API Firebase web memang direka untuk didedahkan di klien — keselamatan datang dari **Rules** + **Auth** di atas, bukan menyembunyikan kunci.
- `maida-content.json` kekal sebagai sandaran/lalai jika Firebase tak dapat dimuat.
