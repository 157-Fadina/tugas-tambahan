<h1> 11: Dispatching URLs To Views With Routing </h1>
Langkah ini memperkenalkan konsep routing yang jauh lebih kuat, yang memungkinkan URL kita menjadi dinamis.

1. Route dengan "Replacement Patterns"
    Perubahan terpenting terjadi di <b> tutorial/__init__.py </b>:

        config.add_route('home', '/howdy/{first}/{last}')

    Bagian {first} dan {last} adalah "replacement patterns" (pola pengganti). Ini memberi tahu Pyramid bahwa route home tidak hanya cocok dengan satu URL statis, tetapi dengan pola URL apa pun yang sesuai.

    Sebagai contoh, ketika seorang pengguna mengunjungi:
        /howdy/amy/smith

    Pyramid akan mencocokkan ini dan secara otomatis menetapkan:
    - first = "amy"
    - last = "smith"

2. Mengakses Data URL di View
    Di dalam view <b> tutorial/views.py </b>, kita sekarang dapat mengakses nilai-nilai dinamis ini melalui dictionary khusus pada objek request yang disebut matchdict:

        first = self.request.matchdict['first']
        last = self.request.matchdict['last']
    
    <i> request.matchdict </i> berisi semua nilai dari URL yang cocok dengan replacement patterns yang kita tentukan di route. Informasi ini kemudian dapat digunakan di dalam view kita, seperti meneruskannya ke template untuk ditampilkan kepada pengguna.

![alt text](<Screenshot 2025-11-13 061633.png>)
![alt text](<Screenshot 2025-11-13 062040.png>)