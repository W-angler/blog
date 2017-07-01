��������ôһ��Ц����`Jeff Dean`��һ����һ��printfʵ����һ��web����������������ʦ�������ǧ��ע�͵���Ȼ�޷���ȫ��������乤��ԭ�������������ǽ���Google��ҳ��ǰ�ˡ�

�����������ϣ������ʵ�ǿ��еġ�

��������ԭ�ģ�[Implementing a web server in a single printf() call](http://tinyhack.com/2014/03/12/implementing-a-web-server-in-a-single-printf-call/)

****

#### ����

****

�������˸ղŸ���ת����һ������`Jeff Dean`��Ц��������ν�ġ�����Jeff Dean����ʵ�������ӣ�[֪��](https://www.zhihu.com/question/20034686/answer/20646787)��[quora](https://www.quora.com/Jeff-Dean/What-are-all-the-Jeff-Dean-facts)����ÿ���ҿ����ʱ�������������Ǻ�ͻ����

> Jeff Dean��һ����һ��printfʵ����һ��web����������������ʦ�������ǧ��ע�͵���Ȼ�޷���ȫ��������乤��ԭ�������������ǽ���Google��ҳ��ǰ�ˡ�

������ʵ�ϣ�ȷʵ����ͨ��һ��`printf`�����ʵ��һ��web�����������ҷ���û�������������������Σ��Ҿ����Լ�ʵ��һ�¡��������ʵ�ֵĴ��룬ֻ�е�����һ��`printf`���ã�û�ж���ı������ߺ궨�壨���ģ��һ����������ô�����ģ���

`web1.c`

    #include <stdio.h>
	int main(int argc, char *argv[]){
		printf("%*c%hn%*c%hn" 
		"\xeb\x3d\x48\x54\x54\x50\x2f\x31\x2e\x30\x20\x32" 
		"\x30\x30\x0d\x0a\x43\x6f\x6e\x74\x65\x6e\x74\x2d" 
		"\x74\x79\x70\x65\x3a\x74\x65\x78\x74\x2f\x68\x74" 
		"\x6d\x6c\x0d\x0a\x0d\x0a\x3c\x68\x31\x3e\x48\x65" 
		"\x6c\x6c\x6f\x20\x57\x6f\x72\x6c\x64\x21\x3c\x2f" 
		"\x68\x31\x3e\x4c\x8d\x2d\xbc\xff\xff\xff\x48\x89" 
		"\xe3\x48\x83\xeb\x10\x48\x31\xc0\x50\x66\xb8\x1f" 
		"\x90\xc1\xe0\x10\xb0\x02\x50\x31\xd2\x31\xf6\xff" 
		"\xc6\x89\xf7\xff\xc7\x31\xc0\xb0\x29\x0f\x05\x49" 
		"\x89\xc2\x31\xd2\xb2\x10\x48\x89\xde\x89\xc7\x31" 
		"\xc0\xb0\x31\x0f\x05\x31\xc0\xb0\x05\x89\xc6\x4c" 
		"\x89\xd0\x89\xc7\x31\xc0\xb0\x32\x0f\x05\x31\xd2" 
		"\x31\xf6\x4c\x89\xd0\x89\xc7\x31\xc0\xb0\x2b\x0f" 
		"\x05\x49\x89\xc4\x48\x31\xd2\xb2\x3d\x4c\x89\xee" 
		"\x4c\x89\xe7\x31\xc0\xff\xc0\x0f\x05\x31\xf6\xff" 
		"\xc6\xff\xc6\x4c\x89\xe7\x31\xc0\xb0\x30\x0f\x05" 
		"\x4c\x89\xe7\x31\xc0\xb0\x03\x0f\x05\xeb\xc3",
		((((unsigned long int)0x4005c8+12)>>16)&0xffff),
		0,0x00000000006007D8+2,
		(((unsigned long int)0x4005c8+12)&0xffff)-((((unsigned long int)0x4005c8+12)>>16)&0xffff),
		0, 0x00000000006007D8 );
	}

������δ���ֻ����64λ��linuxϵͳ�����У����ܻ���gcc�İ汾�йأ��������������

    gcc -g web1.c -o webserver

�������ǿ����Ѿ��µ��ˣ���ʹ����һ������ĸ�ʽ���ַ�����ƭ�����ǡ���δ�����ܲ���������Ļ��������У���Ϊ��Ӳ�����������ڴ��ַ��

������������汾���ܸ�����һЩ��Ҳ�����޸�һЩ������������Ȼ��Ҫ�ı�����ֵ�������һ���ͣ���`FUNCTION_ADDR`��`DESTADDR`��

    #include<stdio.h>
	#include<stdlib.h>
	#include<stdint.h>
	#define FUNCTION_ADDR ((uint64_t)0x4005c8 + 12)
	#define DESTADDR 0x00000000006007D8
	#define a (FUNCTION_ADDR & 0xffff)
	#define b ((FUNCTION_ADDR >> 16) & 0xffff)
	int main(int argc, char *argv[]){
		printf("%*c%hn%*c%hn" 
		"\xeb\x3d\x48\x54\x54\x50\x2f\x31\x2e\x30\x20\x32" 
		"\x30\x30\x0d\x0a\x43\x6f\x6e\x74\x65\x6e\x74\x2d" 
		"\x74\x79\x70\x65\x3a\x74\x65\x78\x74\x2f\x68\x74" 
		"\x6d\x6c\x0d\x0a\x0d\x0a\x3c\x68\x31\x3e\x48\x65" 
		"\x6c\x6c\x6f\x20\x57\x6f\x72\x6c\x64\x21\x3c\x2f" 
		"\x68\x31\x3e\x4c\x8d\x2d\xbc\xff\xff\xff\x48\x89" 
		"\xe3\x48\x83\xeb\x10\x48\x31\xc0\x50\x66\xb8\x1f" 
		"\x90\xc1\xe0\x10\xb0\x02\x50\x31\xd2\x31\xf6\xff" 
		"\xc6\x89\xf7\xff\xc7\x31\xc0\xb0\x29\x0f\x05\x49" 
		"\x89\xc2\x31\xd2\xb2\x10\x48\x89\xde\x89\xc7\x31" 
		"\xc0\xb0\x31\x0f\x05\x31\xc0\xb0\x05\x89\xc6\x4c" 
		"\x89\xd0\x89\xc7\x31\xc0\xb0\x32\x0f\x05\x31\xd2" 
		"\x31\xf6\x4c\x89\xd0\x89\xc7\x31\xc0\xb0\x2b\x0f" 
		"\x05\x49\x89\xc4\x48\x31\xd2\xb2\x3d\x4c\x89\xee" 
		"\x4c\x89\xe7\x31\xc0\xff\xc0\x0f\x05\x31\xf6\xff" 
		"\xc6\xff\xc6\x4c\x89\xe7\x31\xc0\xb0\x30\x0f\x05" 
		"\x4c\x89\xe7\x31\xc0\xb0\x03\x0f\x05\xeb\xc3",
		b, 0, DESTADDR + 2, a-b, 0, DESTADDR );
	}

�����һ�ͨ��������Щ��̵�C�����������������������ġ�

������һ����������β�ͨ����ʽ�ĺ���������������һ�δ��룿������һ�����ӣ�

`run-finalizer.c`

    #include<stdlib.h>
	#include<stdio.h>
	#define ADDR 0x00000000600720
	void hello(){
		printf("hello world\n");
	}
	int main(int argc, char *argv[]){
		(*((unsigned long int*)ADDR))=(unsigned long int)hello;
	}

��������Ա����������������ܲ����������ϵͳ�����У�����ֶδ��󣩣�����Ҫ�������µĲ�������

****

**ע��**

> Ubuntu�����һ�������յ�[ELF](http://baike.baidu.com/subview/1090277/10973487.htm#viewPageContent)���ṩֻ�����ض�λ��İ�ȫ���ơ�Ϊ���ܹ���ubuntu��������Щ���ӣ��ڱ���ʱ��Ҫ���������Щ������

> `-Wl,-z,norelro`

> ���磺

> `gcc -Wl,-z,norelro test.c`

****

����1��������δ��룺

    gcc run-finalizer.c -o run-finalizer

����2���鿴`.fini_array`������ע��������ϣ�[���ӳ���Ϳ�ָ��](http://docs.oracle.com/cd/E19253-01/819-7050/chapter3-1/)��[��ʼ������ֹ��](http://docs.oracle.com/cd/E19253-01/819-7050/6n918j8mn/index.html#chapter2-48195)��[When will the .fini_array section being used?](http://stackoverflow.com/questions/26292964/when-will-the-fini-array-section-being-used)���ĵ�ַ��

    objdump -h -j .fini_array run-finalizer

����Ȼ���ҵ�`VMA`��Virtual Memory Area�������ڴ�ռ䣩�ĵ�ַ��

    run-finalizer��     �ļ���ʽ elf64-x86-64
    �ڣ�
    Idx Name          Size      VMA               LMA               File off  Algn
    19 .fini_array   00000008  00000000006007d8  00000000006007d8  000007d8  2**3
                     CONTENTS, ALLOC, LOAD, DATA

���������ʹ�����°汾��GCC�����룬�ϰ汾ʹ�ò�ͬ�Ļ������洢`��ֹ����`�����ϣ�[��ʼ������ֹ����](http://docs.oracle.com/cd/E19253-01/819-7050/6n918j8n2/index.html)����

����3����`ADDR`��Ϊ`VMA`��ֵ��

����4���ٴα��룻

����5�����С�

����Ȼ����Ӧ�ÿ��Կ�������Ļ�ϴ�ӡ����"hello world"��

����������������ôʵ�ֵ��أ�

��������[Chapter 11 of Linux Standard Base Core Specification 3.1](http://refspecs.linuxbase.org/LSB_3.1.1/LSB-Core-generic/LSB-Core-generic/specialsections.html)��

> .fini_array ����ڰ�����һ������ָ�����飬�ں���������ֵĿ�ִ���ļ����߹����������Ϊһ����������ֹ�������飨termination array����

> This section holds an array of function pointers that contributes to a single termination array for the executable or shared object containing the section.

��������ǿ�Ƹ���������飬����`hello`�����Ϳ��Ա������ˡ������Ҫ���������web�������Ĵ��룬`ADDR`��ֵ��Ҫ��ͬ���������޸ģ�ʹ��`objdump`����

������������֪�����ͨ������ĳһ���ض��ĵ�ַ��ִ��һ��������������������Ҫ֪�����ǣ����ʹ��`printf`�����������ַ��������ҵ��ܶ����`��ʽ���ַ�������`�Ľ̳̣���������������һ����̵Ľ��͡�

****

ע��`��ʽ���ַ�������`��©��������һ�ֺܹ��ϵ�©�������Բ鿴����Ĳ����˽�һ�£�

* [©���ھ����֮��ʽ���ַ���](http://www.tuicool.com/articles/IniyYzA)
* [��ʽ���ַ�����©������](http://www.freebuf.com/articles/system/74224.html)
* [��ʽ���ַ�������ԭ��ʾ�� ](http://blog.csdn.net/immcss/article/details/6267849)

****

����`printf`��������ͨ��`%n`�����ʽ���ַ�����ȡ�����Ѿ�����˶��ٸ��ַ������磺

    #include <stdio.h>
    int main(){
        int count;
        printf("AB%n", &count);
        printf("\n%d characters printed\n", count);
    }

�������Ϊ��

    AB
    2 characters printed

�������ǿ��Խ��κ��ڴ��ַ����`count`�������Ǹõ�ַ��ֵ�����������Ҫ����һ���ܴ�ĵ�ֵַ�Ļ���������Ҫ��ӡ�������ַ�������Ǻ�ɵ�ġ����˵��ǣ�����һ����ʽ���ַ�`%hn`������`short`������`int`�����ǿ���һ�θ���2���ֽڣ��Ӷ����һ��������Ҫ��4�ֽڵ�ֵ��

������������������һ�£�ʹ������`printf`���ã���������Ҫ��ֵ�������Ǻ���`hello`��ָ��ֵ��������`.fini_array`��

    #include <stdio.h>
    #include <stdlib.h>
    #include <stdint.h>
    
    #define FUNCTION_ADDR ((uint64_t)hello)
    #define DESTADDR 0x0000000000600948
    
    void hello(){
	    printf("\n\n\n\nhello world\n\n");
    }
    
    int main(int argc, char *argv[]){
	    short a= FUNCTION_ADDR & 0xffff;
	    short b = (FUNCTION_ADDR >> 16) & 0xffff;
	    printf("a = %04x b = %04x\n", a, b);fflush(stdout);
        uint64_t *p = (uint64_t*)DESTADDR;
        printf("before: %08lx\n", *p); fflush(stdout);
	    printf("%*c%hn", b, 0, DESTADDR + 2 );fflush(stdout);
        printf("after1: %08lx\n", *p); fflush(stdout);
	    printf("%*c%hn", a, 0, DESTADDR);fflush(stdout);
        printf("after2: %08lx\n", *p); fflush(stdout);
	    return 0;
    }

�������йؼ��Ĳ����ǣ�

    short a= FUNCTION_ADDR & 0xffff;
	short b = (FUNCTION_ADDR >> 16) & 0xffff;
	printf("%*c%hn", b, 0, DESTADDR + 2 );
	printf("%*c%hn", a, 0, DESTADDR);

����`a`��`b`��ʵ���Ǻ�����ֵַ��һ�루����ע��ָ��ռ4���ֽڣ�a��b��ǰ��������ֽڣ������ǿ��Թ���һ������Ϊ`a`��`b`���ַ�����`printf`�����������������̫�����ˣ�������ѡ��ʹ��`%*`�����ʽ���ַ���������ͨ����������������ĳ��ȡ�

�������磬����Ĵ��룺

    printf("%*c", 10, 'A');

�����������ӡ9���ո񣬽���������ַ�`A`�����ԣ��ܹ����10���ַ���

�������������ֻ��һ��`printf`�����Ǿ���Ҫ�ǵã��Ѿ���ӡ��`b`���ֽڣ��������ǻ���Ҫ��ӡ�����`b-a`������Ϊ�������ֽڣ�����ļ��������ۼӵģ���

    printf("%*c%hn%*c%hn", b, 0, DESTADDR + 2, b-a, 0, DESTADDR );

��������������ʹ�õ���`hello`����������ʵ�������ǿ��Ե����κκ���������˵���κε�ַ�������Ѿ���д��һ��ֻ���`Hello world`��web��������`shellcode`�����£�

    unsigned char hello[] = 
		"\xeb\x3d\x48\x54\x54\x50\x2f\x31\x2e\x30\x20\x32"
		"\x30\x30\x0d\x0a\x43\x6f\x6e\x74\x65\x6e\x74\x2d"
		"\x74\x79\x70\x65\x3a\x74\x65\x78\x74\x2f\x68\x74"
		"\x6d\x6c\x0d\x0a\x0d\x0a\x3c\x68\x31\x3e\x48\x65"
		"\x6c\x6c\x6f\x20\x57\x6f\x72\x6c\x64\x21\x3c\x2f"
		"\x68\x31\x3e\x4c\x8d\x2d\xbc\xff\xff\xff\x48\x89"
		"\xe3\x48\x83\xeb\x10\x48\x31\xc0\x50\x66\xb8\x1f"
		"\x90\xc1\xe0\x10\xb0\x02\x50\x31\xd2\x31\xf6\xff"
		"\xc6\x89\xf7\xff\xc7\x31\xc0\xb0\x29\x0f\x05\x49"
		"\x89\xc2\x31\xd2\xb2\x10\x48\x89\xde\x89\xc7\x31"
		"\xc0\xb0\x31\x0f\x05\x31\xc0\xb0\x05\x89\xc6\x4c"
		"\x89\xd0\x89\xc7\x31\xc0\xb0\x32\x0f\x05\x31\xd2"
		"\x31\xf6\x4c\x89\xd0\x89\xc7\x31\xc0\xb0\x2b\x0f"
		"\x05\x49\x89\xc4\x48\x31\xd2\xb2\x3d\x4c\x89\xee"
		"\x4c\x89\xe7\x31\xc0\xff\xc0\x0f\x05\x31\xf6\xff"
		"\xc6\xff\xc6\x4c\x89\xe7\x31\xc0\xb0\x30\x0f\x05"
		"\x4c\x89\xe7\x31\xc0\xb0\x03\x0f\x05\xeb\xc3";

****

ע��`shellcode`��һ���������©����ִ�е��ض����롣����������£�

* [Shellcode��ԭ����д ](http://blog.csdn.net/maotoula/article/details/18502679)
* [Shellcode�ı�д](http://blog.chinaunix.net/uid-24917554-id-3506660.html)
* [ShellCode�ļ�ԭ��](http://blog.chinaunix.net/uid-571104-id-2734570.html)
* [��ջ������������ŵ���������дshell code](http://netsecurity.51cto.com/art/201211/367420.htm)
* [�˽�ڿ͵Ĺؼ�����---�ҿ�Shellcode��������ɴ](http://zhaisj.blog.51cto.com/219066/61428/)

****

����������ǽ�`hello`�����Ƴ������������`shellcode`�Ļ������`shellcode`�ͻᱻִ�С�

�������`shellcode`��ʵ����һ���ַ���������˵�����Կ��������������ǿ��Խ������뵽`%*c%hn%*c%hn`�ĺ��档����ַ�����δ�����ģ�����������Ҫ�ڱ�����ҵ����ĵ�ַ��Ϊ�˻�ȡ�����ַ��������Ҫ����������룬Ȼ�󷴱���Ϊ��ࣺ

`objdump -d webserver`

    ......
    ......
    00000000004004e6 <main>:
      4004e6:	55                   	push   %rbp
      4004e7:	48 89 e5             	mov    %rsp,%rbp
      4004ea:	48 83 ec 10          	sub    $0x10,%rsp
      4004ee:	89 7d fc             	mov    %edi,-0x4(%rbp)
      4004f1:	48 89 75 f0          	mov    %rsi,-0x10(%rbp)
      4004f5:	48 83 ec 08          	sub    $0x8,%rsp
      4004f9:	68 d8 07 60 00       	pushq  $0x6007d8
      4004fe:	41 b9 00 00 00 00    	mov    $0x0,%r9d
      400504:	41 b8 94 05 00 00    	mov    $0x594,%r8d
      40050a:	b9 da 07 60 00       	mov    $0x6007da,%ecx
      40050f:	ba 00 00 00 00       	mov    $0x0,%edx
      400514:	be 40 00 00 00       	mov    $0x40,%esi
      400519:	bf c8 05 40 00       	mov    $0x4005c8,%edi
      40051e:	b8 00 00 00 00       	mov    $0x0,%eax
      400523:	e8 98 fe ff ff       	callq  4003c0 <printf@plt>
      400528:	48 83 c4 10          	add    $0x10,%rsp
      40052c:	b8 00 00 00 00       	mov    $0x0,%eax
      400531:	c9                   	leaveq 
      400532:	c3                   	retq   
      400533:	66 2e 0f 1f 84 00 00 	nopw   %cs:0x0(%rax,%rax,1)
      40053a:	00 00 00 
      40053d:	0f 1f 00             	nopl   (%rax)
    ......
    ......

����������Ҫ���ĵ�����һ�У�

    mov    $0x4005c8,%edi

�������`0x4005c8`����������������Ҫ�ĵ�ַ��

    #define FUNCTION_ADDR ((uint64_t)0x4005c8 + 12)

���������`+12`�Ǳ���ģ���Ϊ���ǵ�`shellcode`�������ַ���`%*c%hn%*c%hn`�󣬶����ĳ�����12���ַ���

����������`shellcode`����Դ����Ȥ�Ļ�������ʵ�������µ�C���룺

    #include<stdio.h>
    #include<string.h>
    #include<stdlib.h>
    #include<unistd.h>
    #include<sys/types.h>
    #include<sys/stat.h>
    #include<sys/socket.h>
    #include<arpa/inet.h>
    #include<netdb.h>
    #include<signal.h>
    #include<fcntl.h>
    
    int main(int argc, char *argv[]){
	    int sockfd = socket(AF_INET, SOCK_STREAM, 0);
	    struct sockaddr_in serv_addr;
	    bzero((char *)&serv_addr, sizeof(serv_addr));
        serv_addr.sin_family = AF_INET;
        serv_addr.sin_addr.s_addr = INADDR_ANY;
        serv_addr.sin_port = htons(8080);
	    bind(sockfd, (struct sockaddr *)&serv_addr, sizeof(serv_addr));
	    listen(sockfd, 5);
	    while (1) {
		    int cfd  = accept(sockfd, 0, 0);
		    char *s = "HTTP/1.0 200\r\nContent-type:text/html\r\n\r\n<h1>Hello world!</h1>"; 
		    if (fork()==0) {
			    write(cfd, s, strlen(s));
			    shutdown(cfd, SHUT_RDWR);
			    close(cfd);
		    }	
	    }
        
	    return 0;
    }

�������⣬�һ�����һЩ����Ĺ�������`shellcode`���Ƴ������е�`NUL`�ַ���������������Ǳ���ģ���

> `Jeff Dean`��һ����һ��printfʵ����һ��web����������������ʦ�������ǧ��ע�͵���Ȼ�޷���ȫ��������乤��ԭ�������������ǽ���Google��ҳ��ǰ�ˡ�

������Ϊ��ϰ�����߿����Լ��޸�һ�����web����������������֧��`Google����`ǰ�˵ļ��ء�

������ƪ���͵Ĵ�������������ҵ���[https://github.com/yohanes/printf-webserver](https://github.com/yohanes/printf-webserver)

����������Щ��Ϊ��������ô����ˣ���ֻ��˵��û�������Ǻ����ô�����ֻ��ǡ��ϲ�������ս�������ڽ���������Ĺ����У���ˢ���˶���Щ֪ʶ����ʶ��`shellcode`�ı�д�����Ѿ�����ûд���ˣ���`AMD64`ƽ̨�Ļ�ࡢϵͳ���á�`objdump`��`.fini_array`���ϴ��Ҽ��ʱ������gcc������`.dtors`����`printf`�ĸ�ʽ���ַ�©����`gdb`��ʹ�ü��ɣ����罫�ڴ�鵼��Ϊ�ļ����Լ��Ͳ�ε�`socket`��̣�֮ǰ��һֱʹ��`boost`����

****

#### д�ں���

****

��������������Ц��ʱ��û���뵽��Ŀ���ʵ�֣�ԭ�����������һ����geek�Ĵ����ڷ���Ĺ����У���ѧ���˺ܶණ�������綯̬���ӡ��ڴ沼�֡���������ջ����ʽ���ַ��������ȵȡ�����ıȽ���㣬�ܶ�����û�Ӵ�����Ҳ���벻������������ò�����ˬ������ֱ�ӿ�ԭ�ġ�

�����ҵ����н����

![](https://www.w-angler.com/static/images/2016-12-05_8180005E3725B1E0A9205E0FFCCD8F40.png)

![](https://www.w-angler.com/static/images/2016-12-05_F57336A2CABC27D770FD76FD96A63F06.png)

****

#### ����

**** 

�����ܶ���˵û�п����ף��Ҿ͸����Լ���������һ�°ɡ�

�������ȣ������漰�ļ�����`��̬���ӿ�`�ļ��ء���ʼ����ж�ء���ֹ�ȣ���������ص����ϣ���`��ʽ���ַ�������`��`shellcode`��ԭ����д����

�����ؼ���һ���ǣ�����`.fini_array`����ڣ�����ڵĴ��뽫���ڶ�̬���ӿ⣨���߿�ִ�г���ж��ʱ��ִ�С������ٴθ�����ص��������ӣ�[��ʼ������ֹ��](http://docs.oracle.com/cd/E19253-01/819-7050/6n918j8mn/index.html#chapter2-48195)����������Ҳ����֪����Ϊʲô��Ҫ������ֹ�ڶ����ǳ�ʼ���ڣ���Ϊ������Ҫ��ʹ��printf()�޸�.fini_array�ڵ����ݣ�Ȼ���ڳ������ʱ������ڵ����ݾͻᱻִ�У����ǳ�ʼ���ڵĻ���û��ʲô�ã���Ϊ�����Ѿ�ִ�й��޸�ǰ�ĳ�ʼ�������ˡ����`��ʼ��`��`��ֹ��`���������ΪC++�е�`���캯��`��`��������`��ֻ�������漰�Ĳ����࣬�������ӿ���߿�ִ�г���

������Σ���ʽ���ַ��������������һ����������Ͼ��������ˣ���Ҫԭ�����ͨ����ʽ���ַ����޸�ĳ���ڴ��ַ��ֵ���������ͨ��������޸�.fini_array��

�������`shellcode`���֣�����ͺ�geek�ˣ�����ԭ���Լ�shellcode�ı�д��������ص����ϡ�ԭ�������Ѿ���д��һ���򵥵�web��������shellcode��.fini_array��ֵ���޸�Ϊ���ĵ�ַ�����������ڳ������ʱ��ִ�С������ϣ�ֻҪ���shellcode����ô���ӣ��㼸������ʹ��һ��printfʵ���κι��ܡ�

