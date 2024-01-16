In lecture this week, we learned about the commands **cd**, **ls** and **cat**.
They can provide you ease of access to specific files and directories. 

Here are examples of me using the commands with *no* arguments provided:

```
[user@sahara ~]$ cd
[user@sahara ~]$ ls
lecture1
[user@sahara ~]$ cat
```
Without using a specific argument, `cd` will return you to the home directory.
As such, no change in which file directory we are in happens.

The `ls` command only lists out the name of the current directory, the home directory. 
Since We didn't provide arguments to the `cd` command, what is being printed is the contents of the home directory,
which is the `lecture1` directory within.

The `cat` command prints out the contents of files provided as arguments. Since we are not providing
any file to be used as an argument, nothing will be concatenated in the terminal. 

Here's what happens when I use the commands with a path to a *directory* as an argument:

```
[user@sahara ~]$ cd lecture1
[user@sahara ~/lecture1]$ cd ..
[user@sahara ~]$ ls lecture1
Hello.class  Hello.java  messages  README
[user@sahara ~]$ cat lecture1
cat: lecture1: Is a directory
[user@sahara ~]$ 
```

Prompting the `cd` command with the directory named lecture1 will change the working directory from
the home directory to the lecture1 directory. 

I cd'd out of the lecture1 directory to work within the home directory again. When running the `ls` command with
the lecture1 directory as an argument, the contents of the directory are shown, and the sub directory `messages` is shown
in bold lettering. 

While still working in the home directory, I typed the `cat` command with the lecture1 directory as an argument, to which I was
presented with a line that stated *cat: lecture1: Is a directory* in the terminal after running. This is because the `cat` command
is designed to print out the contents of files, not directories, so this command execution throws an error.

This is what happens when I use the commands with a path to a *file* as an argument:

```
[user@sahara ~]$ cd lecture1/messages/es-mx.txt 
bash: cd: lecture1/messages/es-mx.txt: Not a directory
[user@sahara ~]$ ls lecture1/messages/es-mx.txt 
lecture1/messages/es-mx.txt
[user@sahara ~]$ cat lecture1/messages/es-mx.txt
¡Hola Mundo!
```

When I use the path to a file as an argument for the `cd` command, I am returned the error message: `bash: cd: lecture1/messages/es-mx.txt: Not a directory`.
The `cd` command allows you to traverse through different directories, but not files themselves. As such, this message appears to notify the user that what they
are doing is an error and can only be done on directories.

Using the `ls` command on the file path prompted the message: `lecture1/messages/es-mx.txt`. This is not an error message, but only the name of the file is displayed.
This is because the `ls` command is designed to list information about the file/directory prompted. 

When I use the `cat` command on the file path, I will get a message that prints the contents of the `es-mx.txt` file. The `cat` command here will concatenate
the contents of the file provided, and in this case, that is `¡Hola Mundo!`.





