# MAIDA Esports — Panduan Firebase (Realtime Database)

Laman guna **Firebase Realtime Database**: data yang disimpan di admin akan dilihat oleh **semua pelawat**.
Config sudah ditanam dalam `index.html`, `acara.html`, `admin.html`.

## 1) Tetapkan Rules (JSON — bukan Firestore)

Firebase Console → **Build → Realtime Database → tab Rules** → tampal ini → **Publish**:

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

Ini rule **test mode** (mudah, tiada parse error). Ia bermakna sesiapa boleh baca & tulis — sesuai untuk mula.

### Versi lebih selamat (pilihan, kemudian)
Baca terbuka, tulis hanya jika log masuk Firebase Auth:
```json
{
  "rules": {
    "site": {
      ".read": true,
      ".write": "auth != null"
    }
  }
}
```

## 2) Cara ia berfungsi
- Buka **admin.html** → log masuk (kata laluan: `maida2026`) → edit → **Simpan**
- Data ditulis ke Realtime Database di nod **`site/content`**
- **index.html** & **acara.html** membaca nod itu secara **langsung** — perubahan muncul serta-merta untuk semua pelawat
- Kali pertama log masuk, data lalai akan di-*seed* ke database secara automatik

## 3) Sandaran
- **Export** di admin → simpan `maida-content.json` sebagai sandaran
- `maida-content.json` juga jadi fallback jika Firebase tak dapat dimuat

## 4) Nota gambar
Gambar yang **dimuat naik** disimpan sebagai data (base64) dalam database. Untuk banyak/poster besar, lebih baik guna **URL gambar** supaya database kekal ringan dan laman laju.

## 5) Tukar kata laluan admin
Buka `admin.html`, cari baris:
```js
const PASSWORD = "maida2026";
```
Tukar kepada kata laluan anda. (Ini pagar ringkas di pelayar — untuk keselamatan penuh, guna Firebase Auth.)

## 6) (Pilihan) Hos di Firebase Hosting
```
npm install -g firebase-tools
firebase login
firebase init hosting      # public dir: folder yang ada index.html
firebase deploy
```
