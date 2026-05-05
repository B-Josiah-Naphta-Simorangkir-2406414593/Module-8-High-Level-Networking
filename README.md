# Reflection 8

## 1. Apa perbedaan utama antara metode RPC unary, server streaming, dan bi-directional streaming, dan dalam skenario apa masing-masing paling cocok digunakan?

Unary RPC: Klien mengirim satu permintaan dan mendapatkan satu respons. Cocok untuk operasi sederhana seperti login atau pengambilan data tunggal.

Server Streaming: Klien mengirim satu permintaan, dan server mengirimkan aliran pesan kembali. Cocok untuk fitur seperti real-time dashboard atau mengunduh data riwayat yang besar.

Bi-directional Streaming: Klien dan server saling mengirim urutan pesan menggunakan aliran baca-tulis. Paling cocok untuk aplikasi interaktif seperti chat atau kolaborasi dokumen secara real-time.

## 2. Apa saja pertimbangan keamanan potensial yang terlibat dalam penerapan layanan gRPC di Rust, terutama terkait otentikasi, otorisasi, dan enkripsi data?

Otentikasi: Biasanya diimplementasikan menggunakan metadata gRPC untuk membawa token seperti JWT.

Otorisasi: Menggunakan interceptor di sisi server untuk memeriksa izin akses sebelum mengeksekusi logika fungsi.

Enkripsi Data: gRPC sangat bergantung pada TLS/SSL untuk mengenkripsi lalu lintas data secara end-to-end guna mencegah penyadapan.

## 3. Apa saja tantangan atau masalah potensial yang mungkin muncul saat menangani streaming dua arah di Rust gRPC, terutama dalam skenario seperti aplikasi obrolan?

Manajemen State: Mengelola koneksi banyak pengguna secara bersamaan tanpa menyebabkan kebocoran memori.

Penanganan Error: Menangani pemutusan koneksi yang tiba-tiba agar tidak membuat server crash atau menyebabkan status yang tidak konsisten.

Sinkronisasi Asinkron: Memastikan pesan dikirim dan diterima dalam urutan yang benar menggunakan runtime tokio.

## 4. Apa keuntungan dan kerugian menggunakan tokio_stream::wrappers::ReceiverStream untuk streaming respons dalam layanan Rust gRPC?

Keuntungan: Memungkinkan integrasi yang mulus antara channel mpsc milik Tokio dengan kebutuhan aliran data gRPC, membuat kode lebih bersih.

Kerugian: Jika konsumen (klien) lambat, pesan dapat menumpuk di memori buffer kecuali jika diterapkan kontrol aliran (backpressure) yang tepat.

## 5. Dengan cara apa kode gRPC Rust dapat disusun untuk memfasilitasi penggunaan kembali kode dan modularitas, mempromosikan pemeliharaan dan ekstensibilitas seiring waktu?

Pemisahan Layer: Memisahkan definisi .proto, logika bisnis, dan entry point server ke dalam modul atau crate yang berbeda.

Penggunaan Traits: Memanfaatkan sistem trait di Rust untuk mendefinisikan antarmuka layanan yang konsisten.

## 6. Dalam implementasi MyPaymentService, langkah tambahan apa yang mungkin diperlukan untuk menangani logika pemrosesan pembayaran yang lebih kompleks?

Integrasi Pihak Ketiga: Menambahkan koneksi ke payment gateway eksternal dan menangani responsnya.

Validasi & Database: Menambahkan pengecekan saldo di database dan mekanisme rollback jika transaksi gagal di tengah jalan.

## 7. Apa dampak adopsi gRPC sebagai protokol komunikasi terhadap keseluruhan arsitektur dan desain sistem terdistribusi, terutama dalam hal interoperabilitas dengan teknologi dan platform lain?

Kontrak yang Ketat: Penggunaan IDL (Interface Definition Language) memastikan semua tim pengembang memiliki kontrak yang sama meski menggunakan bahasa pemrograman berbeda.

Efisiensi Sumber Daya: Mengurangi penggunaan bandwidth dan latensi secara signifikan dibandingkan protokol berbasis teks.

## 8. Apa keuntungan dan kerugian menggunakan HTTP/2, protokol dasar untuk gRPC, dibandingkan dengan HTTP/1.1 atau HTTP/1.1 dengan WebSocket untuk REST API?

Keuntungan: Mendukung multiplexing (banyak permintaan dalam satu koneksi), kompresi header, dan komunikasi biner yang lebih efisien.

Kerugian: Sulit untuk diuji secara manual tanpa tools khusus dan memiliki dukungan terbatas pada beberapa ekosistem web lama.

## 9. Bagaimana kontras model request-response REST API dengan kemampuan bidirectional streaming gRPC dalam hal komunikasi real-time dan responsivitas?

REST API: Biasanya bersifat searah dan membutuhkan polling atau overhead tambahan untuk simulasi real-time.

gRPC: Menyediakan komunikasi asinkron yang sangat cepat dan responsif karena jalur data tetap terbuka secara dua arah.

## 10. Apa implikasi dari pendekatan berbasis skema gRPC, menggunakan Protocol Buffers, dibandingkan dengan sifat JSON yang lebih fleksibel dan tanpa skema dalam payload REST API?

Protocol Buffers: Memberikan keamanan tipe data yang kuat dan ukuran pesan yang jauh lebih kecil, namun kurang fleksibel untuk perubahan skema yang drastis.

JSON: Sangat fleksibel dan mudah dibaca manusia, namun memakan lebih banyak beban CPU untuk proses parsing.