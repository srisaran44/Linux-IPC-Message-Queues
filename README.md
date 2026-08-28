# Linux-IPC-Message-Queues
Linux IPC-Message Queues

# AIM:
To write a C program that receives a message from message queue and display them

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux message queues API 

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## C program that receives a message from message queue and display them


SENDER SIDE PROGRAM
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/ipc.h>
#include <sys/msg.h>
#include <string.h>

struct message
{
    long msg_type;
    char msg_text[100];
};

int main()
{
    key_t key;
    int msgid;
    struct message msg;

    key = ftok("msgfile", 65);

    msgid = msgget(key, 0666 | IPC_CREAT);

    if (msgid == -1)
    {
        perror("msgget");
        exit(1);
    }

    msg.msg_type = 1;

    printf("Enter message: ");
    fgets(msg.msg_text, sizeof(msg.msg_text), stdin);

    msg.msg_text[strcspn(msg.msg_text, "\n")] = '\0';

    if (msgsnd(msgid, &msg, sizeof(msg.msg_text), 0) == -1)
    {
        perror("msgsnd");
        exit(1);
    }

    printf("Message sent successfully.\n");

    return 0;
}
```
RECEIVER SIDE PROGRAM
```c


#include <stdio.h>
#include <stdlib.h>
#include <sys/ipc.h>
#include <sys/msg.h>

struct message
{
    long msg_type;
    char msg_text[100];
};

int main()
{
    key_t key;
    int msgid;
    struct message msg;

    key = ftok("msgfile", 65);

    msgid = msgget(key, 0666 | IPC_CREAT);

    if (msgid == -1)
    {
        perror("msgget");
        exit(1);
    }

    if (msgrcv(msgid, &msg, sizeof(msg.msg_text), 1, 0) == -1)
    {
        perror("msgrcv");
        exit(1);
    }

    printf("Message received from message queue: %s\n", msg.msg_text);

    msgctl(msgid, IPC_RMID, NULL);

    return 0;
}


```




## OUTPUT

<img width="574" height="576" alt="image" src="https://github.com/user-attachments/assets/6ae3db54-66a4-4ec3-975b-ccba803eb7c9" />




# RESULT:
The programs are executed successfully.
