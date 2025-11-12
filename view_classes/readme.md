<h1> 09: Organizing Views With View Classes </h1>
Untuk mempermudah transisi, jadi tidak menambahkan fungsionalitas baru pada langkah ini. Disini hanya mengubah view yang berbasis fungsi menjadi method di dalam sebuah view class. Akibatnya, unit test juga harus diperbarui agar sesuai.

1. Pengelompokan Logis
    Di dalam class TutorialViews, bisa melihat bahwa dua view sekarang yang dikelompokkan secara logis sebagai method pada class yang sama. Karena kedua view tersebut menggunakan renderer  yang sama, jadi disini hanya memindahkan konfigurasi tersebut ke decorator <i> @view_defaults </i> di level class. Ini membersihkan kode kita dan mengurangi duplikasi.

2. Memperbarui Unit Test
    Unit test perlu diubah agar bisa bekerja dengan class. (Catatan: Functional test tidak berubah karena aplikasi masih terlihat sama dari luar). Perubahan utama pada unit test adalah:
    
    - Kita sekarang mengimpor class TutorialViews, bukan lagi fungsi home dan hello.
    - Kita mengikuti pola baru untuk setiap tes, yaitu
        * Membuat instance dari view class dengan dummy request <i> (inst = TutorialViews(request)) </i>.
        * Memanggil method view yang sedang diuji pada instance tersebut <i> (response = inst.home()) </i>