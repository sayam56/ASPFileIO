#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
//byteswrite.c

main()
{

//overwrite default umask and open sample.txt in create mode
umask(0000);
int fd=open("sample.txt",O_CREAT,0777);

close(fd);

//open the file again in readwrite mode and write 40 uppercase T
int fd2=open("sample.txt",O_RDWR);
long int n;

char buff1[40];
for(int i=0;i<40;i++)
{
    buff1[i]='T';
}

n=write(fd2,buff1,40);


//writing COMP 8567 10 positions from the beginning
int long n2=lseek(fd2,10,SEEK_SET);

char buff2[9]="COMP 8567";

n2=write(fd2,buff2,9);

//writing ASP 3 positions after the previous write
int long n3=lseek(fd2,3,SEEK_CUR);

char buff3[3]="ASP";

n3=write(fd2,buff3,3);

//writing University of Windsor 15 positions after the end of file
int long n4=lseek(fd2,15,SEEK_END);

char buff4[21]="University of Windsor";

n4=write(fd2,buff4,21);

close(fd2);


int fd3=open("sample.txt",O_RDWR);
char buffRead[50];
int long readByte=read(fd3,buffRead,50);
//printf("\n\nThe number of bytes read were %ld\n",readByte);
char readAr[readByte];

for(int i=0;i<50;i++)
{
    readAr[i] = buffRead[i];
}

close(fd3);

//opening sample.txt in write only mode
int fd4=open("sample.txt",O_WRONLY);
int count=0;

for (int i=0; i<readByte; i++){
    if(readAr[i] == '\0')
    {
        readAr[i] = "#";
    }
}

write(fd4,buffRead,50);

close(fd4);
}

