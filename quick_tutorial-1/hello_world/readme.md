Proyek Hello World Pyramid

Menjalankan Proyek
Pastikan Virtual Environment Aktif Pastikan virtual environment Anda (misalnya %VENV%) sudah aktif di terminal.

Install Ketergantungan (Dependencies) Jika belum, install Pyramid dan Waitress:

Bash
pip install pyramid
pip install waitress
Jalankan Server Dari dalam folder hello_world, jalankan aplikasi:

Bash
python app.py
Lihat di Browser Buka browser Anda dan kunjungi http://localhost:6543/

Penjelasan Kode (app.py)

1. Titik Masuk (Entry Point) - Baris 11

if __name__ == '__main__':
Ini adalah "tombol mulai" Python.

2. Konfigurasi Rute (Routing) - Baris 12-14

    with Configurator() as config:
        config.add_route('hello', '/')
        config.add_view(hello_world, route_name='hello')
Configurator adalah pusat dari aplikasi Pyramid. Bagian ini berfungsi sebagai "peta" untuk situs web:

config.add_route('hello', '/'): Mendaftarkan sebuah rute yang memberi tahu Pyramid: "Jika ada yang mengunjungi URL utama (/), dan beri nama rute ini 'hello'."

config.add_view(hello_world, route_name='hello'): Menghubungkan rute ke view. Ini memberi tahu Pyramid: "Jika rute bernama 'hello' dipanggil, jalankan fungsi Python bernama hello_world."

3. Logika "View" - Baris 6-8

def hello_world(request):
    print('Incoming request')
    return Response('<body><h1>Hello World!</h1></body>')

Ini adalah fungsi "view" (hello_world) yang dirujuk pada langkah konfigurasi. Hal ini adalah logika sebenarnya yang dijalankan saat pengguna (kita) mengunjungi rute /. Tugasnya adalah menerima request dan mengembalikan jawaban, yang dalam hal ini adalah HTML sederhana.

4. Menjalankan Server - Baris 15-16
Python

        app = config.make_wsgi_app()
    serve(app, host='0.0.0.0', port=6543)

Setelah semua konfigurasi selesai, dua baris ini bertugas untuk menyalakan server:

app = config.make_wsgi_app(): Mengubah objek config yang berisi semua aturan Anda menjadi aplikasi web WSGI yang lengkap.

serve(app, ...): Menggunakan server waitress untuk menjalankan aplikasi app tersebut, membuatnya dapat diakses di port 6543.