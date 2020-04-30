# SoalShiftSISOP20_modul4_T17
Nama Anggota Kelompok T17 :
  1. Faza Murtadho [05311840000034]
  2. Rindi Kartika Sari [05311840000013]

## Soal Shift Modul 4 dan Penyelesaian Soal Shift Modul 4
### Soal 1
Jasir, pekerja baru yang jenius, akan membuat program yang mengimplementasikan 2 (dua) buah metode enkripsi yang berjalan pada _directory_. Pada metode enkripsi pertama __"encv1 _ "__ dirancang dengan detail filesystem berikut ini :
  * Jika terdapat sebuah _directory_ yang dibuat dan di-_rename_ dengan awalan kata __"encv1 _ "__, maka _directory_ tersebut akan terenkripsi menggunakan metode enkripsi pertama __"encv1 _ "__. 
  * Jika terdapat sebuah _directory_ terenkripsi yang di-_rename_, maka _directory_ tersebut menjadi tidak terenkripsi. 
  * Setiap pembuatan _directory_ terenkripsi baru (__mkdir__ ataupun __rename__) akan tercatat ke sebuah database/log dalam file. 
  * Proses pengenkripsian dalam metode enkripsi pertama ini menggunakan metode enkripsi _caesar cipher_ dengan _key_ 
    ```bash 
    9(ku@AW1[Lmvgax6q`5Y2Ry?+sF!^HKQiBXCUSe&0M.b%rI'7d)o4~VfZ*{#:}ETt$3J-zpc]lnh8,GwP_ND|jO
    ```
  * Metode enkripsi pada suatu direktori juga berlaku kedalam direktori lainnya yang ada didalamnya. <br>
__Note:__ Metode enkripsi pertama ini mengabaikan karakter ‘/’ pada penamaan file dan ekstensi dari file. 
### Soal 2
Pada metode enkripsi kedua __"encv2 _ "__ dirancang dengan detail filesystem berikut ini :
  * Jika terdapat sebuah _directory_ yang dibuat dan di-_rename_ dengan awalan kata __"encv2 _ "__, maka _directory_ tersebut akan terenkripsi menggunakan metode enkripsi pertama __"encv2 _ "__. 
  * Jika terdapat sebuah _directory_ terenkripsi yang di-_rename_, maka _directory_ tersebut menjadi tidak terenkripsi. 
  * Setiap pembuatan _directory_ terenkripsi baru (__mkdir__ ataupun __rename__) akan tercatat ke sebuah database/log dalam file. 
  * Metode enkripsi pada suatu direktori juga berlaku kedalam direktori lainnya yang ada didalamnya.
  * Pada metode enkripsi kedua, file-file pada direktori asli akan menjadi bagian-bagian kecil sebesar 1024 bytes dan menjadi normal ketika diakses melalui filesystem rancangan jasir.
### Soal 3
### Soal 4
  #### Code :
  #### Penyelesaian :
  Untuk mendukung pembuatan program mengenai metode enkripsi tersebut, maka kita harus membuat fungsi __system call__ yang bertujuan untuk mengatur 3 (tiga) keperluan tertentu sesuai permintaan soal :
  * ```mkdir``` : kondisi dimana terjadi pembuatan _directory_ yang telah ditentukan.
  * ```create``` : kondisi dimana terjadi proses _rename_ pada _directory_ sesuai dengan nama yang telah ditentukan.
  * ```write``` : kondisi dimana terjadi penyimpanan perubahan pada database/log pada file. <br>
  
Oleh karena itu, untuk setiap __system call__ yang telah ditentukan maka kita memanggil 3 (tiga) fungsi antara lain :
  * Fungsi pertama adalah fungsi __changePath()__
