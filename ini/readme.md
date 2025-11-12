<h1> 03: Application Configuration with .ini Files </h1>
-----
    Langkah ini mengubah proyek kita dari skrip Python sederhana menjadi aplikasi terstruktur yang dikelola oleh file konfigurasi. Kita tidak lagi menjalankan python tutorial/app.py. Sebagai gantinya, kita menggunakan pserve.

1. Cara Menjalankan Aplikasi
    Aplikasi sekarang dijalankan menggunakan pserve dan file development.ini.
    # Pastikan Anda berada di direktori 'ini'
    # Perintah ini adalah cara yang paling andal untuk menjalankan server,
    # menggabungkan Python dari venv, module 'pserve', dan file config:
        ..\venv\Scripts\python -m pyramid.scripts.pserve development.ini --reload


    Flag <i> --reload </i> sangat berguna untuk pengembangan. pserve akan secara otomatis me-restart server setiap kali mendeteksi ada perubahan pada file .py atau .ini Anda.

2. Alur Startup Aplikasi
    # pserve membaca file development.ini dan memulai alur berikut untuk menjalankan aplikasi Anda:
    # pserve mencari bagian [app:main] di file .ini.

    # Di dalam bagian itu, ia menemukan use = egg:tutorial. Ini memberitahu pserve untuk mencari package Python yang terinstal dengan nama tutorial.

    # pserve kemudian memeriksa entry_points dari package tersebut. Berkat setup.py kita, ia menemukan entry point paste.     app_factory yang bernama main.

    # Entry point ini (didefinisikan sebagai main = tutorial:main) menunjuk ke fungsi main yang ada di dalam file tutorial/__init__.py.

    # pserve memanggil fungsi main tersebut. Fungsi ini (dikenal sebagai "app factory") kemudian membuat dan mengembalikan objek aplikasi WSGI Anda.

3. Peran Ganda File .ini
    File development.ini sekarang menjadi pusat kendali utama dan memiliki tiga fungsi:
    # Konfigurasi Aplikasi: Seperti yang dijelaskan di atas, file ini memberi tahu pserve aplikasi apa yang harus dimuat (melalui [app:main]).

    # Konfigurasi Server WSGI: Bagian [server:main] mengatur server yang akan menjalankan aplikasi Anda.

    # use = egg:waitress#main memilih waitress sebagai server kita (yang sudah terinstal sebagai dependensi).

    # listen = localhost:6543 mengatur host dan port. Perhatikan bahwa konfigurasi ini sekarang di luar kode Python Anda, sehingga lebih fleksibel.

    # Konfigurasi Logging: File .ini juga mengatur logging standar Python. Inilah yang menghasilkan output di konsol Anda saat server berjalan dan setiap kali ada request masuk.


4. Peran pip install -e .
    Menjalankan ..\venv\Scripts\python -m pip install -e . (atau yang serupa) tetap sangat penting. Perintah ini:

    1) Membaca setup.py.
    2) Menginstal package tutorial Anda dalam mode editable (perubahan kode langsung terlihat tanpa instal ulang).
        Yang terpenting, perintah ini mendaftarkan entry_point Anda, sehingga pserve dapat menemukannya.
    3) Perintah ini juga memeriksa dan menginstal dependensi yang ada di install_requires (seperti pyramid dan waitress) jika belum ada di venv Anda.

5. Refaktor Kode
    Kode startup kita telah dipindahkan dari tutorial/app.py ke tutorial/__init__.py. Ini bukan keharusan, tetapi ini adalah gaya yang umum di Pyramid untuk memisahkan logika bootstrapping aplikasi (di __init__.py) dari modul aplikasi lainnya.