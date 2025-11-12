<h1> 12: Templating With jinja2 </h1>
Langkah ini menunjukkan betapa fleksibelnya Pyramid dalam mengizinkan kita menukar komponen-komponen utama, seperti templating engine.

1. Cara Kerja Add-on di Pyramid
    Mendapatkan add-on Pyramid untuk bekerja adalah proses yang sederhana dan terdiri dari dua langkah:
    - Pertama, kita meng-install paket tersebut ke virtual environment kita menggunakan pip (seperti paket Python lainnya).
    - Kedua, kita memberi tahu Configurator Pyramid (config) untuk menjalankan kode setup dari add-on tersebut.

    Dalam kasus ini, kita menggunakan <i> config.include('pyramid_jinja2') </i> di dalam file <b>  __init__.py. </b> Langkah ini "mendaftarkan" Jinja2 ke Pyramid sebagai templating engine baru. Secara spesifik, ini memberi tahu Pyramid bahwa setiap view yang dikonfigurasi dengan renderer yang berakhiran .jinja2 harus diproses menggunakan Jinja2.

2. Perubahan Kode
    Kode yang perlu kita ubah untuk beralih templating engine:
    - <i> View (views.py) </i>, pada kode view logika bisnis sama sekali tidak berubah. Satu-satunya perbedaan adalah pada decorator, di mana  mengubah nama renderer, sebelumnya: <b> @view_defaults(renderer='home.pt') </b> dan sekarang yaitu <b> @view_defaults(renderer='home.jinja2') </b>
    - Template <i> (home.jinja2) </i> adalah sintaks untuk menyisipkan variabel dasar di Jinja2 ({{ name }}) sangat mirip dengan Chameleon (${name}), sehingga isi template-nya pun hampir identik.

3. Analisis Tambahan
    Analisis ini menyoroti dua cara berbeda untuk melakukan hal yang sama di Pyramid, yaitu:

    1. Proyek kita sekarang bergantung pada pyramid_jinja2. Kita menambahkannya ke setup.py. Mengapa ini lebih baik daripada menginstalnya secara manual (pip install pyramid_jinja2)? karena dengan menambahkannya ke <b> install_requires </b> di setup.py, kita mencatatnya sebagai dependensi permanen proyek. Yang berarti:

    - Proyek kita menjadi self-contained. Siapa pun dapat meng-install semua yang dibutuhkan proyek hanya dengan satu perintah: <b> pip install -e .. </b> Kita tidak perlu ingat untuk meng-install pyramid_jinja2 secara manual setiap kali kita membuat virtual environment baru.

    2. Kita menggunakan <i> config.include('pyramid_jinja2')</i> yaitu konfigurasi imperatif di __init__.py. Dengan ini kita bisa menggunakan konfigurasi deklaratif dengan menambahkannya ke file <i> .ini <i> kita. Sama seperti yang kita lakukan untuk debug toolbar, kita bisa menambahkannya di development.ini:

        pyramid.includes =
            pyramid_debugtoolbar
            pyramid_jinja2

    Kedua metode imperatif di .py atau deklaratif di .ini mencapai hasil akhir yang sama persis. Ini murni soal preferensi dan di mana kita lebih suka mengelola konfigurasi add-on.