<h1> 03: Application Configuration with .ini Files </h1>

Langkah ini mengubah proyek kita dari skrip Python sederhana menjadi aplikasi terstruktur yang dikelola oleh file konfigurasi. Kita tidak lagi menjalankan python tutorial/app.py. Sebagai gantinya, kita menggunakan pserve.

1. Cara Menjalankan Aplikasi
    Aplikasi sekarang dijalankan menggunakan pserve dan file development.ini.
    - Pastikan berada di direktori 'ini'
    - Perintah ini adalah cara yang paling andal untuk menjalankan server,
    - menggabungkan Python dari venv, module 'pserve', dan file config:
    
        ..\venv\Scripts\python -m pyramid.scripts.pserve development.ini --reload


    - Flag <i> --reload </i> sangat berguna untuk pengembangan. pserve akan secara otomatis me-restart server setiap kali mendeteksi ada perubahan pada file .py atau .ini.

2. Alur Startup Aplikasi
    pserve membaca file <i> development.ini</i> dan memulai alur berikut untuk menjalankan aplikasi dengan:
    - pserve mencari bagian [app:main] di file <i> .ini. </i>
    - Di dalam bagian itu, ia menemukan <i> use = egg:tutorial</i>. Yang memberitahu pserve untuk mencari package Python yang terinstal dengan nama tutorial.
    - pserve kemudian memeriksa entry_points dari package tersebut. Karena <i> setup.py </i>, ia akan menemukan entry point paste. <i> app_factory </i> yang bernama main.
    - Entry point ini didefinisikan sebagai main = tutorial:main menunjuk ke fungsi main yang ada di dalam file tutorial/__init__.py.
    - pserve memanggil fungsi main tersebut. Fungsi ini dikenal sebagai "app factory" kemudian membuat dan mengembalikan objek aplikasi WSGI.

3. Peran Ganda File .ini
    File development.ini sekarang menjadi pusat kendali utama dan memiliki tiga fungsi, yaitu:
    - Konfigurasi Aplikasi, seperti yang dijelaskan di atas, file ini memberi tahu pserve aplikasi apa yang harus dimuatmelalui [app:main].
    - Konfigurasi Server WSGI, pada bagian [server:main] mengatur server yang akan menjalankan aplikasi Anda.
    - <i> use = egg:waitress#main </i> memilih waitress sebagai server yang sudah terinstal sebagai dependensi.
    - listen = localhost:6543 mengatur host dan port. Perhatikan bahwa konfigurasi ini sekarang di luar kode Python, sehingga lebih fleksibel.
    - Konfigurasi Logging pada file .ini juga mengatur logging standar Python. Inilah yang menghasilkan output di konsol  saat server berjalan dan setiap kali ada request masuk.


4. Peran pip install -e .
    Menjalankan `..\venv\Scripts\python -m pip install -e .` atau yang serupa tetap sangat penting. Perintah ini:

    1) Membaca setup.py.
    2) Menginstal package tutorial dalam mode editable dengan perubahan kode langsung terlihat tanpa instal ulang.
        Yang terpenting, perintah ini mendaftarkan entry_point, sehingga pserve dapat menemukannya.
    3) Perintah ini juga memeriksa dan menginstal dependensi yang ada di install_requires (seperti pyramid dan waitress) jika belum ada di venv.

5. Refaktor Kode
    Kode startup telah dipindahkan dari tutorial/app.py ke tutorial/__init__.py. Ini bukan keharusan, tetapi ini adalah gaya yang umum di Pyramid untuk memisahkan logika bootstrapping aplikasi (di __init__.py) dari modul aplikasi lainnya.