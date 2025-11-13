<h1> 14: AJAX Development With JSON Renderers </h1>
Langkah ini menunjukkan kekuatan dari arsitektur Pyramid, di mana kita dapat dengan mudah menyajikan format data yang berbeda antara HTML dan JSON dari logika view yang sama persis.

1. Keuntungan View Berorientasi Data
    Sebelumnya, kita mengubah view untuk mengembalikan data Python seperti dictionary alih-alih string HTML. Perubahan ini sangat penting karena memisahkan logika yang mana memisahkan logika bisnis Python pada view dari HTML di template.Lalu, memudahkan test untuk unit test kita menjadi lebih bersih. Kita hanya perlu memeriksa apakah dictionary yang dikembalikan sudah benar, bukan lagi parsing string HTML yang rapuh.

2. Satu View, Dua Output (HTML dan JSON)
    Karena view kita sudah mengembalikan dictionary, menyajikannya sebagai JSON menjadi sangat mudah. Pyramid memiliki renderer json bawaan. Perlu memberi tahu Pyramid untuk menggunakan renderer tersebut alih-alih renderer template. Kita tidak membuat method view baru. Sebaliknya, kita "menumpuk" (stack) decorator @view_config kedua di atas method hello yang sudah ada.

        @view_config(route_name='hello')
        @view_config(route_name='hello_json', renderer='json')
        def hello(self):
            return {'name': 'Hello View'}

    Cara kerjanya seperti:
    - Ketika pengguna mengunjungi /howdy yaitu route hello dan decorator pertama cocok. Renderer default pada home.pt dari <i> @view_defaults</i> digunakan. Ketika pengguna mengunjungi /howdy.json di route hello_json dan decorator kedua cocok. Renderer yang ditentukannya 'json' akan menggantikan renderer default, dan Pyramid secara otomatis mengubah dictionary <b> {'name': 'Hello View'} </b> menjadi respons JSON <b> {"name": "Hello View"} </b>.

3. Topik Lanjutan
    Alternatif: Predikat View (AJAX)
    Sebenarnya, bisa saja menggunakan route yang sama (/howdy) untuk menyajikan HTML dan JSON. KDengan menggunakan "view predicate" untuk memeriksa header Accept dari request. Jika request meminta application/json seperti panggilan AJAX, Pyramid akan menggunakan renderer JSON. Jika tidak, ia akan menggunakan renderer template.

4. Batasan Renderer JSON
    Renderer JSON bawaan Pyramid adalah encoder JSON standar Python. Ini berarti ia memiliki batasan yang sama, misalnya tidak bisa mengubah objek DateTime menjadi JSON secara otomatis. Masalah ini bisa diatasi di Pyramid dengan membuat renderer JSON kustom jika diperlukan.

![alt text](<Screenshot 2025-11-13 070615.png>)
![alt text](<Screenshot 2025-11-13 070626.png>)