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
    ```bash
      void changePath(char *fpath, const char *path, int isWriteOper, int isFileAsked) {
      char *ptr = strstr(path, "/encv1_");
      int state = 0;
      if (ptr != NULL) {
        if (strstr(ptr+1, "/") != NULL) state = 1;
      }
      char fixPath[1000];
      memset(fixPath, 0, sizeof(fixPath));
      if (ptr != NULL && state) {
        ptr = strstr(ptr+1, "/");
        char pathEncvDirBuff[1000];
        char pathEncryptedBuff[1000];
        strcpy(pathEncryptedBuff, ptr);
        strncpy(pathEncvDirBuff, path, ptr-path);
        if (isWriteOper) {
          char pathFileBuff[1000];
          char pathDirBuff[1000];
          getDirAndFile(pathDirBuff, pathFileBuff, pathEncryptedBuff);
          decrypt(pathDirBuff, 0);
          sprintf(fixPath, "%s%s/%s", pathEncvDirBuff, pathDirBuff, pathFileBuff);
        } else if (isFileAsked) {
          char pathFileBuff[1000];
          char pathDirBuff[1000];
          char pathExtBuff[1000];
          getDirAndFile(pathDirBuff, pathFileBuff, pathEncryptedBuff);
          char *whereIsExtension = strrchr(pathFileBuff, '.');
          if (whereIsExtension-pathFileBuff <= 1) {
            decrypt(pathDirBuff, 0);
            decrypt(pathFileBuff, 0);
            sprintf(fixPath, "%s%s/%s", pathEncvDirBuff, pathDirBuff, pathFileBuff);
          } else {
            char pathJustFileBuff[1000];
            memset(pathJustFileBuff, 0, sizeof(pathJustFileBuff));
            strcpy(pathExtBuff, whereIsExtension);
            snprintf(pathJustFileBuff, whereIsExtension-pathFileBuff+1, "%s", pathFileBuff);
            decrypt(pathDirBuff, 0);
            decrypt(pathJustFileBuff, 0);
            sprintf(fixPath, "%s%s/%s%s", pathEncvDirBuff, pathDirBuff, pathJustFileBuff, pathExtBuff);
          }
        } else {
          decrypt(pathEncryptedBuff, 0);
          sprintf(fixPath, "%s%s", pathEncvDirBuff, pathEncryptedBuff);
        }
      } else {
        strcpy(fixPath, path);
      }
      if (strcmp(path, "/") == 0) {
        sprintf(fpath, "%s", dirpath);
      } else {
        sprintf(fpath, "%s%s", dirpath, fixPath);
      }
    }
    ```
    Fungsi ```void changePath(char *fpath, const char *path, int isWriteOper, int isFileAsked) {``` akan melakukan pencarian terhadap _file name_ dengan nama __"encv1 _ "__. Kemudian, akan ada _pointer_ pada file tersebut dan ```int state = 0;``` state-nya menjadi __0__. Namun, ```if (ptr != NULL) {``` apabila _file name_ tidak bernama __"encv1 _ "__ dan ``` if (strstr(ptr+1, "/") != NULL) ``` apabila _file name_ bernama  __"encv1 _ "__ diikuti dengan _path_ yang berlanjut maka ```state = 1;``` state-nya menjadi __1__. <br>
    Tidak lupa untuk mendefinisikan __buffer__ yang merupakan tempat penyimpanan _processed path_ dari 3 (tiga) keperluan yang berbeda (__1.0__, __0.1__ dan __0.0__).
    ```bash
    char fixPath[1000];
    memset(fixPath, 0, sizeof(fixPath));
    ```
    Terdapat satu kondisi yang terdapat di dalam fungsi __fixPath__ yang akan bekerja untuk kondisi __1.0__ dan __0.1__ yang 
  akan mendefinisikan dan mengisi _buffer_.
    ```bash 
    if (ptr != NULL && state) {
    ptr = strstr(ptr+1, "/");
    char pathEncvDirBuff[1000];
    char pathEncryptedBuff[1000];
    strcpy(pathEncryptedBuff, ptr);
    strncpy(pathEncvDirBuff, path, ptr-path);
    ```
    Pada ```strncpy(pathEncvDirBuff, path, ptr-path);``` akan melakukan _copy_ dari _path_ ke __EncvDirBuff__. <br>
    ```bash
    if (isWriteOper) {
      char pathFileBuff[1000];
      char pathDirBuff[1000];
      getDirAndFile(pathDirBuff, pathFileBuff, pathEncryptedBuff);
      decrypt(pathDirBuff, 0);
      sprintf(fixPath, "%s%s/%s", pathEncvDirBuff, pathDirBuff, pathFileBuff);
    } 
    ```
    Apabila hanya kondisi __1.0__ ```if (isWriteOper) {```  yang memiliki nilai, maka  ```char pathFileBuff[1000];``` akan mendefinisikan _file path_ dan ```char pathDirBuff[1000];``` akan mendefinisikan _directory path_ kemudian  menjalankan fungsi ```getDirAndFile(pathDirBuff, pathFileBuff, pathEncryptedBuff);``` yang bertujuan untuk mengembalikan _file path_ dan _directory path_ sesuai dengan _path_ yang telah tersimpan di dalam __pathEncryptedBuff__ dan dilanjutkan dengan proses ```decrypt(pathDirBuff, 0);``` pendeskripsian dan menyimpannya di dalam _buffer_ __fixPath__```sprintf(fixPath, "%s%s/%s", pathEncvDirBuff, pathDirBuff, pathFileBuff);``` dengan format "__encv1 _ , directory path, file path__". <br>
    ```bash 
    else if (isFileAsked) {
      char pathFileBuff[1000];
      char pathDirBuff[1000];
      char pathExtBuff[1000];
      getDirAndFile(pathDirBuff, pathFileBuff, pathEncryptedBuff);
      char *whereIsExtension = strrchr(pathFileBuff, '.');
      if (whereIsExtension-pathFileBuff <= 1) {
        decrypt(pathDirBuff, 0);
        decrypt(pathFileBuff, 0);
        sprintf(fixPath, "%s%s/%s", pathEncvDirBuff, pathDirBuff, pathFileBuff);
      } else {
        char pathJustFileBuff[1000];
        memset(pathJustFileBuff, 0, sizeof(pathJustFileBuff));
        strcpy(pathExtBuff, whereIsExtension);
        snprintf(pathJustFileBuff, whereIsExtension-pathFileBuff+1, "%s", pathFileBuff);
        decrypt(pathDirBuff, 0);
        decrypt(pathJustFileBuff, 0);
        sprintf(fixPath, "%s%s/%s%s", pathEncvDirBuff, pathDirBuff, pathJustFileBuff, pathExtBuff);
      }
      ```
      Untuk kondisi lainnya yakni apabila hanya kondisi __0.1__ ```else if (isFileAsked) {``` yang memiliki nilai, maka ```char pathFileBuff[1000];``` akan mendefinisikan _file path_, ```char pathDirBuff[1000];``` akan mendefinisikan _directory path_ serta ```char pathExtBuff[1000];``` akan mendefinisikan ekstensi dari file tersebut. Kemudian, _file path_ dan _directory path_ akan disimpan terlebih dahulu ke dalam __encrypted buffer__ menggunakan fungsi ```getDirAndFile(pathDirBuff, pathFileBuff, pathEncryptedBuff);```. Setelah melakukan penyimpanan _path_ ke dalam _buffer_, kita akan melakukan ```char *whereIsExtension = strrchr(pathFileBuff, '.');``` pencarian ekstensi dari file melalui _path_ yang telah ditentukan dengan melakukan pengecekan karakter ekstensi. Apabila ```if (whereIsExtension-pathFileBuff <= 1) {```tidak ditemukan ekstensi sesuai dengan _path_ yang ditentukan atau ditemukan ekstensi sesuai dengan _path_ yang ditentukan sejumlah 1 karakter, , maka akan dilakukan proses pendeskripsian ``` decrypt(pathDirBuff, 0);``` pada _directory path_ dan ```decrypt(pathFileBuff, 0);``` pada _file path_. Kemudian, hasil deskripsi tersebut disimpan ke dalam __fixPath__ ``` sprintf(fixPath, "%s%s/%s", pathEncvDirBuff, pathDirBuff, pathFileBuff);```. dengan format "__encv1 _ , directory path, file path__".
