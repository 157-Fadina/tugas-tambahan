<h1> 13: CSS/JS/Images Files With Static Assets </h1>
Pada langkah ini, kita mengonfigurasi aplikasi WSGI kita untuk menyajikan file statis (seperti CSS, JavaScript, atau gambar) langsung dari package Python kita.

1. Mendaftarkan Direktori Statis
    Perubahan utama ada di tutorial/__init__.py:

        config.add_static_view(name='static', path='tutorial:static')

    Perintah ini melakukan dua hal:
    1) <b> name='static' </b>, membuat rute baru. Ini memberi tahu Pyramid bahwa setiap request yang URL-nya dimulai dengan /static/ misalnya, http://localhost:6543/static/app.css harus ditangani oleh view statis ini.

    2) <b> path='tutorial:static' </b>, memberi tahu Pyramid di mana harus mencari file-file tersebut. Sintaks tutorial:static adalah asset specification yang menunjuk ke folder static/ di dalam package tutorial/. Secara singkat, perintah ini memetakan URL /static/ ke direktori fisik tutorial/static/.

2. Menghubungkan Aset di Template (Cara yang Benar)
    Di dalam template home.pt, kita perlu menautkan file CSS kita. Kita bisa saja melakukan hard-coding pada link tersebut:

        <link rel="stylesheet" href="/static/app.css"/>

    Masalahnya, apa yang terjadi jika nanti kita memindahkan seluruh situs ke subdirectory (misalnya, /situsku/)? Atau jika kita memutuskan untuk mengubah name dari static menjadi assets di __init__.py? Semua link kita akan rusak. Pyramid menyediakan helper yang jauh lebih fleksibel untuk menghasilkan URL:

        <link rel="stylesheet"
            href="${request.static_url('tutorial:static/app.css') }"/>

    Fungsi <i> request.static_url() </i> akan secara dinamis menghasilkan URL yang benar berdasarkan konfigurasi kita. Ini memberi kita fleksibilitas penuh untuk refactoring. Kita bisa mengubah name='static' menjadi name='assets' di __init__.py, dan request.static_url() akan secara otomatis menghasilkan link baru (/assets/app.css) di semua template kita tanpa perlu mengubah satu file template pun. Ini membuat konfigurasi dan template Anda tetap sinkron.

![alt text](<Screenshot 2025-11-13 064519.png>)