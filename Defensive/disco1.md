Kategori : Forensics
Tingkat Kesulitan : Easy
Tujuan : Mengekstraksi flag tersembunyi dari sebuah file disk image

1. Persiapan Alat
Untuk menganalisis disk image, kita memerlukan beberapa alat forensik dasar Web Shell picoCTF
strings: Untuk mencari teks yang dapat dibaca manusia di dalam file biner
grep: Untuk menyaring hasil pencarian

2. Langkah-Langkah Penyelesaian
Metode 1: Quick Win

strings [disko-1.gg.dz].img | grep "picoCTF"

3. Execute
Setelah berhasil dijalankan akan muncul flag yang menjadi jawaban dari task tersebut.