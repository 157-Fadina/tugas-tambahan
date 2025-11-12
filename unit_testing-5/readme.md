<h1> 05: Unit Tests and pytest </h1>
File `tests.py` ini adalah contoh implementasi *unit test* untuk aplikasi Pyramid. Berikut adalah rincian dari apa yang kita lakukan dan mengapa kita melakukannya.

1. Framework Test
    Kita menggunakan modul `unittest` standar dari Python sebagai fondasi. Namun, untuk mempermudah pengujian yang spesifik untuk Pyramid, kita juga mengimpor *helper* dari `pyramid.testing`.
    
    - `testing.setUp()`, fungsi ini dijalankan sebelum setiap tes. Ia membuat *Configurator* Pyramid tiruan (`self.config`) yang bisa kita gunakan jika tes kita perlu mendaftarkan pengaturan khusus.
    - `testing.tearDown()`: Dijalankan setelah setiap tes selesai untuk "membersihkan" konfigurasi yang dibuat oleh `setUp()`.

2. Anatomi Test (`test_hello_world`)
    Setiap *unit test* harus fokus pada satu "unit" fungsionalitas. Tes ini sangat spesifik:
    
    1) Impor di Dalam Fungsi, mengimpor *view* (`from tutorial import hello_world`) di dalam fungsi tes, bukan di bagian atas file. Ini adalah praktik penting untuk **isolasi tes**. Ini memastikan setiap tes dimulai dalam kondisi bersih dan tidak terpengaruh oleh impor atau status dari tes lain.
    2)  **Membuat Request Palsu**: Kita membuat `testing.DummyRequest()`. Ini adalah objek tiruan yang berperilaku seperti request HTTP yang masuk ke server kita.
    3)  **Memanggil View**: Kita memanggil fungsi *view* `hello_world` secara langsung dan meneruskan request palsu tadi.
    4)  **Memeriksa Hasil (Assertion)**: Kita menggunakan `self.assertEqual()` untuk memeriksa apakah `response.status_code` bernilai `200`. Status 200 berarti "OK", yang menandakan *view* kita berhasil memproses request tanpa error.

3. Catatan Penting
    Untuk tes spesifik yang kita tulis ini, *view* `hello_world` sangat sederhana dan tidak bergantung pada konfigurasi Pyramid apa pun. Oleh karena itu, panggilan ke `testing.setUp()` dan `testing.tearDown()` sebenarnya **tidak wajib** di sini. Kita menyertakannya sebagai contoh praktik yang baik (*best practice*) untuk tes di masa depan yang mungkin lebih kompleks (misalnya, tes yang perlu mendaftarkan *route*, *renderer*, atau pengaturan keamanan sebelum dijalankan).

![alt text](<Screenshot 2025-11-12 225002.png>)