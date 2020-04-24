# SoalShiftSISOP20_modul4_A11

## Kevin Angga Wijaya - 05111840000024
## Julius - 05111840000082
![image](https://user-images.githubusercontent.com/60419316/80186560-617b9100-8638-11ea-8c27-9e6a4b37459d.png)

## Soal
Di suatu perusahaan, terdapat pekerja baru yang super jenius, ia bernama jasir. Jasir baru bekerja selama seminggu di perusahan itu, dalam waktu seminggu tersebut ia selalu terhantui oleh ketidak amanan dan ketidak efisienan file system yang digunakan perusahaan tersebut. Sehingga ia merancang sebuah file system yang sangat aman dan efisien. Pada segi keamanan, Jasir telah menemukan 2 buah metode enkripsi file. Pada mode enkripsi pertama, nama file-file pada direktori terenkripsi akan dienkripsi menggunakan metode substitusi. Sedangkan pada metode enkripsi yang kedua, file-file pada direktori terenkripsi akan di-split menjadi file-file kecil. Sehingga orang-orang yang tidak menggunakan filesystem rancangannya akan kebingungan saat mengakses direktori terenkripsi tersebut. Untuk segi efisiensi, dikarenakan pada perusahaan tersebut sering dilaksanakan sinkronisasi antara 2 direktori, maka jasir telah merumuskan sebuah metode agar filesystem-nya mampu mengsingkronkan kedua direktori tersebut secara otomatis. Agar integritas filesystem tersebut lebih terjamin, maka setiap command yang dilakukan akan dicatat kedalam sebuah file log.
(catatan: filesystem berfungsi normal layaknya linux pada umumnya)
(catatan: mount source (root) filesystem adalah direktori /home/[user]/Documents)

Berikut adalah detail filesystem rancangan jasir:
1. Enkripsi versi 1:
   1. Jika sebuah direktori dibuat dengan awalan “encv1_”, maka direktori tersebut akan menjadi direktori terenkripsi menggunakan metode enkripsi v1.
   2. Jika sebuah direktori di-rename dengan awalan “encv1_”, maka direktori tersebut akan menjadi direktori terenkripsi menggunakan metode enkripsi v1.
   3. Apabila sebuah direktori terenkripsi di-rename menjadi tidak terenkripsi, maka isi adirektori tersebut akan terdekrip.
   4. Setiap pembuatan direktori terenkripsi baru (mkdir ataupun rename) akan tercatat ke sebuah database/log berupa file.
   5. Semua file yang berada dalam direktori ter enkripsi menggunakan caesar cipher dengan key.

![image](https://user-images.githubusercontent.com/60419316/80187995-bddfb000-863a-11ea-935b-7a81398796c0.png)

Misal kan ada file bernama “kelincilucu.jpg” dalam directory FOTO_PENTING, dan key yang dipakai adalah 10
“**encv1_rahasia/FOTO_PENTING/kelincilucu.jpg**” => “**encv1_rahasia/ULlL@u]AlZA(/g7D.|\_.Da_a.jpg**
**Note** : Dalam penamaan file ‘/’ diabaikan, dan ekstensi tidak perlu di encrypt.
   6. Metode enkripsi pada suatu direktori juga berlaku kedalam direktori lainnya yang ada didalamnya.

2. Enkripsi versi 2:
   1. Jika sebuah direktori dibuat dengan awalan “encv2_”, maka direktori tersebut akan menjadi direktori terenkripsi menggunakan metode enkripsi v2.
   2. Jika sebuah direktori di-rename dengan awalan “encv2_”, maka direktori tersebut akan menjadi direktori terenkripsi menggunakan metode enkripsi v2.
   3. Apabila sebuah direktori terenkripsi di-rename menjadi tidak terenkripsi, maka isi direktori tersebut akan terdekrip.
   4. Setiap pembuatan direktori terenkripsi baru (mkdir ataupun rename) akan tercatat ke sebuah database/log berupa file.
   5. Pada enkripsi v2, file-file pada direktori asli akan menjadi bagian-bagian kecil sebesar 1024 bytes dan menjadi normal ketika diakses melalui filesystem rancangan jasir. Sebagai contoh, file File_Contoh.txt berukuran 5 kB pada direktori asli akan menjadi 5 file kecil yakni: File_Contoh.txt.000, File_Contoh.txt.001, File_Contoh.txt.002, File_Contoh.txt.003, dan File_Contoh.txt.004.
   6. Metode enkripsi pada suatu direktori juga berlaku kedalam direktori lain yang ada didalam direktori tersebut (rekursif).

3. Sinkronisasi direktori otomatis:

Tanpa mengurangi keumuman, misalkan suatu directory bernama dir akan tersinkronisasi dengan directory yang memiliki nama yang sama dengan awalan sync_ yaitu sync_dir. Persyaratan untuk sinkronisasi yaitu:
   1. Kedua directory memiliki parent directory yang sama.
   2. Kedua directory kosong atau memiliki isi yang sama. Dua directory dapat dikatakan memiliki isi yang sama jika memenuhi:
      1. Nama dari setiap berkas di dalamnya sama.
      2. Modified time dari setiap berkas di dalamnya tidak berselisih lebih dari 0.1 detik.
   3. Sinkronisasi dilakukan ke seluruh isi dari kedua directory tersebut, tidak hanya di satu child directory saja.
   4. Sinkronisasi mencakup pembuatan berkas/directory, penghapusan berkas/directory, dan pengubahan berkas/directory.

Jika persyaratan di atas terlanggar, maka kedua directory tersebut tidak akan tersinkronisasi lagi.
Implementasi dilarang menggunakan symbolic links dan thread.

4. Log system:

   1. Sebuah berkas nantinya akan terbentuk bernama "fs.log" di direktori *home* pengguna (/home/[user]/fs.log) yang berguna menyimpan daftar perintah system call yang telah dijalankan.
   2. Agar nantinya pencatatan lebih rapi dan terstruktur, log akan dibagi menjadi beberapa level yaitu INFO dan WARNING.
   3. Untuk log level WARNING, merupakan pencatatan log untuk syscall rmdir dan unlink.
   4. Sisanya, akan dicatat dengan level INFO.
   5. Format untuk logging yaitu:

![image](https://user-images.githubusercontent.com/60419316/80188024-ccc66280-863a-11ea-9576-b5b14517398c.png)


LEVEL    : Level logging
yy   	 : Tahun dua digit
mm    	 : Bulan dua digit
dd    	 : Hari dua digit
HH    	 : Jam dua digit
MM    	 : Menit dua digit
SS    	 : Detik dua digit
CMD     	 : System call yang terpanggil
DESC      : Deskripsi tambahan (bisa lebih dari satu, dipisahkan dengan ::)

Contoh format logging nantinya seperti:

![image](https://user-images.githubusercontent.com/60419316/80188125-ed8eb800-863a-11ea-80ad-fa5c119288ed.png)


### Jawaban
```
/*
  FUSE: Filesystem in Userspace
  Copyright (C) 2001-2007  Miklos Szeredi <miklos@szeredi.hu>
  Minor modifications and note by Andy Sayler (2012) <www.andysayler.com>
  Source: fuse-2.8.7.tar.gz examples directory
  http://sourceforge.net/projects/fuse/files/fuse-2.X/
  This program can be distributed under the terms of the GNU GPL.
  See the file COPYING.
  gcc -Wall `pkg-config fuse --cflags` fusexmp.c -o fusexmp `pkg-config fuse --libs`
  Note: This implementation is largely stateless and does not maintain
        open file handels between open and release calls (fi->fh).
        Instead, files are opened and closed as necessary inside read(), write(),
        etc calls. As such, the functions that rely on maintaining file handles are
        not implmented (fgetattr(), etc). Those seeking a more efficient and
        more complete implementation may wish to add fi->fh support to minimize
        open() and close() calls and support fh dependent functions.
	https://github.com/asayler/CU-CS3753-PA5/blob/master/fusexmp.c
*/

// /home/zenados/Documents/FuseDebugging
// Command to start fuse
// ./1.exe -d testlink/ -o modules=subdir,subdir=/home/zenados/Documents/4shift/FuseDebugging
#define FUSE_USE_VERSION 28
#define HAVE_SETXATTR

#ifdef HAVE_CONFIG_H
#include <config.h>
#endif

#ifdef linux
/* For pread()/pwrite() */
#define _XOPEN_SOURCE 500
#endif

#include <fuse.h>
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include <dirent.h>
#include <errno.h>
#include <sys/time.h>
#include <sys/stat.h>
#ifdef HAVE_SETXATTR
#include <sys/xattr.h>
#endif
#include <sys/types.h>

// Switched dot '.' character with space ' ' character since . is an extension specific character
char charlist[] = "9(ku@AW1[Lmvgax6q`5Y2Ry?+sF!^HKQiBXCUSe&0M b%rI'7d)o4~VfZ*{#:}ETt$3J-zpc]lnh8,GwP_ND|jO9(ku@AW1[Lmvgax6q`5Y2Ry?+sF!^HKQiBXCUSe&0M b%rI'7d)o4~VfZ*{#:}ETt$3J-zpc]lnh8,GwP_ND|jO";

void encrypt1(const char *filepathIn, int offset) {
	char filepath[500];
  int startIdx = 0;
	char *tmpChrP;

	strcpy(filepath,filepathIn);
	printf("strstr success\n");
	if ((tmpChrP = strrchr(filepath+1,'/'))!=NULL) {
		startIdx = tmpChrP-filepath;
		printf("startIdx:%d\n",startIdx);
	} else {
	// Else, this is the /encv1_ itself, there's nothing to encrypt or decrypt
		return;
	}

	int fileExtIdx;
	if ((tmpChrP = strrchr(filepath, '.'))!=NULL) {
		fileExtIdx = tmpChrP-filepath;
	} else {
		fileExtIdx = strlen(filepath);
	}

	int tempIdx;
	printf("input:%s",filepathIn);
  while(startIdx != fileExtIdx) {
	// Iterate until file extension
		if (filepath[startIdx] != '/') {
			tempIdx = strchr(charlist,filepath[startIdx]) - charlist + offset;
			filepath[startIdx] = charlist[tempIdx];
		}
		startIdx++;
  }

	printf("output:%s",filepath);
	rename(filepathIn,filepath);
}

void decrypt1(const char *filepathIn, int offset) {
	char filepath[500];
  int startIdx = 0;
	char *tmpChrP;

	strcpy(filepath,filepathIn);
	if ((tmpChrP = strrchr(filepath+1,'/'))!=NULL) {
		startIdx = tmpChrP-filepath;
	} else {
	// Else, this is the /encv1_ itself, there's nothing to encrypt or decrypt
		return;
	}

	int fileExtIdx;
	if ((tmpChrP = strrchr(filepath, '.'))!=NULL) {
		fileExtIdx = tmpChrP-filepath;
	} else {
		fileExtIdx = strlen(filepath);
	}

	int tempIdx;
  while(startIdx != fileExtIdx) {
	// Iterate until file extension
		if (filepath[startIdx] != '/') {
			tempIdx = strrchr(charlist,filepath[startIdx]) - charlist - offset;
			filepath[startIdx] = charlist[tempIdx];
		}
		startIdx++;
  }
	
	rename(filepathIn,filepath);
}

void encryptDir1(const char *dirPath) {
	char currDir[500];
	strcpy(currDir,dirPath);

	DIR *dp;
	struct dirent *de;

	dp = opendir(currDir);
	if (dp == NULL)
		return;

	while ((de = readdir(dp)) != NULL) {
		if (strcmp(de->d_name,".")==0 || strcmp(de->d_name,"..")==0) {
			continue;
		}
		printf("\nencryptdir called\n");
		struct stat st;
		memset(&st, 0, sizeof(st));
		st.st_ino = de->d_ino;
		st.st_mode = de->d_type << 12;
		char fullpath[500];
		sprintf(fullpath,"%s/%s",currDir,de->d_name);
		printf("fullpath:%s\n",fullpath);
		if (S_ISREG(st.st_mode)) {
		// If is regular file
			encrypt1(fullpath,10);
		} else if (S_ISDIR(st.st_mode)) {
			encryptDir1(fullpath);
			encrypt1(fullpath,10);
		} else {
			continue;
		}
	}

	closedir(dp);
	return;
}

void decryptDir1(const char *dirPath) {
	char currDir[500];
	strcpy(currDir,dirPath);

	DIR *dp;
	struct dirent *de;

	dp = opendir(currDir);
	if (dp == NULL)
		return;

	while ((de = readdir(dp)) != NULL) {
		if (strcmp(de->d_name,".")==0 || strcmp(de->d_name,"..")==0) {
			continue;
		}
		struct stat st;
		memset(&st, 0, sizeof(st));
		st.st_ino = de->d_ino;
		st.st_mode = de->d_type << 12;
		char fullpath[500];
		sprintf(fullpath,"%s/%s",currDir,de->d_name);
		if (S_ISREG(st.st_mode)) {
		// If is regular file
			decrypt1(fullpath,10);
		} else if (S_ISDIR(st.st_mode)) {
			decryptDir1(fullpath);
			decrypt1(fullpath,10);
		} else {
			continue;
		}
	}

	closedir(dp);
	return;
}


#define bufSize 32
// #define bufSize 1024
void encrypt2(const char *filepath) {
	char buff[1025];
	int bytes_read;
	FILE *output;
	FILE *input;
	int fileCount = 0;
	
	// if filepath already have numbers at the end then the file is encrypted, don't re encrypt it.
	int startIdx;
	char *tmpChrP;
	if ((tmpChrP = strrchr(filepath,'.'))!=NULL) {
	// Get last dot found on file
		startIdx = tmpChrP-filepath;
		int isNum = 1;
		while(filepath[++startIdx]!='\0') {
		// If rest of string are not numbers then file is not encrypted, encrypt it.
			if(!(filepath[startIdx]>='0' && filepath[startIdx]<='9')) {
				isNum = 0;
				break;
			}
		}
		// If rest of string are numbers then file is encrypted, don't re encrypt it.
		if (isNum) {
			return;
		}
	} 

	input = fopen(filepath, "r");
	if (input == NULL) 
		printf("Error opening file '%s' !",filepath);
	while ((bytes_read = fread(buff, sizeof(char), bufSize, input)) > 0) {
		buff[bytes_read] = '\0';

		// Write to new file
		char newFilePath[500];
		sprintf(newFilePath,"%s.%03d",filepath,fileCount++);

		output = fopen(newFilePath, "w+");
		if (output == NULL) 
			printf("Error opening file '%s' !",newFilePath);
		fwrite(buff, bytes_read, 1, output);
		fclose(output);
		memset(buff, 0, bufSize);
	}
	fclose(input);
	// Remove file
	if (remove(filepath) == 0) 
		printf("Deleted successfully"); 
	else
		printf("Unable to delete the file"); 
}

// filepath is one of the partial files to concatenate
void decrypt2(const char *filenameIn) {
	char buff[1025];
	int bytes_read;
	FILE *output;
	FILE *input;
	int fileCount = 0;
	// if filepath doesn't have all numbers at the end then the file is not encrypted.
	int startIdx;
	char *tmpChrP;
	if ((tmpChrP = strrchr(filenameIn,'.'))!=NULL) {
	// Get last dot found on file
		startIdx = tmpChrP-filenameIn;
		int isNum = 1;
		while(filenameIn[++startIdx]!='\0') {
		// If rest of string are numbers then file is encrypted, decrypt it.
			if(!(filenameIn[startIdx]>='0' && filenameIn[startIdx]<='9')) {
				isNum = 0;
				break;
			}
		}
		// If rest of string are numbers then file is encrypted, don't re encrypt it.
		if (!isNum) {
			return;
		}
	} 

	int fileExtIdx = strrchr(filenameIn,'.')-filenameIn;
	char filename[500];
	sprintf(filename,"%s",filenameIn);
	filename[fileExtIdx] = '\0';
	output = fopen(filename, "a");
	if (output == NULL) 
		printf("Error opening file '%s' !",filename);

	while (1) {
		// get curfile
		char curFilename[500];
		sprintf(curFilename,"%s.%03d",filename,fileCount++);
		
		// check if file doesn't exist
		if (access(curFilename,F_OK)==-1) break;
		input = fopen(curFilename, "r");
		if (input == NULL)
			printf("Error opening file '%s' !",curFilename);
		
		bytes_read = fread(buff, sizeof(char), bufSize, input);
		fclose(input);
		printf("\n\nread %d:%s\n\n\n", bytes_read, buff);
		fwrite(buff, bytes_read, 1, output);
		memset(buff, 0, bufSize);
		// remove file
		remove(curFilename);
	}
	fclose(output);
}

void encryptDir2(const char *dirPath) {
	char currDir[500];
	strcpy(currDir,dirPath);

	DIR *dp;
	struct dirent *de;

	dp = opendir(currDir);
	if (dp == NULL)
		return;

	while ((de = readdir(dp)) != NULL) {
		if (strcmp(de->d_name,".")==0 || strcmp(de->d_name,"..")==0) {
			continue;
		}
		struct stat st;
		memset(&st, 0, sizeof(st));
		st.st_ino = de->d_ino;
		st.st_mode = de->d_type << 12;
		char fullpath[500];
		sprintf(fullpath,"%s/%s",currDir,de->d_name);
		if (S_ISREG(st.st_mode)) {
		// If is regular file
			encrypt2(fullpath);
		} else if (S_ISDIR(st.st_mode)) {
			encryptDir2(fullpath);
		} else {
			continue;
		}
	}

	closedir(dp);
	return;
}

void decryptDir2(const char *dirPath) {
	char currDir[500];
	strcpy(currDir,dirPath);

	DIR *dp;
	struct dirent *de;

	dp = opendir(currDir);
	if (dp == NULL)
		return;

	while ((de = readdir(dp)) != NULL) {
		if (strcmp(de->d_name,".")==0 || strcmp(de->d_name,"..")==0) {
			continue;
		}
		struct stat st;
		memset(&st, 0, sizeof(st));
		st.st_ino = de->d_ino;
		st.st_mode = de->d_type << 12;
		char fullpath[500];
		sprintf(fullpath,"%s/%s",currDir,de->d_name);
		if (S_ISREG(st.st_mode)) {
		// If is regular file
			decrypt2(fullpath);
		} else if (S_ISDIR(st.st_mode)) {
			decryptDir2(fullpath);
		} else {
			continue;
		}
	}

	closedir(dp);
	return;
}

// Use caps for level input
// Use empty string if arg2 does not exist
char logFilepath[] = "/home/zenados/fs.log";
void logging(char levelIn[], const char *arg1, const char *arg2) {
	char *level="INFO";
	if ( strcmp(levelIn,"RMDIR")==0 || strcmp(levelIn,"UNLINK")==0 ) {
		level = "WARNING";
	}

	time_t rawtime;
	time (&rawtime);
	char timestring[25];
	struct tm *timeis = localtime(&rawtime);
	sprintf(timestring,"%02d%02d%02d-%02d:%02d:%02d",timeis->tm_year-100,timeis->tm_mon,timeis->tm_mday,timeis->tm_hour,timeis->tm_min,timeis->tm_sec);

	char finalString[500];
	sprintf(finalString,"%s::%s::%s::%s",level,timestring,levelIn,arg1);
	if (strcmp(arg2,"")!=0){
		sprintf(finalString,"%s::%s",finalString,arg2);
	}

	FILE *logfs;

	logfs = fopen(logFilepath, "a");
	if (logfs == NULL)
		printf("Error opening log file!");
	fprintf(logfs,"%s\n",finalString);
	fclose(logfs);
}

char encLogFilepath[] = "/home/zenados/database.log";
void loggingCustom(char action[], char type[], const char *arg1, const char *arg2) {
	FILE *logfs;

	time_t rawtime;
	time (&rawtime);
	char timestring[25];
	struct tm *timeis = localtime(&rawtime);
	sprintf(timestring,"%02d%02d%02d-%02d:%02d:%02d",timeis->tm_year-100,timeis->tm_mon,timeis->tm_mday,timeis->tm_hour,timeis->tm_min,timeis->tm_sec);

	logfs = fopen(encLogFilepath, "a");
	if (logfs == NULL)
		printf("Error opening log file!");
	
	if (strcmp(action,"CREATED")==0) {
		fprintf(logfs,"%s|%s %s:\n\tfolder: %s\n",timestring,action,type,arg1);
	} else {
		if (strcmp(type,"") == 0) {
			fprintf(logfs,"%s|%s:\n\tfrom: %s\n\tto: %s\n",timestring,action,arg1,arg2);
		} else {
			fprintf(logfs,"%s|%s %s:\n\tfrom: %s\n\tto: %s\n",timestring,action,type,arg1,arg2);
		}
	}
	fclose(logfs);
}

char *syncFolderCheck(const char *folderPath) {
	size_t size=500; char *buff = malloc(size*sizeof(char));
	if (getxattr(folderPath,"user.xsync_",buff,size)!=-1) {
		return buff;
	} else {
		return NULL;
	}
}

void syncFolderSet(const char *from, const char *to) {
	char dirPath[500]; //path of directory without the sync
	char syncPath[500]; //path of directory with the sync
	// Get syncPath
	strcpy(syncPath,to);
	// Get dirPath
	strcpy(dirPath,to);
	char *tmpChrP1;
	char *tmpChrP2;
	tmpChrP1 = strstr(dirPath,"/sync_")+1;
	tmpChrP2 = (strchr(tmpChrP1,'_')+1);
	strcpy(tmpChrP1,tmpChrP2); //replace string from foo/sync_bar to foo/bar
	loggingCustom("SYNCED","",from,to);

	size_t size=500; char buff[size];
	strcpy(buff,dirPath);
	if (setxattr(from,"user.xsync_",buff,(strlen(buff)+1)*sizeof(char),XATTR_CREATE)==-1) {
		perror("There was an error setting the sync attribute\n");
	}
	strcpy(buff,syncPath);
	if (setxattr(dirPath,"user.xsync_",buff,(strlen(buff)+1)*sizeof(char),XATTR_CREATE)==-1) {
		perror("There was an error setting the sync attribute\n");
	}
}

void syncFolderUnset(const char *from) {
	char dirPath[500]; //path of directory without the sync
	// Get dirPath
	strcpy(dirPath,from);
	char *tmpChrP1;
	char *tmpChrP2;
	tmpChrP1 = strstr(dirPath,"/sync_")+1;
	tmpChrP2 = (strchr(tmpChrP1,'_')+1);
	strcpy(tmpChrP1,tmpChrP2); //replace string from foo/sync_bar to foo/bar

	loggingCustom("UNSYNCED","",from,dirPath);
	removexattr(from,"user.xsync_");
	removexattr(dirPath,"user.xsync_");
}

int syncFolderDirReq(char path1[], char path2[]) {

	DIR *dp;
	struct dirent *de;

	dp = opendir(path1);
	if (dp == NULL)
		return 0;

	while ((de = readdir(dp)) != NULL) {
		if (strcmp(de->d_name,".")==0 || strcmp(de->d_name,"..")==0) {
			continue;
		}

		char fullpath1[500],fullpath2[500];
		sprintf(fullpath1,"%s/%s",path1,de->d_name);
		sprintf(fullpath2,"%s/%s",path2,de->d_name);
		struct stat stmp1,stmp2;
		stat(fullpath1, &stmp1);
		stat(fullpath2, &stmp2);
		unsigned long long int ms1 = stmp1.st_mtime*1e3 + stmp1.st_mtimensec/1e6;
		unsigned long long int ms2 = stmp2.st_mtime*1e3 + stmp2.st_mtimensec/1e6;
		// if requirements not met
		if (access(fullpath2,F_OK)!=-1 && abs((int)ms1-ms2) > 100) {
			return 0;
		}

		if (S_ISDIR(stmp1.st_mode)) {
			if (syncFolderDirReq(fullpath1,fullpath2)) return 0;
		} else {
			continue;
		}

	}

	closedir(dp);
	return 1;
}

int syncFolderReq(const char *from, const char *to) {
	char parentDirectory[500];
	char dirPath[500]; //path of directory without the sync
	char syncPath[500]; //path of directory with the sync
	// Get parent dir path
	strcpy(parentDirectory,to);
	int tmpI = strrchr(parentDirectory,'/')-parentDirectory;
	parentDirectory[tmpI] = '\0';
	// Get syncPath
	strcpy(syncPath,to);
	// Get dirPath
	strcpy(dirPath,to);
	char *tmpChrP1;
	char *tmpChrP2;
	tmpChrP1 = strstr(dirPath,"/sync_")+1;
	tmpChrP2 = (strchr(tmpChrP1,'_')+1);
	strcpy(tmpChrP1,tmpChrP2); //replace string from foo/sync_bar to foo/bar
	// If parent dir is not similar
	if (access(dirPath,F_OK)==-1) return 0;

	// If path is already synced
	size_t size=500; char buff[size];
	if (getxattr(from,"user.xsync_",buff,size)!=-1) return 0;
	if (getxattr(dirPath,"user.xsync_",buff,size)!=-1) return 0;
	
	// If contents is not similar or last modified time is not > 0.1 s
	if (syncFolderDirReq(dirPath,syncPath)) return 0;
	if (syncFolderDirReq(syncPath,dirPath)) return 0;

	return 1;
}

static int xmp_rename(const char *from, const char *to) {
	logging("RENAME",from,to);
	// Find last occurence of 
	char *tmpChrPFrom = strrchr(from,'/');
	char *tmpChrPTo = strrchr(to,'/');
	if (strstr(tmpChrPFrom,"/encv1_")==NULL && strstr(tmpChrPTo,"/encv1_")!=NULL) {
	// If directory is now encrypted
		loggingCustom("ENCRYPTED","Type 1",from,to);
		encryptDir1(from);
	}
	if (strstr(tmpChrPFrom,"/encv1_")!=NULL && strstr(tmpChrPTo,"/encv1_")==NULL) {
	// If directory is now decrypted
		loggingCustom("DECRYPTED","Type 1",from,to);
		decryptDir1(from);
	}
	if (strstr(tmpChrPFrom,"/encv2_")==NULL && strstr(tmpChrPTo,"/encv2_")!=NULL) {
	// If directory is now encrypted
		loggingCustom("ENCRYPTED","Type 2",from,to);
		encryptDir2(from);
	}
	if (strstr(tmpChrPFrom,"/encv2_")!=NULL && strstr(tmpChrPTo,"/encv2_")==NULL) {
	// If directory is now decrypted
		loggingCustom("DECRYPTED","Type 2",from,to);
		decryptDir2(from);
	}
	if (strstr(tmpChrPFrom,"/sync_")==NULL && strstr(tmpChrPTo,"/sync_")!=NULL) {
	// If directory is now a sync directory
		if (syncFolderReq(from,to)) {
			syncFolderSet(from,to);
		} else {
			// Cancel rename
			return -1;
		}
	}
	if (strstr(tmpChrPFrom,"/sync_")!=NULL && strstr(tmpChrPTo,"/sync_")==NULL) {
	// If directory is now not a sync directory
		syncFolderUnset(from);
	}


	int res;
	res = rename(from, to);
	if (res == -1)
		return -errno;

	return 0;
}

static int xmp_mkdir(const char *path, mode_t mode) {
	logging("MKDIR",path,"");
	char *tmpChrP = strrchr(path,'/');
	if (strstr(tmpChrP,"/encv1_")!=NULL) {
	// If new directory is encrypted
		loggingCustom("CREATED","Type 1",path,"");
	}
	if (strstr(tmpChrP,"/encv2_")!=NULL) {
	// If new directory is encrypted
		loggingCustom("CREATED","Type 2",path,"");
	}

	int res;
	res = mkdir(path, mode);
	if (res == -1)
		return -errno;

	return 0;
}

static int xmp_write(const char *path, const char *buf, size_t size, off_t offset, struct fuse_file_info *fi)
{
	logging("WRITE",path,"");

	int fd;
	int res;

	(void) fi;
	fd = open(path, O_WRONLY);
	if (fd == -1)
		return -errno;

	res = pwrite(fd, buf, size, offset);
	if (res == -1)
		res = -errno;

	close(fd);
	if (strstr(path,"/encv2_")!=NULL) {
		encrypt2(path);
	}
	return res;
}

static int xmp_create(const char* path, mode_t mode, struct fuse_file_info* fi) {
	logging("CREATE",path,"");

	(void) fi;

	int res;
	res = creat(path, mode);
	if(res == -1)
		return -errno;

	close(res);
	
	if (strstr(path,"/encv1_")!=NULL) {
		encrypt1(path,10);
	}

	return 0;
}

static int xmp_getattr(const char *path, struct stat *stbuf)
{
	// logging("GETATTR",path,"");
	int res;
	res = lstat(path, stbuf);
	if (res == -1)
		return -errno;

	return 0;
}

static int xmp_readdir(const char *path, void *buf, fuse_fill_dir_t filler, off_t offset, struct fuse_file_info *fi)
{
	// logging("READDIR",path,"");

	DIR *dp;
	struct dirent *de;

	(void) offset;
	(void) fi;

	dp = opendir(path);
	if (dp == NULL)
		return -errno;

	while ((de = readdir(dp)) != NULL) {
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

static int xmp_access(const char *path, int mask)
{
	// logging("ACCESS",path,"");

	int res;
	res = access(path, mask);
	if (res == -1)
		return -errno;

	return 0;
}

static int xmp_unlink(const char *path)
{
	logging("UNLINK",path,"");

	int res;
	res = unlink(path);
	if (res == -1)
		return -errno;

	return 0;
}

static int xmp_readlink(const char *path, char *buf, size_t size)
{
	logging("READLINK",path,"");

	int res;
	res = readlink(path, buf, size - 1);
	if (res == -1)
		return -errno;

	buf[res] = '\0';
	return 0;
}

static int xmp_mknod(const char *path, mode_t mode, dev_t rdev)
{
	logging("MKNOD",path,"");

	int res;
	/* On Linux this could just be 'mknod(path, mode, rdev)' but this
	   is more portable */
	if (S_ISREG(mode)) {
		res = open(path, O_CREAT | O_EXCL | O_WRONLY, mode);
		if (res >= 0)
			res = close(res);
	} else if (S_ISFIFO(mode))
		res = mkfifo(path, mode);
	else
		res = mknod(path, mode, rdev);
	if (res == -1)
		return -errno;

	return 0;
}

static int xmp_rmdir(const char *path)
{
	logging("RMDIR",path,"");

	int res;
	res = rmdir(path);
	if (res == -1)
		return -errno;

	return 0;
}

static int xmp_symlink(const char *from, const char *to)
{
	logging("SYMLINK",from,to);

	int res;
	res = symlink(from, to);
	if (res == -1)
		return -errno;

	return 0;
}

static int xmp_link(const char *from, const char *to)
{
	logging("LINK",from,to);

	int res;
	res = link(from, to);
	if (res == -1)
		return -errno;

	return 0;
}

static int xmp_chmod(const char *path, mode_t mode)
{
	logging("CHMOD",path,"");

	int res;
	res = chmod(path, mode);
	if (res == -1)
		return -errno;

	return 0;
}

static int xmp_chown(const char *path, uid_t uid, gid_t gid)
{
	logging("CHOWN",path,"");

	int res;
	res = lchown(path, uid, gid);
	if (res == -1)
		return -errno;

	return 0;
}

static int xmp_truncate(const char *path, off_t size)
{
	logging("TRUNCATE",path,"");

	int res;
	res = truncate(path, size);
	if (res == -1)
		return -errno;

	return 0;
}

static int xmp_utimens(const char *path, const struct timespec ts[2])
{
	logging("UTIMENS",path,"");

	int res;
	struct timeval tv[2];

	tv[0].tv_sec = ts[0].tv_sec;
	tv[0].tv_usec = ts[0].tv_nsec / 1000;
	tv[1].tv_sec = ts[1].tv_sec;
	tv[1].tv_usec = ts[1].tv_nsec / 1000;

	res = utimes(path, tv);
	if (res == -1)
		return -errno;

	return 0;
}

static int xmp_open(const char *path, struct fuse_file_info *fi)
{
	logging("OPEN",path,"");

	int res;
	res = open(path, fi->flags);
	if (res == -1)
		return -errno;

	close(res);
	return 0;
}

static int xmp_read(const char *path, char *buf, size_t size, off_t offset, struct fuse_file_info *fi)
{
	logging("READ",path,"");

	int fd;
	int res;

	(void) fi;
	
	fd = open(path, O_RDONLY);
	if (fd == -1)
		return -errno;

	res = pread(fd, buf, size, offset);
	if (res == -1)
		res = -errno;

	close(fd);
	return res;
}

static int xmp_statfs(const char *path, struct statvfs *stbuf)
{
	logging("STATFS",path,"");

	int res;
	res = statvfs(path, stbuf);
	if (res == -1)
		return -errno;

	return 0;
}

static int xmp_release(const char *path, struct fuse_file_info *fi)
{
	/* Just a stub.	 This method is optional and can safely be left
	   unimplemented */
	logging("RELEASE",path,"");

	(void) path;
	(void) fi;
	return 0;
}

static int xmp_fsync(const char *path, int isdatasync, struct fuse_file_info *fi)
{
	/* Just a stub.	 This method is optional and can safely be left
	   unimplemented */
	logging("FSYNC",path,"");

	(void) path;
	(void) isdatasync;
	(void) fi;
	return 0;
}

#ifdef HAVE_SETXATTR
static int xmp_setxattr(const char *path, const char *name, const char *value, size_t size, int flags)
{
	logging("SETXATTR",path,"");

	int res = lsetxattr(path, name, value, size, flags);
	if (res == -1)
		return -errno;
	return 0;
}

static int xmp_getxattr(const char *path, const char *name, char *value, size_t size)
{
	logging("GETXATTR",path,"");

	int res = lgetxattr(path, name, value, size);
	if (res == -1)
		return -errno;
	return res;
}

static int xmp_listxattr(const char *path, char *list, size_t size)
{
	logging("LISTXATTR",path,"");

	int res = llistxattr(path, list, size);
	if (res == -1)
		return -errno;
	return res;
}

static int xmp_removexattr(const char *path, const char *name)
{
	logging("REMOVEXATTR",path,"");

	int res = lremovexattr(path, name);
	if (res == -1)
		return -errno;
	return 0;
}
#endif /* HAVE_SETXATTR */

static struct fuse_operations xmp_oper = {
	.getattr	= xmp_getattr,
	.access		= xmp_access,
	.readlink	= xmp_readlink,
	.readdir	= xmp_readdir,
	.mknod		= xmp_mknod,
	.mkdir		= xmp_mkdir,
	.symlink	= xmp_symlink,
	.unlink		= xmp_unlink,
	.rmdir		= xmp_rmdir,
	.rename		= xmp_rename,
	.link			= xmp_link,
	.chmod		= xmp_chmod,
	.chown		= xmp_chown,
	.truncate	= xmp_truncate,
	.utimens	= xmp_utimens,
	.open			= xmp_open,
	.read			= xmp_read,
	.write		= xmp_write,
	.statfs		= xmp_statfs,
	.create 	= xmp_create,
	.release	= xmp_release,
	.fsync		= xmp_fsync,
#ifdef HAVE_SETXATTR
	.setxattr	= xmp_setxattr,
	.getxattr	= xmp_getxattr,
	.listxattr	= xmp_listxattr,
	.removexattr	= xmp_removexattr,
#endif
};

int main(int argc, char *argv[])
{
	umask(0);
	return fuse_main(argc, argv, &xmp_oper, NULL);
}
```

### Bukti
#### Saat 

#### Saat

#### Saat

