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
/* 
./compile.sh soal1.c 1.exe
./1.exe -d testlink/ -o modules=subdir,subdir=/home/zenados/Documents/4shift/FuseDebugging
./compile.sh soal1.c 1.exe && ./1.exe -d testlink/ -o modules=subdir,subdir=/home/zenados/Documents/4shift/FuseDebugging
cd /home/zenados/Documents/4shift/testlink/testDir
rm -r dirindir2/
rm -r dirindir3/
rm -r tmp/
rm -r sync_tmp/
cp -r dirindir/ dirindir2/
cp -r dirindir/ dirindir3/
mv dirindir2/ tmp/ && mv dirindir3/ sync_tmp/
 */
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


// #define bufSize 32
#define bufSize 1024
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

char *syncFolderGetSyncedPath(const char *folderPathIn) {
	printf("\n\nsyncFolderGetSyncedPath Called\n");
	char folderPath[500];
	strcpy(folderPath,folderPathIn);
	char parentPath[500];
	strcpy(parentPath,folderPathIn);

	size_t size=500; char *buff = malloc(size*sizeof(char));
	int found = 0;
	while (strcmp(parentPath,"")!=0) {
		printf("currDir:%s\n",parentPath);
		if (getxattr(parentPath,"user.xsync_",buff,size)!=-1) {
			found = 1;
			break;
		}
		*(strrchr(parentPath,'/')) = '\0'; //
	}

	if (found) {
		char *subPath = folderPath+strlen(parentPath);
		char *endP;
		endP = buff+strlen(buff);
		strcpy(endP,subPath);
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
	loggingCustom("SYNCED","",to,dirPath);

	size_t size=500; char buff[size];
	strcpy(buff,dirPath);
	printf("\nSetting x Attribute: %s\n",from);
	if (setxattr(from,"user.xsync_",buff,(strlen(buff)+1)*sizeof(char),XATTR_CREATE)==-1) {
		perror("\n\nThere was an error setting the sync attribute\n\n");
	}
	printf("\nSetting x Attribute: %s\n",from);
	strcpy(buff,syncPath);
	if (setxattr(dirPath,"user.xsync_",buff,(strlen(buff)+1)*sizeof(char),XATTR_CREATE)==-1) {
		perror("\n\nThere was an error setting the sync attribute\n\n");
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

int syncFolderDirReq(const char *path1, const char *path2) {
	DIR *dp;
	struct dirent *de;

	dp = opendir(path1);
	if (dp == NULL)
		return 0;
	printf("checking dir:%s\n",path1);
	while ((de = readdir(dp)) != NULL) {
		if (strcmp(de->d_name,".")==0 || strcmp(de->d_name,"..")==0) {
			continue;
		}

		char fullpath1[500],fullpath2[500];
		sprintf(fullpath1,"%s/%s",path1,de->d_name);
		sprintf(fullpath2,"%s/%s",path2,de->d_name);
		printf("\tcurPath checked:\n\t\t%s\n\t\t%s\n",fullpath1,fullpath2);
		struct stat stmp1,stmp2;
		stat(fullpath1, &stmp1);
		stat(fullpath2, &stmp2);
		printf("\tTime1:\t%ld | %ld\n",stmp1.st_mtime,stmp1.st_mtimensec);
		unsigned long long int ms1 = stmp1.st_mtime*1e3 + stmp1.st_mtimensec/1e6;
		printf("\tTime2:\t%ld | %ld\n",stmp2.st_mtime,stmp2.st_mtimensec);
		unsigned long long int ms2 = stmp2.st_mtime*1e3 + stmp2.st_mtimensec/1e6;
		// if requirements not met
		printf("\tTime:\t%lld | %lld\n",ms1,ms2);
		printf("\nAccess:%d\n",access(fullpath2,F_OK));
		printf("TimeDiff:%d\n",abs((int)(ms1-ms2)));
		if (access(fullpath2,F_OK)==-1 || abs((int)(ms1-ms2)) > 100) {
			return 0;
		}

		if (S_ISDIR(stmp1.st_mode)) {
			if (!syncFolderDirReq(fullpath1,fullpath2)) return 0;
		} else {
			continue;
		}

	}

	closedir(dp);
	printf("Closing dir returning true");
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
	printf("From:%s\nAccessing:%s\n",from,dirPath);
	if (access(dirPath,F_OK)==-1) return 0;

	// If path is already synced
	size_t size=500; char buff[size];
	printf("getting x attr from:%s\n",from);
	if (getxattr(from,"user.xsync_",buff,size)!=-1) return 0;
	printf("getting x attr from:%s\n",dirPath);
	if (getxattr(dirPath,"user.xsync_",buff,size)!=-1) return 0;
	
	// If contents is not similar or last modified time is not > 0.1 s
	printf("Checking syncFolderDirReq %s | %s\n",dirPath,from);
	if (!syncFolderDirReq(dirPath,from)) return 0;
	printf("Done checking\n");
	printf("Checking syncFolderDirReq %s | %s\n",dirPath,from);
	if (!syncFolderDirReq(from,dirPath)) return 0;

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
		printf("\n\nRenaming directory to a sync directory\n");
		if (syncFolderReq(from,to)) {
			printf("Directory eligible to ba a sync directory\n");
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
	char *syncedPathFrom = syncFolderGetSyncedPath(from);
	char *syncedPathTo = syncFolderGetSyncedPath(to);
	if (syncedPathFrom!=NULL || syncedPathTo!=NULL) {
		printf("\n\nrename sync path:%s\n%s\n",syncedPathFrom,syncedPathTo);
		if (syncedPathFrom==NULL || syncedPathTo==NULL) {
			// Cannot move between synced and unsyced folder
			return -1;
		} else {
			printf("Synced path is being renamed\n");
			int res;
			res = rename(syncedPathFrom,syncedPathTo);
			if (res == -1)
				return -errno;
			printf("Synced path is renamed\n\n\n");
		}
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
	char *syncedPathFrom;
	printf("\n\nSyncpath check\n");
	if ((syncedPathFrom=syncFolderGetSyncedPath(path))!=NULL) {
		printf("Syncpath not null\n");
		int res;
		res = mkdir(syncedPathFrom, mode);
		if (res == -1)
			return -errno;
	}

	int res;
	res = mkdir(path, mode);
	if (res == -1)
		return -errno;

	return 0;
}

static int xmp_write(const char *path, const char *buf, size_t size, off_t offset, struct fuse_file_info *fi) {
	logging("WRITE",path,"");

	char *syncedPathFrom;
	if ((syncedPathFrom=syncFolderGetSyncedPath(path))!=NULL) {
		int fd;
		int res;

		(void) fi;
		fd = open(syncedPathFrom, O_WRONLY);
		if (fd == -1)
			return -errno;

		res = pwrite(fd, buf, size, offset);
		if (res == -1)
			res = -errno;

		close(fd);
		if (strstr(syncedPathFrom,"/encv2_")!=NULL) {
			encrypt2(syncedPathFrom);
		}

	}

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

	char *syncedPathFrom;
	if ((syncedPathFrom=syncFolderGetSyncedPath(path))!=NULL) {
		(void) fi;

		int res;
		res = creat(syncedPathFrom, mode);
		if(res == -1)
			return -errno;

		close(res);
		
		if (strstr(syncedPathFrom,"/encv1_")!=NULL) {
			encrypt1(syncedPathFrom,10);
		}
	}

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

## ENCV1
### Diberikan sebuah directory
![image](https://user-images.githubusercontent.com/60419316/80701261-511c5800-8b09-11ea-8696-3a282db67343.png)
### Rename menjadi encv1_
![image](https://user-images.githubusercontent.com/60419316/80701307-65605500-8b09-11ea-893a-d45ec1c10023.png)
</br> Jika dilihat di storage yang utama (yang bukan di mount)
![image](https://user-images.githubusercontent.com/60419316/80701385-82952380-8b09-11ea-8ef0-c3ab584edfc6.png)
### Jika kita rename kembali dari encv1
![image](https://user-images.githubusercontent.com/60419316/80701450-9a6ca780-8b09-11ea-9716-4a95cea07029.png)
</br> Jika kita lihat di main storage (yang bukan di mount)
![image](https://user-images.githubusercontent.com/60419316/80701517-b2442b80-8b09-11ea-9a0f-5258f11cb807.png)
### Logging
![image](https://user-images.githubusercontent.com/60419316/80701557-c25c0b00-8b09-11ea-903b-7edb03b0f9d8.png)
## ENCV2
### Rename 2: diberikan subdir
![image](https://user-images.githubusercontent.com/60419316/80701594-d56edb00-8b09-11ea-92e6-9a56ef994327.png)
![image](https://user-images.githubusercontent.com/60419316/80701606-dc95e900-8b09-11ea-870e-fc259999d211.png)
### Rename jadi encv2_
![image](https://user-images.githubusercontent.com/60419316/80701638-e7507e00-8b09-11ea-83a1-a7833e79a950.png)
![image](https://user-images.githubusercontent.com/60419316/80701655-efa8b900-8b09-11ea-8354-831ba988f9c1.png)
### Rename kembali dari encv2_
![image](https://user-images.githubusercontent.com/60419316/80701673-fa634e00-8b09-11ea-87a4-d24f102b8ce0.png)
![image](https://user-images.githubusercontent.com/60419316/80701697-0222f280-8b0a-11ea-8996-dcc149fc8120.png)
</br> Maka file kembali lagi tergabung
### Berlaku juga untuk subdir
![image](https://user-images.githubusercontent.com/60419316/80702136-c89eb700-8b0a-11ea-85c3-ce8fd8ddca58.png)
![image](https://user-images.githubusercontent.com/60419316/80702170-d3594c00-8b0a-11ea-81e1-8e9c998ec92b.png)
![image](https://user-images.githubusercontent.com/60419316/80702179-dbb18700-8b0a-11ea-9cb9-3c515152a903.png)
![image](https://user-images.githubusercontent.com/60419316/80702197-e23ffe80-8b0a-11ea-9b36-155bf116d29d.png)
![image](https://user-images.githubusercontent.com/60419316/80702218-e9670c80-8b0a-11ea-84ac-cfb64db53781.png)
![image](https://user-images.githubusercontent.com/60419316/80702237-f126b100-8b0a-11ea-9a7b-af5b222e97e6.png)
![image](https://user-images.githubusercontent.com/60419316/80702248-f71c9200-8b0a-11ea-8027-a29d101820ca.png)
### Encryption logging di database.log
![image](https://user-images.githubusercontent.com/60419316/80702276-04398100-8b0b-11ea-8dc5-02a5ca99bd4d.png)
## Sync DIR
### Creating dir
![image](https://user-images.githubusercontent.com/60419316/80702325-161b2400-8b0b-11ea-8293-dbf91fd02a9b.png)
</br> Hasilnya
</br> ![image](https://user-images.githubusercontent.com/60419316/80702374-292df400-8b0b-11ea-8f18-9128cc507459.png)
![image](https://user-images.githubusercontent.com/60419316/80702409-35b24c80-8b0b-11ea-8648-4b37e11e9066.png)
### Coba create dir
![image](https://user-images.githubusercontent.com/60419316/80702442-4662c280-8b0b-11ea-9106-21b591d8dd56.png)
![image](https://user-images.githubusercontent.com/60419316/80702464-4fec2a80-8b0b-11ea-84c9-2d0714553ec4.png)
### Coba create file
![image](https://user-images.githubusercontent.com/60419316/80702497-61cdcd80-8b0b-11ea-9f8e-8b303993a7ff.png)
![image](https://user-images.githubusercontent.com/60419316/80702526-6befcc00-8b0b-11ea-8fa7-150d75bafb84.png)
### Coba delete file sama dir
![image](https://user-images.githubusercontent.com/60419316/80702572-7e6a0580-8b0b-11ea-85fa-572084a7fcd1.png)
![image](https://user-images.githubusercontent.com/60419316/80702593-8629aa00-8b0b-11ea-84b9-a7addd8956e1.png)
## LOGGING
Hasil logging dari membuat contoh diatas
![image](https://user-images.githubusercontent.com/60419316/80702632-993c7a00-8b0b-11ea-815f-19bd6c2648bf.png)
