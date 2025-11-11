<h1> 02: Python Packages for Pyramid Applications </h1>

-----

1. Tutorial Aplikasi Python (Dasar Package)

    Repositori ini berisi langkah awal dalam tutorial pengembangan aplikasi web Python, yang berfokus pada cara menstrukturkan proyek sebagai *package* Python yang dapat diinstal.

2. Setup dan Instalasi

    Untuk menyiapkan proyek ini, Anda harus menginstalnya dalam **mode pengembangan (editable)**. Mode ini, yang diaktifkan oleh flag `-e`, memungkinkan Anda untuk mengedit kode sumber dan melihat perubahan secara langsung tanpa perlu menginstal ulang.

3. Jalankan perintah berikut dari direktori *root* (direktori yang berisi file `setup.py`):
    
    <b> Bash </b>
    Menginstal paket 'tutorial' dan dependensinya (seperti pyramid, waitress)
        
        pip install -e .

4. Setelah instalasi selesai, Anda dapat menjalankan server aplikasi.

    <b> Bash </b>
        python tutorial/app.py

5. Server akan dimulai, dan Anda dapat mengakses aplikasi di http://localhost:6543.

-----

<h3> Analisis </h3>
    Cara kita menjalankan aplikasi saat ini <b>(python tutorial/app.py)</b> bukanlah praktik standar untuk aplikasi Python yang sudah di-package. Menjalankan modul Python di dalam package secara langsung seperti ini umumnya dihindari.Metode ini digunakan hanya untuk tujuan tutorial ini, agar dapat menunjukkan cara kerja setiap komponen secara bertahap. Di langkah-langkah selanjutnya, kita akan beralih ke cara yang lebih profesional untuk menjalankan aplikasi.