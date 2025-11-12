<h1> 10: Handling Web Requests and Responses </h1>
Di langkah ini, kita beralih dari template dan mulai bekerja secara langsung dengan objek Request dan Response Pyramid.

1. Redirect HTTP
    Class view sekarang memiliki dua rute. View pertama yaitu home tidak lagi menampilkan halaman, melainkan langsung mengarahkan pengguna ke view kedua yaitu plain.

    Melakukannya dengan mengembalikan objek HTTPFound dari view kita:

        views.py
        from pyramid.httpexceptions import HTTPFound

            @view_config(route_name='home')
        def home(self):
            return HTTPFound(location='/plain')

    Ini adalah cara Pyramid untuk mengirimkan respons HTTP 302 Redirect ke browser, memberi tahu browser untuk secara otomatis memuat URL /plain.

2. Mengakses Data dari Request
    Di view plain, sekarang berinteraksi langsung dengan objek <i> self.request </i> untuk mendapatkan informasi tentang request yang masuk:
    - <i> self.request.url </i>, properti ini memberi kita URL lengkap yang dikunjungi pengguna (misalnya, http://localhost:6543/plain).
    - <i> self.request.params.get(...) </i>, Ini adalah cara mengambil data dari query string (parameter URL).
    - <i> self.request.params.get('name', 'No Name Provided') </i> yang berarti,"Coba dapatkan parameter URL bernama 'name'. Jika tidak ada, gunakan nilai default 'No Name Provided'."
    - Inilah yang membuat URL http://localhost:6543/plain?name=alice bisa menampilkan "alice" di halaman.

3. Membuat Respons Manual
    Alih-alih mengembalikan dictionary dan membiarkan renderer bekerja, view plain sekarang membuat objek Response secara manual. Ini memberi kontrol penuh atas respons HTTP:

        views.py
        from pyramid.response import Response
            @view_config(route_name='plain')
        def plain(self):
        ...
        body = 'URL %s with name: %s' % (self.request.url, name)
        return Response(
            content_type='text/plain',
            body=body
        )

    Perhatikan bahwa secara eksplisit mengatur <i> content_type='text/plain' </i>. Inilah sebabnya mengapa browser menampilkannya sebagai teks mentah, bukan HTML.

4. Tes yang Diperbarui
    Terakhir, memperbarui unit test dan functional test untuk memverifikasi semua fungsionalitas baru ini antara lain:
    - <i> Tes Unit test_home </i>, yang sekarang memeriksa bahwa view home mengembalikan status 302 Found.
    - <i> Tes Baru (test_plain_without_name) </i>, dengan memverifikasi kasus di mana tidak ada parameter name yang diberikan.
    - <i> Tes Baru (test_plain_with_name) </i>, dengan memverifikasi bahwa parameter name berhasil dibaca dan ditampilkan dengan benar di body respons.

![alt text](<Screenshot 2025-11-13 055417.png>)
![alt text](<Screenshot 2025-11-13 055554.png>)