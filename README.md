# SHM (Simple Hotspot MikroTik)

Template halaman login **MikroTik Hotspot** sederhana berbasis **Bootstrap 4**, dirancang agar tampil responsif di semua ukuran layar — dari HP 320px hingga monitor lebar.

**Author** : Adhy Nugraha
**Versi**   : v.1

- Personal Blog : <https://hlambuh.wordpress.com> atau <https://jurnalmuis.github.io/>
- Icon : <https://www.flaticon.com/authors/flat-icons>
- Framework : Bootstrap 4 <https://getbootstrap.com/>

## Fitur

- Responsif & mobile-first: kartu login mengikuti lebar layar dan ter-center vertikal-horizontal.
- Tombol ramah sentuh (min. 46px) sesuai pedoman aksesibilitas touch target.
- Input besar (min. 44px) dan label tetap terlihat (bukan placeholder-only).
- Fokus keyboard terlihat jelas (`:focus-visible`).
- Mendukung `prefers-reduced-motion`.
- CATATAN: variabel `$(...)` (mis. `$(link-login-only)`) diisi otomatis oleh RouterOS — jangan dihapus.

## Daftar File

| File            | Fungsi                                                          |
| --------------- | --------------------------------------------------------------- |
| `login.html`    | Halaman login utama (dengan dukungan CHAP/MD5)                 |
| `rlogin.html`   | Redirect WISPr / saat login diperlukan                          |
| `redirect.html` | Redirect ke `$(link-redirect)`                                  |
| `alogin.html`   | Layar "login berhasil, silahkan tunggu" sebelum `status.html`   |
| `status.html`   | Status sesi (IP, bytes, uptime, logout)                         |
| `logout.html`   | Konfirmasi logout + ringkasan sesi                              |
| `radvert.html`  | Halaman iklan/advertisement MikroTik                            |
| `error.html`    | Halaman error dengan pesan `$(error)` dan tautan login          |
| `md5.js`        | Library MD5 untuk login CHAP                                    |
| `css/style.css` | Gaya responsif & touch-friendly kustom (diatas Bootstrap)       |

## Struktur Folder

```
SHM/
├── css/
│   └── style.css        # gaya kustom (responsive layout)
├── img/
│   ├── internet.png
│   └── logobottom.png
├── *.html               # halaman hotspot MikroTik
├── md5.js
└── README.md
```

## Cara Pasang di Router MikroTik

1. Siapkan **Hotspot** di RouterOS:
   ```
   /ip hotspot setup
   ```
   atau eksisting:
   ```
   /ip hotspot set
   ```
2. Upload file (gunakan FTP/WinBox drag-and-drop) ke folder hotspot:
   - `login.html`
   - `alogin.html`
   - `status.html`
   - `logout.html`
   - `rlogin.html`
   - `redirect.html`
   - `radvert.html`
   - `error.html`
   - `md5.js`
   - `css/style.css`
   - `css/bootstrap.min.css`
   - `img/internet.png` (otorisasi tergantung setelan hotspot server)

   FTP contoh (dari PC):
   ```
   ftp 192.168.1.1
   cd hotspot
   put login.html
   put alogin.html
   ...
   ```

3. Tautkan file hotlogin ke template kustom:
   ```
   /ip hotspot walled-garden ip
   /ip hotspot servlet set hotspot-html-directory=hotspot
   /ip hotspot set html-directory=hotspot
   ```
   atau lewat WebFig/WinBox hapus `login.html` bawaan lalu ganti dengan file di atas.

4. Tentukan menu awal login (pertama kali muncul pilihan MANUAL/CHAP):
   ```
   /ip hotspot profile set [find] login-by=both
   ```
   Login **MANUAL** memakai JS biasa, **CHAP** memakai `md5.js`.

## Catatan Server-Side (jangan diubah)

File di direktorat ini berisi skrip template RouterOS dengan sintaks `$(...)`:

- `$(if chap-id)` / `$(endif)` — percabangan server-side
- `$(link-login-only)`, `$(link-orig)`, `$(link-logout)` — tautan dinamis
- `$(username)`, `$(password)`, `$(ip)`, `$(mac)`, `$(uptime)`, `$(bytes-in-nice)`, `$(bytes-out-nice)`, `$(session-time-left)` — variabel sesi

Menghapus atau mengubah variabel tersebut akan membuat halaman tidak berfungsi di router.

## Lisensi

**Ketentuan wajib**:

- **Jangan hapus atau samarkan kredit author** — nama penulis, tautan blog, dan komentar header (mis. `Author : Adhy Nugraha`) di dalam file HTML/CSS **tetap wajib dipertahankan** selama template ini digunakan atau diturunkan.
- Jika dimodifikasi dan dibagikan ulang, cukup tambahkan nama/kredit Anda di samping kredit asli — jangan menggantinya.