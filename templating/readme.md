<h1> 08: HTML Generation With Templating </h1>
Perubahan ini adalah kemajuan besar dalam struktur aplikasi kita. Kita telah berhasil memisahkan logika (Python) dari presentasi (HTML), yang membuat kode kita jauh lebih bersih dan mudah dikelola.

1. View yang Fokus pada Data
    Lihatlah file tutorial/views.py kita sekarang. Tidak ada lagi HTML di Python, dengan View yang sekarang murni berisi kode Python. Tugasnya sederhana yaitu memproses request dan menyiapkan data.Dengan mengembalikan dictionary, alih-alih membuat objek Response secara manual, view sekarang hanya mengembalikan dictionary Python.

        def home(request):
        return {'name': 'Home View'}

2. @view_config dan renderer
    Kunci dari keajaiban ini ada di decorator @view_config:

        @view_config(route_name='home', renderer='home.pt')

    Parameter renderer='home.pt' memberi tahu Pyramid dengan <i> "Ambil dictionary yang dikembalikan oleh fungsi ini, temukan file template bernama home.pt, dan gabungkan keduanya." </i> Pyramid, dengan bantuan pyramid_chameleon, menangani semua pekerjaan kotor untuk me-render HTML akhir dan mengirimkannya ke browser.

3. Template yang Dapat Digunakan Kembali
    Perhatikan bahwa kedua view yaitu home dan hello menggunakan template yang sama persis (home.pt). Ini adalah keuntungan besar. Bisa memiliki banyak view yang berbagi tampilan dan nuansa yang sama tanpa duplikasi kode HTML.

4. Dampak Positif pada Testing
    Pemisahan ini membuat pengujian kita jauh lebih bersih dan memiliki fokus yang jelas. Unit Test (Cek Data) adalah tes unit kita (TutorialViewTests) sekarang bisa fokus pada "kontrak data" dari view. Kita tidak perlu lagi memeriksa string HTML yang berantakan. Kita hanya perlu memeriksa apakah view mengembalikan dictionary dengan key dan value yang benar.

        tes.py
        self.assertEqual('Home View', response['name'])

    Functional Test (Cek Tampilan) adalah tes fungsional kita (TutorialFunctionalTests) masih memiliki peran penting. Tes ini memverifikasi bahwa keseluruhan proses bekerja: data dari view dan template berhasil digabungkan untuk menghasilkan HTML akhir yang dilihat pengguna.

        tes.py
        self.assertIn(b'<h1>Hi Home View', res.body)

![alt text](<Screenshot 2025-11-12 232304.png>)