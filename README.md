# SoalShiftSISOP20_modul4_A03

kelompok A03
1. Abdullatif Fajar Sidiq (05111840007002)
2. siti munawroh (05111840007004)


1.)Enkripsi versi 1:

a. Jika sebuah direktori dibuat dengan awalan “encv1_”, maka direktori tersebut akan menjadi direktori terenkripsi menggunakan metode enkripsi v1.
b. Jika sebuah direktori di-rename dengan awalan “encv1_”, maka direktori tersebut akan menjadi direktori terenkripsi menggunakan metode enkripsi v1.
c. Apabila sebuah direktori terenkripsi di-rename menjadi tidak terenkripsi, maka isi adirektori tersebut akan terdekrip.
d. Setiap pembuatan direktori terenkripsi baru (mkdir ataupun rename) akan tercatat ke sebuah database/log berupa file.
e. Semua file yang berada dalam direktori ter enkripsi menggunakan caesar cipher dengan key.

9(ku@AW1[Lmvgax6q`5Y2Ry?+sF!^HKQiBXCUSe&0M.b%rI'7d)o4~VfZ*{#:}ETt$3J-zpc]lnh8,GwP_ND|jO

f. Metode enkripsi pada suatu direktori juga berlaku kedalam direktori lainnya yang ada didalamnya.

2.)Enkripsi versi 2:

a. Jika sebuah direktori dibuat dengan awalan “encv2_”, maka direktori tersebut akan menjadi direktori terenkripsi menggunakan metode enkripsi v2.
b. Jika sebuah direktori di-rename dengan awalan “encv2_”, maka direktori tersebut akan menjadi direktori terenkripsi menggunakan metode enkripsi v2.
c. Apabila sebuah direktori terenkripsi di-rename menjadi tidak terenkripsi, maka isi direktori tersebut akan terdekrip.
d. Setiap pembuatan direktori terenkripsi baru (mkdir ataupun rename) akan tercatat ke sebuah database/log berupa file.
e. Pada enkripsi v2, file-file pada direktori asli akan menjadi bagian-bagian kecil sebesar 1024 bytes dan menjadi normal ketika diakses melalui filesystem rancangan jasir. Sebagai contoh, file File_Contoh.txt berukuran 5 kB pada direktori asli akan menjadi 5 file kecil yakni: File_Contoh.txt.000, File_Contoh.txt.001, File_Contoh.txt.002, File_Contoh.txt.003, dan File_Contoh.txt.004.
f. Metode enkripsi pada suatu direktori juga berlaku kedalam direktori lain yang ada didalam direktori tersebut (rekursif).

3.) Sinkronisasi direktori otomatis:

Tanpa mengurangi keumuman, misalkan suatu directory bernama dir akan tersinkronisasi dengan directory yang memiliki nama yang sama dengan awalan sync_ yaitu sync_dir. Persyaratan untuk sinkronisasi yaitu:
a. Kedua directory memiliki parent directory yang sama.
b. Kedua directory kosong atau memiliki isi yang sama. Dua directory dapat dikatakan memiliki isi yang sama jika memenuhi:
      i.) Nama dari setiap berkas di dalamnya sama.
      ii.) Modified time dari setiap berkas di dalamnya tidak berselisih lebih dari 0.1 detik.
c. Sinkronisasi dilakukan ke seluruh isi dari kedua directory tersebut, tidak hanya di satu child directory saja.
d. Sinkronisasi mencakup pembuatan berkas/directory, penghapusan berkas/directory, dan pengubahan berkas/directory.


4.) Log system:

a. Sebuah berkas nantinya akan terbentuk bernama "fs.log" di direktori *home* pengguna (/home/[user]/fs.log) yang berguna menyimpan daftar perintah system call yang telah dijalankan.
b. Agar nantinya pencatatan lebih rapi dan terstruktur, log akan dibagi menjadi beberapa level yaitu INFO dan WARNING.
c. Untuk log level WARNING, merupakan pencatatan log untuk syscall rmdir dan unlink.
d. Sisanya, akan dicatat dengan level INFO.
e. Format untuk logging yaitu:

[LEVEL]::[yy][mm][dd]-[HH]:[MM]:[SS]::[CMD]::[DESC ...]


LEVEL    : Level logging
yy   	 : Tahun dua digit
mm    	 : Bulan dua digit
dd    	 : Hari dua digit
HH    	 : Jam dua digit
MM    	 : Menit dua digit
SS    	 : Detik dua digit
CMD     	 : System call yang terpanggil
DESC      : Deskripsi tambahan (bisa lebih dari satu, dipisahkan dengan ::)


