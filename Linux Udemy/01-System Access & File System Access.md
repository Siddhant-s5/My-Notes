### File System Access
- There are paths to navigate to a File system:
	1. Absolute path
	2. Relative path
- An Absolute path always begins with '/'. This indicates that the path starts at the root directory.
	e.g. `cd /var/log/samba`
- A relative path does not begin with '/'. It identifies a location relative to your current position.
	e.g. `cd log`
	   `cd samba`
### Creating Files and Directories
   `touch` --> creates empty file
   `cp` --> copy contents of one file into another file or in a new file
   `vi` --> creates new file and simultaneously launches editor to edit file, also used as an editor for editing a file
   `mkdir` --> creates new directory
   `ls` --> list contents of directory
   `ls -l` --> list all files and directories in alphabetical order
   `ls -lt` --> list all files and directories in time-wise order
   `ls -ltr` --> list all files and directories in reverse time-wise order
- Two main commands are used to find files or directories:
	1. `find`
	2. `locate`
	`find . -name name_of_file`
		-> `.` represents the present working directory, so this command will start finding the file from current directory
	`locate name_of_file`
	- If `locate` command does not output any result then as root run `updatedb`. Also make sure you have *mlocate* package installed.
		to check => `rpm -qa | grep mlocate`
		to install => `yum install mlocate`
	- `locate` uses a prebuilt database, which should be regularly updated, while `find` iterates over a filesystem to locate files. Thus, `locate` is much faster than `find`, but can be inaccurate if the database ( can be seen as a cache ) is not updated.
### Wildcard
- A *wildcard* is a character that can be used as a substitute for any of a class of charaters in a search.
- There are many wildcards present, but basic onces are listed below:
	`*` --> represents zero or more characters
	`?` --> represents a single character
	`[ ]` --> represents a range of characters
	e.g. 
	`touch abcd{1..9}-xyz` -> creates 9 files named abcd1-xyz, abcd2-xyz,..., abcd9-xyz
	`ls -l abc*` -> list all files starts with abc
	`rm a*` -> remove all files starts with a
	`rm *xyz` -> remove all files ends with xyz
	`ls -l ?bcd*` -> list all files in which whatever character (single) comes at first and whatever or nothing comes after d
	`ls -l *[cd]*` -> list all files which has either c or d or c&d both
- Some other wildcards:
	`\` --> as an escape character
	`^` --> the beginning of the line
	`$` --> the end of the line
### Linux File Types

| File Symbol |           Meaning           |
| :---------: | :-------------------------: |
|      -      |        Regular file         |
|      d      |          Directory          |
|      l      |            Link             |
|      c      | Special file or device file |
|      s      |           Socket            |
|      p      |         Named pipe          |
|      b      |        Block device         |
- Regular files consists of text files, executable files, images, videos, etc.
- links are special files that point to other files or directories. Link have two types: soft link and hard link
- Special file or device file represents hardware devices i.e. if we connect keyboard, mouse, memory, etc system creates file for those.
- Also another device file is block device file, these file represents hardware devices that handle data in blocks, such as hard drives or usb drives; Identification of block device files is starting with 'b'.
- Socket files are used for network communication between processes allowing data exchange between them.
- Named pipe also known as FIFO, these files are used for interprocess communication inside our computer. They allow data to be passed from one process to another in a first in, first out manner.
### Soft links and Hard links
##### inode -
- Index Node (inode) is a data structure in a unix-style file system that contains metadata about a file or directory.
- This metadata includes information like file size, permissions, owner, access times, and ***the location of the file's data blocks on the disk***.
- It's pointer or number of a file on the hard disk.
- Essencially, an inode is a unique identifier for a file or directory.

**Soft link** - Link will be removed if file is removed or renamed
**Hard link** - Deleting, remaning or moving the original file will not affect the hard link

to create hard link => `ln`
to create soft link => `ln -s`
to see inode number of files, i option is used => `ls -li`

![[Pasted image 20250702145859.png]]
- You can create soft or hard link within the same directory with the same name
- If while creating hard link you get the error like 'Invalid cross-device link', then it is most likely that your directory in which file is present and the directory in which you are creating hard link are not present on the same *partition*. Hard links only work within the same partition.
- Creating hard link is nothing but creating another file which uses same inode data/values, that's why if we run `ls -ltr` in directory in which hard-link is created, it does not show 'l' in front of that hard-link but rather it shows '-' i.e. normal file. That's why even if we delete original file, contents of that file remains in hard-link.
![[Pasted image 20250702151319.png]]

