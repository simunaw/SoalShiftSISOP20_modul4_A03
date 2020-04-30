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

jadi dari soal tersebut kita disuruh membuat program fuse yg mengarah ke direktori /home/user/Documents, jika di run pasti nanti mengarah ke suatu folder tujuan buat ngemount isi-isinya /home/user/Documents. intinya di /home/user/Documents itu bagaimana caranya jika kita membuat folder disitu dengan nama folder berawalan encv1_ maka folder tsb beserta isi-isinya akan di enkripsi namanya dengan metode enkripsi 1. Trus jika folder tersub di rename tanpa ada nama "encv1_" nanti isi-isi dari foldernya akan terdekrip namanya seperti awal.

Terus setiap perubahan yg kita lakukan baik itu mkdir atau rename akan tercatat pada sebuah database log.

#define FUSE_USE_VERSION 28

#include <fuse.h>

#include <stdio.h>

#include <string.h>

#include <unistd.h>

#include <fcntl.h>

#include <dirent.h>

#include <errno.h>

#include <sys/time.h>

#include <stdlib.h>

#include <sys/wait.h>

#include <syslog.h>

#include <sys/statfs.h>

#include <sys/types.h>

#include <sys/stat.h>

#include <stdlib.h>

static const char *dirpath = "/home/fasijardiq/Documents";

char chisper[100] = "9(ku@AW1[Lmvgax6q`5Y2Ry?+sF!^HKQiBXCUSe&0M.b%rI'7d)o4~VfZ*{#:}ETt$3J-zpc]lnh8,GwP_ND|jO";

void ekrip (char * word)
{
  
  int a;
  
  int b;
  
  if(!strcmp(word,".") || !strcmp(word,"..")) return;
  
  for(a=0;a<(strlen(word));a++)
  
  {
    
    printf("%c",word[a]);
    
    if(word[a]!='/')
    
    {
      
      for(b=0;b<(strlen(chisper));b++)
      
      {
        
	if(chisper[b]==word[a])
        
	{
          
	  int c = (b + 10) % 87;
          
	  word[a] = chisper[c];
          
	  break;
	}
      }
    }	
  }
}

void dkrip (char * word)

{
  
  int a;
  
  int b;
  
  if(!strcmp(word,".") || !strcmp(word,"..")) return;
  
  for(a=0;a<(strlen(word));a++)
  
  {
  
  printf("%c",word[a]);
  
  if(word[a]!='/')
  
  {
    
    for(b=0;b<(strlen(chisper));b++)
     
     {
	
	if(chisper[b]==word[a])
	
	{
	  
	  int c = (b + 87-10) % 87;
	  
	  word[a] = chisper[c];
	  
	  break;
	}
      }
    }	
  }
}

static int xmp_getattr(const char *path, struct stat *stbuf) //mengembalikan informasi penting tentang setiap file yang berada di 

sistem file kita dengan mengisi struktur tipe stat.
{
  int res;
  
  char fpath[1000];
  
  sprintf(fpath, "%s%s", dirpath, path);
  
  res = lstat(fpath, stbuf);
  
  if (res == -1)
    
    return -errno;
  
  return 0;
}

static int xmp_readdir(const char *path, void *buf, fuse_fill_dir_t filler, off_t offset, struct fuse_file_info *fi) // read directory 
{
  char fpath[1000];
  
  if (!strcmp(fpath, "/"))
  {
    path = dirpath;
    
    strcpy(fpath, path);
  }
  
  else sprintf(fpath, "%s%s", dirpath, path);
  
  DIR *dp;
  
  struct dirent *de;
  
  (void)offset;
  
  (void)fi;
  
  dp = opendir(fpath);
  
  if (dp == NULL)
    
    return -errno;
 
 while ((de = readdir(dp)) != NULL)
  
  {
    struct stat st;
    
    memset(&st, 0, sizeof(st));
    
    st.st_ino = de->d_ino;
    
    st.st_mode = de->d_type << 12;
    
    if (filler(buf, de->d_name, &st, 0)) 
     
     break;
  }
  
  closedir(dp);
  
  return 0;
}

static int xmp_read(const char *path, char *buf, size_t size, off_t offset, struct fuse_file_info *fi) //Read data from an open file

{
  
  char fpath[1000];
  
  if (!strcmp(fpath, "/"))
 
 {
   
   path = dirpath;
   
   strcpy(fpath, path);
  }
 
 else sprintf(fpath, "%s%s", dirpath, path);
  
  int res = 0;
  
  int fd = 0;
  
  (void)fi;
  
  fd = open(fpath, O_RDONLY);
  
  if (fd == -1)
   
   return -errno;
  
  res = pread(fd, buf, size, offset);
 
 if (res == -1)
    
    res = -errno;
  
  close(fd);
  
  return res;
}

static int xmp_rename(const char *from, const char *to) //Rename a file

{
    int res;
    
    char ffrom[1000];
    
    char fto[1000];
    
    system("mkdir /home/fasijardiq/Dokumen");
    
    char direktori[] = "/home/fasijardiq/Dokumen";
    
    system("zenity --info --text=\"INI ADALAH INFO: file telah direname dan dipindahkan ke Home\" --title=\"INFO\"");
    
    sprintf(ffrom,"%s%s",dirpath,from);
    
    sprintf(fto,"%s%s",direktori,to);
      
      res = rename(ffrom, fto);
   
   if(res == -1)
      
      return -errno;
   
   return 0;
}

static int xmp_truncate(const char *path, off_t size)

{
  
  int res = truncate(path, size);

	if (res == -1)

return -errno;

return 0;
}

static struct fuse_operations xmp_oper = // pointer yang menuju ke fungsi

{

.getattr = xmp_getattr,

.readdir = xmp_readdir,

.read = xmp_read,

.rename = xmp_rename,

.truncate = xmp_truncate,

};

int main(int argc, char *argv[]) //fungsi main (userspace), program user memanggil fungsi fuse_main() kemudian fungsi fuse_mount() 

dipanggil.

{

umask(0);

return fuse_main(argc, argv, &xmp_oper, NULL);

}




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


