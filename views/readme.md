<h1> 07: Basic Web Handling With Views </h1>
Langkah ini adalah refaktor besar yang membuat kode kita lebih bersih dan memperkenalkan cara baru yang lebih umum untuk mendaftarkan views.

1. Kode yang Lebih Rapi (Refactoring)
    Tujuan utama di sini adalah memisahkan logika view dari kode startup aplikasi. Sebelumnya, file <i> tutorial/__init__.py </i> kita berisi logika untuk memulai aplikasi dan logika untuk view (fungsi hello_world). Ini akan cepat berantakan seiring bertambahnya view. Dan sekarang, file <i> tutorial/__init__.py </i> sekarang hanya fokus pada konfigurasi dengan menyiapkan routes dan memberi tahu Pyramid di mana harus mencari views. Semua logika view (fungsi home dan hello) telah dipindahkan ke file baru yang didedikasikan untuk itu: tutorial/views.py. Bagaimana Pyramid menemukan views di file baru itu? Dengan perintah ini di __init__.py:

        config.scan('.views')

    Ini menyuruh Pyramid untuk "memindai" file views.py dan secara otomatis menemukan view apa pun yang telah ditandai untuk didaftarkan.

2. URL, Route, dan View Bisa Berbeda
    Langkah ini juga menyoroti tiga konsep berbeda yang bekerja sama:
    - URL: Alamat yang diketik pengguna di browser (misalnya, / atau /howdy).
    - Nama Route: Nama internal yang kita berikan pada sebuah URL (misalnya, home atau hello).
    - Nama View: Nama fungsi Python yang menangani request (misalnya, def home(...) atau def hello(...)).

    Perhatikan bahwa untuk view kedua kita, yaitu URL-nya adalah /howdy dengan nama route-nya adalah hello. Lalu, nama fungsi view-nya adalah hello dan ketiganya bisa saja berbeda, memberi banyak fleksibilitas.

3. Konfigurasi: Imperatif vs. Deklaratif
    Ini adalah konsep terpenting. Pyramid menawarkan dua cara untuk mendaftarkan view, dan keduanya mencapai hasil yang sama persis.
    
    1) Cara Imperatif (Yang Kita Lakukan Sebelumnya)
    Ini adalah saat kita secara eksplisit memberi tahu configurator untuk mendaftarkan view dari dalam file __init__.py.
    - Di __init__.py
    
        config.add_view(hello_world) 

2. Cara Deklaratif (Yang Baru)
    Ini adalah saat kita "mendekorasi" fungsi view kita langsung di file views.py untuk mendaftarkan dirinya sendiri.
    
    - Di views.py

        @view_config(route_name='hello')
            def hello(request):
            # ...

    Decorator @view_config adalah "penanda" yang akan ditemukan oleh config.scan(). Ini memberi tahu Pyramid, "Hei, fungsi ini adalah view untuk route bernama 'hello'." Pada akhirnya, pilihan antara imperatif dan deklaratif murni soal selera. Cara deklaratif (@view_config) sering disukai karena menjaga konfigurasi view tetap berada tepat di sebelah kode view itu sendiri, membuatnya lebih mudah dibaca.

![alt text](<Screenshot 2025-11-12 230953.png>)