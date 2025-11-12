<h1> 06: Functional Testing with WebTest </h1>
Kita baru saja menambahkan WebTest ke dalam alur kerja testing kita. Ini adalah langkah besar yang memberi kita "end-to-end testing". Mari kita bedah apa artinya itu.

1. Unit Test vs. Functional Test
    - Unit Test (yang lama), menguji satu "unit" <i> (fungsi hello_world) </i> secara terisolasi. Dengan membuat DummyRequest palsu dan hanya memeriksa status kode. Analogi ini seperti menguji mesin mobil di luar sasis mobilnya. Kita tahu mesinnya menyala, tapi kita tidak tahu apakah mobilnya bisa jalan.

    - Functional Test (yang baru), menguji seluruh alur kerja aplikasi. WebTest "mem-boot" seluruh aplikasi Pyramid kita di memori. Ia bertindak seperti browser palsu, mengirim request nyata ke route (/), membiarkan Pyramid memprosesnya, dan menerima respons HTML yang utuh. Analogi ini seperti kita benar-benar masuk ke mobil, menyalakan kunci, dan mengendarainya di jalan.

2. Manfaat Utama: Menguji Template
    Unit test kita hanya membuktikan bahwa fungsi hello_world mengembalikan status 200 OK. Ia tidak tahu HTML apa yang dilihat oleh pengguna. Dengan functional test, kita sekarang bisa memeriksa isi body dari respons: <i> self.assertIn(b'<.h1>Hello World!</.h1>', res.body) </i>

    Ini adalah "end-to-end testing" yang sesungguhnya. Kita membuktikan bahwa:
    - Aplikasi kita berjalan.
    - Route / terhubung ke view yang benar.
    - View tersebut menggunakan template yang benar.
    - Template tersebut berhasil di-render dan menghasilkan HTML yang kita harapkan.

3. Integrasi Mulus dengan Pytest
    Kita tidak perlu menjalankan dua perintah terpisah. pytest cukup pintar untuk menemukan kedua class tes kita (TutorialViewTests dan TutorialFunctionalTests) dan menjalankannya bersama-sama. Inilah mengapa output kita sekarang .. (dua tes lolos) dan dilaporkan dalam satu hasil yang sama.

4. Tetap Cepat
    Mungkin melihat waktu eksekusi tes hanya naik sedikit (misalnya, dari 0.14s menjadi 0.25s). Meskipun functional test melakukan lebih banyak pekerjaan (mem-boot seluruh aplikasi), WebTest sangat efisien. Ini karena WebTest melakukan semuanya di memori. Ia tidak perlu benar-benar membuka browser (seperti Selenium) atau menjalankan server web di port sungguhan. Ini membuat alur kerja testing kita tetap cepat dan praktis untuk dijalankan setiap saat.

![alt text](<Screenshot 2025-11-12 225945.png>)