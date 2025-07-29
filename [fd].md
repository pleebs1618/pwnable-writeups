1.When browsing directory, fd, fd.c, and flag file exist.
2. Looking through fd.c, we can notice code that could lead to flag.


```
cpp

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
char buf[32];
int main(int argc, char* argv[], char* envp[]){
        if(argc<2){
                printf("pass argv[1] a number\n");
                return 0;
        }
        int fd = atoi( argv[1] ) - 0x1234;
        int len = 0;
        len = read(fd, buf, 32);
        if(!strcmp("LETMEWIN\n", buf)){
                printf("good job :)\n");
                setregid(getegid(), getegid());
                system("/bin/cat flag");
                exit(0);
        }
        printf("learn about Linux file IO\n");
        return 0;

}
```

atoi - ASCII to Integer
fd = ASCII to Integer - 0x1234 - normally 0 (standard input of keyboard)
read() - File Descriptor, stores read data to buff, with length of 32 bytes.

When we change 0x1234 to integer - of 4660, and input, we can type string for comparison - here we type LETMEWIN, as it is checking if strings are equal.

This leads to flag being revealed.

Flag: Mama! Now_I_understand_what_file_descriptors_are!
