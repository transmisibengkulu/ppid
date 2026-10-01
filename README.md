# TVRI Bengkulu — Integrasi CSV dari tautan Google Drive

## Prinsip integrasi
- Halaman Admin `Apps_Script/index.html` adalah salinan asli yang diberikan pengguna; tampilannya tidak diubah.
- `Apps_Script/Code.gs` membaca jadwal dari tautan Drive pada sheet yang memiliki kolom `Date` dan `File csv`. CSV dibaca langsung, tidak diimpor ke sheet `Program` untuk kebutuhan Display.
- CSV menggunakan kolom `Start time`, `Name`, dan `Short description`; `Duration` serta `Date` dibaca bila tersedia. Bila CSV tidak memiliki tanggal pada baris, tanggal pada tabel tautan atau tanggal dari nama file digunakan sebagai cadangan.
- Foto dibaca dari sheet `Foto`: kolom `Name` ditampilkan sebagai deskripsi dan `Image` menjadi tautan foto. Tautan Drive harus dapat dibaca oleh Display (biasanya izin berbagi Viewer dengan link).
- Video publik hanya berasal dari file di repository GitHub melalui `GitHub_Pages/videos.json`. Kolom/tautan video di Google Sheets tidak digunakan sebagai sumber video Display.
- GitHub Pages mengambil data satu kali saat halaman dibuka. Jam dan perpindahan program dihitung di browser; foto tetap berganti dari data yang sudah dimuat. Video diputar otomatis, berurutan, lalu kembali ke video pertama setelah video terakhir selesai.

## Pemasangan
1. Di project Apps Script yang sudah ada, ganti isi `Code.gs` dengan `Apps_Script/Code.gs`.
2. **Jangan ganti `index.html` Admin bila halaman Admin Anda masih sama dengan file asli.** File dalam paket disertakan sebagai salinan asli saja.
3. Deploy ulang Apps Script sebagai Web App. Pastikan akun eksekusi memiliki akses ke file CSV di Google Drive. Endpoint `page=data` mengirim data jadwal dan foto ke Display.
4. Di repository GitHub Pages, unggah isi folder `GitHub_Pages` ke root repository. Pertahankan atau isi `APPS_SCRIPT_URL` pada `config.js` dengan URL Web App yang berakhiran `/exec`.
5. Isi `videos.json` dengan path relatif file video yang memang sudah ada di repository. Contoh:

```json
{
  "videos": [
    "videos/video1.mp4",
    "videos/video2.mp4"
  ]
}
```

Jangan mengisi `videos.json` dengan tautan Drive bila video harus diputar dari repository GitHub.

## Format sheet
### Tabel tautan CSV
Header minimal: `Date` dan `File csv`. Kolom `ID`, `Created`, dan `Created_By` didukung dan dapat tetap dipakai.

### Sheet foto
Nama tab: `Foto`. Header yang digunakan: `Name`, `Date`, dan `Image`; kolom `ID` dan `Created_By` boleh tetap ada.

Catatan: jadwal CSV yang memiliki kolom tanggal menggunakan tanggal pada CSV. Untuk format tanggal ambigu `D/M/YYYY` pada CSV, kode mengikuti format file jadwal TVRI (misalnya `3/9/2026` = 3 September 2026). Pada kolom `Date` di sheet tautan dan foto, format teks ambigu seperti `10/1/2026` dibaca sebagai `1 Oktober 2026`; nilai tanggal asli Google Sheets dibaca sebagai tanggal.
