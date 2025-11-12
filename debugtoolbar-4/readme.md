<h1> 04: Easier Development with debugtoolbar </h1>
Menambahkan <i> pyramid_debugtoolbar </i> ke proyek. Ini adalah add-on (paket tambahan) untuk Pyramid yang sangat berguna selama proses pengembangan (development).

1. Instalasi Menggunakan <b> "Extras" ([dev]) </b>
    Daripada meng-install toolbar ini sebagai dependensi utama, tapi meng-installnya sebagai "extra". Kenapa? Karena toolbar ini hanya dibutuhkan saat coding. Kita tidak mau toolbar ini ikut ter-install saat aplikasi kita sudah jadi dan dipakai publik.

    Inilah gunanya bagian extras_require di dalam file setup.py:

    - setup.py
    
        'extras_require={
        'dev': ['pyramid_debugtoolbar'],
        },


    - Kita memberi tahu Python bahwa <i> Hanya jika saya minta, install dependensi untuk dev (development) </i>. Lalu, menjalankannya dengan perintah:
    
        pip install -e ".[dev]"
    
    Tanda . berarti "install proyek di folder ini", dan [dev] berarti <i> install juga semua dependensi yang ada di bagian dev" </i>.

2. Aktivasi Menggunakan <b> .ini (pyramid.includes) </b>
    Setelah ter-install, toolbar ini belum aktif. Kita harus "menyalakannya". Daripada mengubah kode Python kita <b>(__init__.py) </b>, Pyramid mengizinkan kita menyalakannya langsung dari file konfigurasi .ini. Ini sangat praktis karena kita bisa punya konfigurasi berbeda untuk development dan production.

    Kita menambahkannya di development.ini:

        [app:main]
        use = egg:tutorial
        pyramid.includes =
        pyramid_debugtoolbar

    Baris <i> pyramid.includes </i> memberi tahu Pyramid untuk memuat dan mengaktifkan add-on <i> pyramid_debugtoolbar </i> saat aplikasi berjalan.

3. Apa Manfaatnya?
    Saat Anda membuka http://localhost:6543/ sekarang, Anda akan melihat dua hal utama:
    - Tombol toolbar, terdapat tombol kecil di sisi kanan layar. Jika diklik, akan muncul panel penuh informasi debug tentang halaman (konfigurasi, header, request, dll).

    - Halaman error yang jauh lebih baik, misal jika kode error, tidak akan lagi melihat halaman crash yang membingungkan.Akan disajikan traceback (laporan error) yang interaktif dan detail, yang menunjukkan dengan tepat di mana letak kesalahannya.

4. Catatan Penting (Cara Menonaktifkan)
    Toolbar ini bekerja dengan cara "menyuntikkan" sedikit kode HTML dan CSS ke halaman Anda.

    - Jika halaman web Anda tiba-tiba terlihat aneh (misalnya, layout-nya berantakan), coba nonaktifkan toolbar ini untuk melihat apakah itu penyebabnya.

    - Cara menonaktifkannya sangat mudah: Cukup beri tanda # (komentar) di depan baris pyramid_debugtoolbar di file development.ini Anda dan simpan. Server akan me-restart, dan toolbar itu akan hilang.

        [app:main]
        pyramid.includes =
        #pyramid_debugtoolbar