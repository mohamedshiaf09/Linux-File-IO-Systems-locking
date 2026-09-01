# Linux-File-IO-Systems-locking
Ex07-Linux File-IO Systems-locking
# AIM:
To Write a C program that illustrates files copying and locking

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux IO Systems locking

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## 1.To Write a C program that illustrates files copying 

```

#include <unistd.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdlib.h>
#include <stdio.h>

int main(int argc, char *argv[]) {
    if (argc != 3) {
        fprintf(stderr, "Usage: %s <source_file> <destination_file>\n", argv[0]);
        exit(EXIT_FAILURE);
    }

    char block[1024];
    int in, out;
    ssize_t nread;

    // Open source file
    in = open(argv[1], O_RDONLY);
    if (in == -1) {
        perror("Error opening source file");
        exit(EXIT_FAILURE);
    }

    // Open destination file
    out = open(argv[2], O_WRONLY | O_CREAT | O_TRUNC, S_IRUSR | S_IWUSR);
    if (out == -1) {
        perror("Error opening destination file");
        close(in);
        exit(EXIT_FAILURE);
    }

    // Copy contents
    while ((nread = read(in, block, sizeof(block))) > 0) {
        if (write(out, block, nread) != nread) {
            perror("Error writing to destination file");
            close(in);
            close(out);
            exit(EXIT_FAILURE);
        }
    }

    if (nread == -1) {
        perror("Error reading source file");
    }

    close(in);
    close(out);
    return EXIT_SUCCESS;
}

```
## OUTPUT

<img width="763" height="405" alt="image" src="https://github.com/user-attachments/assets/90bdac1c-dc91-428f-b439-cf4fde445256" />

## 2.To Write a C program that illustrates files locking
```

#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/file.h>

int main(int argc, char *argv[])
{
    int fd;

    if (argc != 2)
    {
        printf("Usage: %s <filename>\n", argv[0]);
        return 1;
    }

    fd = open(argv[1], O_WRONLY);

    if (fd == -1)
    {
        perror("Error opening file");
        return 1;
    }

    printf("Opening file: %s\n", argv[1]);

    if (flock(fd, LOCK_EX) == -1)
    {
        perror("Error locking file");
        close(fd);
        return 1;
    }

    printf("File locked successfully.\n");
    printf("File is locked for 10 seconds...\n");

    sleep(10);

    if (flock(fd, LOCK_UN) == -1)
    {
        perror("Error unlocking file");
        close(fd);
        return 1;
    }

    printf("File unlocked successfully.\n");

    close(fd);

    return 0;
}

```
## OUTPUT

<img width="802" height="602" alt="image" src="https://github.com/user-attachments/assets/5fbe2f2f-173c-4093-b363-57b08789f7e7" />

# RESULT:
The programs are executed successfully.
