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
4. Tekan **"Proses jadi ASCII"**. Videomu akan dibaca frame demi frame, diubah jadi karakter, lalu disusun langsung menjadi berkas MP4 — semuanya berjalan di perangkatmu, dan biasanya jauh lebih cepat dari durasi video aslinya (tidak perlu menunggu "real-time" seperti merekam layar). Progres ditampilkan secara langsung.
5. Setelah selesai, video asli dan hasil ASCII-nya tampil berdampingan. Tekan **"Unduh sebagai .mp4"** untuk menyimpan, atau **"Bagikan"** untuk mengirim langsung ke aplikasi lain (muncul otomatis bila browser mendukung).

Nama file unduhan mengikuti nama video asli, misalnya `videolucu.mp4` → **`videolucu_pixelframe4.mp4`**.

## Batasan yang perlu diketahui
- **Ukuran file**: maksimal 75MB. Video mendekati batas ini akan lebih lambat diproses.
- **Durasi**: untuk menjaga performa browser, hanya sekitar 60 detik pertama dari video yang diproses; sisanya akan diberi tahu di layar bila terpotong.
- **Audio**: hasil video ASCII tidak menyertakan audio — fokus alat ini murni pada transformasi visual frame demi frame.
- **Koneksi internet**: seluruh pemrosesan video (pembacaan frame, konversi karakter, penyandian ke MP4) berjalan sepenuhnya di perangkatmu tanpa mengunggah apa pun ke server. Satu-satunya hal yang diambil dari internet adalah pustaka penyusun MP4 (`mp4-muxer`, murni JavaScript, ±20KB) — jauh lebih kecil dibanding pendekatan sebelumnya, dan biasanya langsung tersimpan di cache browser setelah pertama kali dipakai.
- **Kompatibilitas browser**: memerlukan dukungan **WebCodecs API** (`VideoEncoder`/`VideoFrame`) — tersedia di Chrome, Edge versi modern, dan Safari 16.4+. Firefox mendukungnya mulai versi yang cukup baru; bila browser belum mendukung, akan muncul pemberitahuan jelas di halaman alih-alih gagal diam-diam.

## Catatan teknis
- Satu file HTML mandiri (`index.html`) berisi CSS dan JavaScript.
- Frame video diambil dengan menjeda video pada interval waktu tertentu (sesuai fps pilihan), digambar ke kanvas tersembunyi, diubah jadi karakter, lalu digambar ulang ke kanvas rekaman berukuran genap (syarat H.264).
- Tiap frame kanvas itu langsung disandikan ke H.264 memakai **WebCodecs API** (`VideoEncoder`) — berjalan di thread utama sebagai skrip biasa, tanpa Web Worker lintas-domain dan tanpa mesin WASM besar.
- Potongan-potongan video terenkode itu disusun menjadi satu berkas MP4 yang valid memakai [mp4-muxer](https://github.com/Vanilagy/mp4-muxer), sebuah pustaka JavaScript murni (bukan WASM) yang dirancang khusus untuk dipasangkan dengan WebCodecs.
- Pendekatan ini menggantikan cara sebelumnya (`MediaRecorder` + `canvas.captureStream` + `ffmpeg.wasm`), yang rawan gagal karena pembatasan Web Worker lintas-domain saat dihost online, serta memerlukan unduhan mesin video berukuran puluhan MB.

## Menghosting online
File ini bisa langsung diunggah ke layanan hosting statis apa pun (GitHub Pages, Netlify, Vercel, cPanel, dll) tanpa perubahan apa pun — cukup unggah `index.html`.
