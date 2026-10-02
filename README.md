# PixelFrame

Website satu halaman yang mengubah video menjadi rangkaian karakter (ASCII) frame demi frame, lalu menyusunnya kembali menjadi video utuh yang siap diunduh sebagai MP4 dan dibagikan. Ini adalah "saudara" dari PixelGlyph, dengan bahasa desain yang sama, tapi bekerja pada video, bukan gambar diam.

## Cara pakai
1. Buka `index.html` di browser (double-click, atau klik kanan → Open with → browser modern seperti Chrome, Edge, atau Firefox versi terbaru).
2. Di bagian "Ubah videomu", jatuhkan (drag & drop) sebuah video atau klik kotak putus-putus untuk memilih file dari perangkatmu. Maksimal ukuran file **75MB** (tepat 75MB masih diterima, tapi proses konversi mungkin lebih lambat).
3. Atur sebelum memproses:
   - **Set karakter** — Klasik, Rinci, Blok, Biner, atau tulis sendiri.
   - **Kepadatan** — jumlah kolom karakter per frame.
   - **Mode warna** — tinta monokrom atau warna asli video, per karakter.
   - **Frame per detik hasil** — makin tinggi, makin halus gerakannya, tapi makin lama diproses.
   - **Balik terang/gelap** — membalik pemetaan kecerahan.
4. Tekan **"Proses jadi ASCII"**. Videomu akan dibaca frame demi frame, diubah jadi karakter, lalu direkam ulang menjadi video dan dikonversi ke format MP4 — semuanya berjalan di perangkatmu. Progres ditampilkan secara langsung.
5. Setelah selesai, video asli dan hasil ASCII-nya tampil berdampingan. Tekan **"Unduh sebagai .mp4"** untuk menyimpan, atau **"Bagikan"** untuk mengirim langsung ke aplikasi lain (muncul otomatis bila browser mendukung).

Nama file unduhan mengikuti nama video asli, misalnya `videolucu.mp4` → **`videolucu_pixelframe4.mp4`**.

## Batasan yang perlu diketahui
- **Ukuran file**: maksimal 75MB. Video mendekati batas ini akan lebih lambat diproses.
- **Durasi**: untuk menjaga performa browser, hanya sekitar 60 detik pertama dari video yang diproses; sisanya akan diberi tahu di layar bila terpotong.
- **Audio**: hasil video ASCII tidak menyertakan audio — fokus alat ini murni pada transformasi visual frame demi frame.
- **Koneksi internet**: seluruh pemrosesan video (pembacaan frame, konversi karakter, perekaman) berjalan di perangkatmu tanpa mengunggah apa pun ke server. Namun, saat pertama kali memproses, browser akan mengambil satu berkas mesin konversi video open-source (±30MB, dari CDN publik) untuk mengubah rekaman menjadi format MP4. Setelah diambil sekali, browser biasanya akan menyimpannya di cache untuk pemrosesan berikutnya.
- **Kompatibilitas browser**: memerlukan dukungan `MediaRecorder` dan `canvas.captureStream` — tersedia di Chrome, Edge, dan Firefox versi modern. Bila tidak didukung, akan muncul pemberitahuan di halaman.

## Catatan teknis
- Satu file HTML mandiri (`index.html`) berisi CSS dan JavaScript.
- Frame video diambil dengan menjeda video pada interval waktu tertentu (sesuai fps pilihan), digambar ke kanvas tersembunyi, diubah jadi karakter, lalu digambar ulang ke kanvas rekaman.
- Kanvas rekaman ditangkap sebagai aliran video (`canvas.captureStream`) dan direkam dengan `MediaRecorder` menjadi berkas WebM.
- Berkas WebM tersebut dikonversi menjadi MP4 sepenuhnya di browser memakai [ffmpeg.wasm](https://github.com/ffmpegwasm/ffmpeg.wasm) (build single-thread, tidak memerlukan header server khusus).

## Menghosting online
File ini bisa langsung diunggah ke layanan hosting statis apa pun (GitHub Pages, Netlify, Vercel, cPanel, dll) tanpa perubahan apa pun — cukup unggah `index.html`.