Begitupun apabila ``` else {``` ditemukan ekstensi sesuai dengan _path_ yang ditentukan sejumlah lebih dari 1 karakter, maka akan dilakukan penyimpanan ekstensi dari file tersebut ke dalam _file path_ dan dilanjutkan dengan proses pendeskripsian ``` decrypt(pathDirBuff, 0);``` pada _directory path_ dan ```decrypt(pathFileBuff, 0);``` pada _file path_. Dan hasil deskripsi tersebut disimpan ke dalam __fixPath__ ```sprintf(fixPath, "%s%s/%s%s", pathEncvDirBuff, pathDirBuff, pathJustFileBuff, pathExtBuff);``` dengan format "__encv1 _ , directory path, file path, __ekstensi__".
      ```bash
        else {
        decrypt(pathEncryptedBuff, 0);
        sprintf(fixPath, "%s%s", pathEncvDirBuff, pathEncryptedBuff);
      }
      ```
      Fungsi di atas akan bekerja pada kondisi __0.0__ dimanan kondisi __1.0__ dan kondisi __0.1__ tidak memiliki nilai, maka ``` decrypt(pathEncryptedBuff, 0);``` akan terjadi proses pendeskripsian pada isi dari _buffer_ tersebut dan kemudian ``` sprintf(fixPath, "%s%s", pathEncvDirBuff, pathEncryptedBuff);``` disimpan di dalam __fixPath__ dengan format "__encv1 _ , decrypt path__".
      ```bash
          else {
        strcpy(fixPath, path);
      }
      ```
      Fungsi di atas berjalan pada state bernilai 0 yang dimana hanya ada 1 (satu) _directory_ atau _file_ pada _path_ yang berada setelah __"encv1 _ "__, maka ```strcpy(fixPath, path);``` _path_ akan langsung disimpan di dalam __fixPath__. <br>
      Lalu, untuk fungsi _ __getattr__ berikut ini:
      ```bash
	static int _getattr(const char *path, struct stat *stbuf)
	{
		char fpath[1000];
	  	changePath(fpath, path, 0, 1);
	  	if (access(fpath, F_OK) == -1) {
	    		memset(fpath, 0, sizeof(fpath));
	    		changePath(fpath, path, 0, 0);
	  	}
	``` 
    Fungsi __changePath()__ dijalankan oleh _system call_ untuk kondisi __0.1__ yang kemudian akan dilakukan ```if (access(fpath, F_OK) == -1) {``` pengecekan mengenai apakah _path_ yang diinputkan sudah menjadi  _file_ atau _directory_ dan apakah _path_ tersebut sudah terenkripsi. Jika _path_ belum berhasil diinputkan menjadi _file_ atau _directory_, maka ```changePath(fpath, path, 0, 0);``` fungsi __changePath()__ dijalankan untuk kondisi __0.0__. <br>
    Kemudian, fungsi selanjutnya yang akan dijalankan adalah fungsi _ __access__ yang berguna untuk melakukan _permission check_ pada pengguna.
    ```bash
    static int _access(const char *path, int mask)
    {
	char fpath[1000];
	changePath(fpath, path, 0, 1);
	if (access(fpath, F_OK) == -1) {
	    memset(fpath, 0, 1000);
	    changePath(fpath, path, 0, 0);
	}

	int res;

	res = access(fpath, mask);

	const char *desc[] = {path};
	logFile("INFO", "ACCESS", res, 1, desc);

	if (res == -1) return -errno;


	return 0;
	}
	```
	akmsakmsska
    
      
      
