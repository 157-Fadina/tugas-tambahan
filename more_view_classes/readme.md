<h1> 15: More With View Classes </h1>
Sekarang dikelompokkan secara logis ke dalam satu class. Ini adalah pola yang sangat umum dan kuat di Pyramid.

1. Pengelompokan View Secara Logis
    Satu class di <b> TutorialViews </b> sekarang menangani beberapa route dan request yang saling terkait:

    - View home, tersedia di http://localhost:6543/. View ini memiliki link yang dapat diklik ke view hello.
    - View hello, dikembalikan saat mengunjungi <b> /howdy/jane/doe </b>. URL ini dipetakan ke route hello, yang telah kita tetapkan sebagai default untuk seluruh class menggunakan @view_defaults.
    - View edit, yang dikembalikan saat formulir dikirim dengan metode POST misalnya, menekan tombol "Save". Aturan ini ditentukan dalam <i> @view_config </i> untuk view tersebut di <b> request_method='POST' </b>.
    - View delete, dikembalikan saat formulir dikirim melalui tombol tertentu, seperti <input type="submit" name="form.delete" value="Delete"/>. Ini menggunakan predikat request_param='form.delete'.

2. Kriteria Pemilihan View (View Predicates)
    Langkah ini menunjukkan bagaimana Pyramid dapat memutuskan view mana yang akan digunakan berdasarkan kriteria (dikenal sebagai view predicates) selain hanya route:
    
    - Metode HTTP, <i> request_method='POST' </i> memastikan view hanya merespons request POST.
    - Parameter Request, <i> request_param='form.delete' </i> memastikan view hanya merespons jika request POST berisi parameter form.delete (artinya tombol delete yang ditekan).

3. Konfigurasi Terpusat
    Kita memusatkan konfigurasi umum ke level class menggunakan @view_defaults:

    - Default pada <i> @view_defaults(route_name='hello') </i> menetapkan bahwa semua view method di class ini akan terikat ke route hello secara default.
    - Override, yang mana view home kemudian mengganti default tersebut dengan konfigurasinya sendiri yaitu <i> @view_config(route_name='home', ...) </i>.

4. Berbagi Logika di Dalam Class
    Keuntungan terbesar menggunakan class adalah kita dapat berbagi logika dan status antar view method dan template:

    - <i> Shared State (__init__), </i> dengan menetapkan <b> self.view_name </b> di __init__, membuatnya tersedia untuk semua method di class.

    - Computed Value (@property), membuat properti full_name yang menghitung nama lengkap dari matchdict. Kedua nilai ini (view_name dan full_name) secara otomatis tersedia di dalam template melalui variabel view:

        ${view.view_name}

        ${view.full_name}

5. Pembuatan URL Dinamis
    Sebagai catatan penting, kita mengubah cara kita membuat URL di template.

        <a href="/howdy/jane/doe">Howdy</a>
    
    Cara Baru (Fleksibel): Di home.pt, kita beralih ke:

        <a href="${request.route_url('hello', first='jane', last='doe')}">form</a>

    Pyramid memiliki fasilitas canggih untuk membantu membuat URL secara fleksibel dan tidak rentan error. Jika kita mengubah pola route hello di __init__.py, request.route_url() akan secara otomatis menghasilkan URL baru yang benar tanpa kita harus mengubah template.

![alt text](<Screenshot 2025-11-13 071918.png>)