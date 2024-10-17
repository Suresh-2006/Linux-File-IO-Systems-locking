# Ex07-Linux-File-IO-Systems-locking
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
#include <sys/stat.h>
#include <fcntl.h>
#include <stdlib.h>
#include <stdio.h> // Include stdio.h for perror()

int main() {
    char block[1024];
    int in, out;
    int nread;

    in = open("filecopy.c", O_RDONLY);

    out = open("file.out", O_WRONLY | O_CREAT | O_TRUNC, S_IRUSR | S_IWUSR);

    exit(EXIT_SUCCESS);
}
```
## 2.To Write a C program that illustrates files locking
```
#include <fcntl.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <sys/file.h>

int main(int argc, char* argv[]) {
    if (argc != 2) {
        printf("Usage: %s <filename>\n", argv[0]);
        return 1;
    }

    char* file = argv[1];
    int fd;
    struct flock lock;

    printf("Opening %s\n", file);

    // Open the file with read-write permissions
    fd = open(file, O_RDWR);
    if (fd == -1) {
        perror("Error opening file");
        return 1;
    }

    // Acquire shared lock
    lock.l_type = F_RDLCK; // Shared lock
    lock.l_whence = SEEK_SET;
    lock.l_start = 0;
    lock.l_len = 0;
    if (fcntl(fd, F_SETLK, &lock) == -1) {
        perror("Error acquiring shared lock");
    } else {
        printf("Acquiring shared lock using fcntl\n");
    }
    getchar();

    // Upgrade to exclusive lock
    lock.l_type = F_WRLCK; // Exclusive lock
    if (fcntl(fd, F_SETLK, &lock) == -1) {
        perror("Error acquiring exclusive lock");
    } else {
        printf("Acquiring exclusive lock using fcntl\n");
    }
    getchar();

    // Release lock
    lock.l_type = F_UNLCK;
    if (fcntl(fd, F_SETLK, &lock) == -1) {
        perror("Error releasing lock");
    } else {
        printf("Unlocking\n");
    }
    getchar();

    close(fd);
    return 0;
}
```
# OUTPUT:
## C program that illustrates files copying:
![image](https://github.com/user-attachments/assets/540cfc8e-c0a0-4c13-bd9e-9ff779973c42)


## C program that illustrates files locking:
![image](https://github.com/user-attachments/assets/23af8399-cdea-4007-83d4-13d6e63ffae8)


# RESULT:
The programs are executed successfully.
