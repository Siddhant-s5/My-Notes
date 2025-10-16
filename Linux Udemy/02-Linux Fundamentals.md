### Command Syntax
		command option(s) argument(s)
`options` - modify the way that a command works, usually consist of a hyphen/dash (-) followed by a single letter. Some commands accept multiple options which can usually be grouped together after a single hyphen e.g. `ls -ltri` -> options for list, time reverse and inode number

![[Pasted image 20250705093112.png]]
### File Permissions
- Unix is a multi-user system. Every file and directory in your account can be protected from or made accessible to other users by changing its access permissions. Every user has responsibility for controlling access to their files.
- Permissions for a file or directory access may be restricted by three types of permissions:
1. r - read
2. w - write
3. x - execute (running a program)
- Each permission (rwx) can be controlled at three levels:
1. u - user (yourself)
2. g - group (can be people in same project)
3. o - others (everyone on the system)
- File or directory permission can be displayed by running `ls -l` command
![[Pasted image 20250705101341.png]]
- Command used to change permission is `chmod`. e.g. suppose we have file named 'jerry' having permissions as *-rw-rw-r--*, so
  `chmod g-w jerry` => remove write permission from group (*-rw-r--r--*)
  `chmod a-r jerry` => remove read permission from all (*--w-------*)
  `chmod u-w jerry` => remove write permission from user (*----------*)
  `chmod u+rw jerry` => add read & write permission to user (*-rw-------*)
- Execute permission for directory means we can go inside that directory i.e. we can `cd` into that directory
- Permission to a file and directory can also be assigned numerically
![[Pasted image 20250705104318.png]]
### File Ownership
- There are 2 owners of a file or directory
  1. user
  2. group
- Commands to change file ownership are
  `chown` and `chgrp`
  - `chown` changes the user ownership of a file
  - `chgrp` changes the group ownership of a file
- Recursive ownership change option (cascade) (-R): If we use '-R' option then it will not only change the ownership of directory but the files & directories present inside the directory
- By running command `ls -l` we get owner in third column and owner group in fourth column.
- Ownership can only be changed by a ***root*** user
![[Pasted image 20250705214542.png]]
- If we are the owner of a parent directory and have all permissions (rwx) then we can edit/delete the files or directories inside that parent directory even if we are not the owner or in ownership group of that file/directory. e.g. suppose file named 'myFile' is present at */home/sid/Desktop* and has ownership as *san san*, and if i logged in as *sid* and try to delete that file (`rm myFile`), I can delete it because if we check the ownership and permissions of folder `/home/sid` it will be as ***drwx------ sid sid***. So the user *sid* has all permissions (rwx) and that is why it was able to delete that file.
- Check the ownership & permissions by running command `ls -l` in **/** (root directory).
### Access Control List (ACL)
- ACL provides an additional, more flexible permission mechanism for file systems. It is designed to assist with UNIX file permissions. ACL allows you to give permissions for any user or group to any disc resource.
- Think of a scenario in which a particular user is not a member of a group created by you but still, you want to give some read or write access, how can you do it without making the user a member of the group, here comes in picture Access Control Lists. Basically, ACLs are used to make a flexible permission mechanism in Linux. From Linux man pages, ACLs are used to define more fine-grained discretionary access rights for files and directories.
- Commands to assign and remove ACL permissions are
  `setfacl` and `getfacl`
  1. to add permission for user
    `setfacl -m u:user:rwx /path/to/file`
  2. to add permission for group
    `setfacl -m g:group:rw /path/to/file`
  3. to allow all files or directories to inherit ACL entries from the directory it is within
    `setfacl -Rm "entry" /path/to/dir`
  4. to remove a specific entry
    `setfacl -x u:user /path/to/file` (for a specific user)
  5. to remove all entries
    `setfacl -b path/to/file` (for all users)
- As you assign the ACL permission to a file or directory it adds '+' sign at the end of the permission.
- Setting 'w' permission with ACL does not allow to remove a file.
- `getfacl myFile` --> give all ownership info about file.
- Why to use ACL?
  Suppose a scenario, in which we want to give read or write permission to a particular user which is not present in our group. So either we need to add that user in the group or we need to give read/write permission to others(o). But, giving permissions to others will exploit file to everyone i.e. for all users. And we also don't want to add a user into a group just to access one file. In such case we can use ACL.
  `setfacl -m u:san:rw /tmp/tx`
  `getfacl /tmp/tx`
### Help Commands
There are three types of help commands:
1. `whatis command`
2. `command --help`
3. `man command`
just remember while using man command, use *space* key to go on next page, and use *q* key to exit
### Adding Text to Files
- 3 simple ways to add text to a file
  1. using text editors like vi, nano, etc.
  2. Redirect command output > or >>
  3. `echo` > or >>
- `ls -ltr > myFile` => output of `ls -ltr` command will be redirected to (written in) myFile
- `echo` command actually echos back what we have talked. 
  e.g. `echo "my name is sid"` => output: *my name is sid*
  ![[Pasted image 20250710211206.png]]

- **>** and **>>** are used to redirecting the output of a command to a file.
  **>**  => this will delete the previous contents of file
  **>>**  => this will add new contents after old contents
Note: In linux, everything is treated as a file. e.g. text files, directories, keyboard/mouse/devices connected to the system, etc.
Some Important Commands:
  `cat myFile` => print contents of myFile
  `ls -la` => list all files & directories including hidden files/directories (hidden are the files/directories which has naming starts from dot(.) eg => .myHiddenFile)
### Input and Output Redirects
- **What is Redirection?**
  Redirection is a way to send the output of a command somewhere else, or to take input for a command from somewhere other than the keyboard.
- There are 3 redirects in linux
  1. Standard input (stdin) and it has file descriptor number as 0
  2. Standard output (stdout) and it has file descriptor number as 1
  3. Standard error (stderr) and it has file descriptor number as 2
- **Output Redirection (> and >>)**
  By default, when you run a command, the output appears on the screen i.e. on terminal, but you can redirect that output into a file using output redirection.
  to override use > (e.g. `echo "Hello World" > output.txt`)
  to append use >> (e.g. `echo "Hello Again!" >> output.txt`)
- **Input Redirection (<)**
  It is used to feed file contents to a command. i.e. to provide contents of a file as an input to a command instead of typing it. e.g.
  1. `mail -s "Office memo" random@gmail.com < myContentFile` -> Here mail is sent and body/content of mail is picked from file named myContentFile
  2. `sort < names.txt` -> sort command takes its input from names.txt instead of writing for you to type names manually
  3. `cat < myFile`
- **Stderr (Error) Redirection (2>)**
  When a command is executed we use a keyboard and that is also considered stdin-0 (standard input), that command's output goes on the monitor and that output is stdout-1 (standard output), if the command produces any error on the screen then it is considered stderr-2 (standard error). Errors can be redirected by using 2>.
  `command 2> myErrorFile.txt`
  **2>** => redirects error messages (file descriptor 2)
  **>** => redirects normal output (file descriptor 1)
  e.g. 
  1. sid => `ls -l /root` --> this will throw an error as root directory is not accessible for used sid. So, this is considered stderr-2, we can use redirects to route errors from screen to our file. sid => `ls -l /root 2> errorFile`
  2. `telnet localhost 2> myErrorFile` --> if telnet gets failed to connect then error is stored in myErrorFile. 
  If the command gets executed perfectly then nothing will be stored in file.
### Standard Output to a File (tee)
