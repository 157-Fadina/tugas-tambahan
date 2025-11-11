<h1> Proyek Hello World Pyramid </h1>

-------

1. Menjalankan Proyek
Pastikan Virtual Environment Aktif Pastikan virtual environment Anda (misalnya %VENV%) sudah aktif di terminal.

2. Install Ketergantungan (Dependencies) Jika belum, install Pyramid dan Waitress:

<b> Bash </b>

    <i> pip install pyramid </i>

    <i> pip install waitress </i>

Jalankan Server Dari dalam folder hello_world, jalankan aplikasi:

<b> Bash </b>

    <i> python app.py </i>

Lihat di Browser Buka browser Anda dan kunjungi http://localhost:6543/

----------
<h3> Penjelasan Kode (app.py) </h3>

1. Titik Masuk (Entry Point) - Baris 11

        <i> if __name__ == '__main__': </i>

    Ini adalah "tombol mulai" Python.

2. Konfigurasi Rute (Routing) - Baris 12-14

        <i> with Configurator() as config:
        config.add_route('hello', '/')
        config.add_view(hello_world, route_name='hello') </i>
        
    Configurator adalah pusat dari aplikasi Pyramid. Bagian ini berfungsi sebagai "peta" untuk situs web:

    <i>config.add_route('hello', '/') </i> untuk mendaftarkan sebuah rute yang memberi tahu Pyramid: "Jika ada yang mengunjungi URL utama (/), dan beri nama rute ini 'hello'."

    <i>config.add_view(hello_world, route_name='hello')</i> untuk menghubungkan rute ke view. Hal ini memberi tahukan Pyramid: "Jika rute bernama 'hello' dipanggil, jalankan fungsi Python bernama hello_world."

3. Logika "View" - Baris 6-8

        <i> def hello_world(request):
        print('Incoming request')
        return Response('<body><.h1>Hello World!</.h1></body>') </i>

    ini adalah fungsi "view" (hello_world) yang dirujuk pada langkah konfigurasi. Hal ini adalah logika sebenarnya yang dijalankan saat pengguna (kita) mengunjungi rute /. Tugasnya adalah menerima request dan mengembalikan jawaban, yang dalam hal ini adalah HTML sederhana.

4. Menjalankan Server - Baris 15-16
   
        <i> app = config.make_wsgi_app()
        serve(app, host='0.0.0.0', port=6543) </i>

    Setelah semua konfigurasi selesai, dua baris ini bertugas untuk menyalakan server:

    <i>app = config.make_wsgi_app()</i> untuk mengubah objek config yang berisi semua aturan Anda menjadi aplikasi web WSGI yang lengkap.

    <i>serve(app, ...)</i> untuk menggunakan server waitress untuk menjalankan aplikasi app tersebut, membuatnya dapat diakses di port 6543.
