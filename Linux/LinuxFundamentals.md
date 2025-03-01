# Linux Fundamentals, 2nd Edition
Instructor: Sander van Vugt

## Module 1: Essential Commands
### Lesson 1: Installing Linux
#### 1.2 Understanding Linux Distributions
Red Hat and Debian are major Linux distribution families.

Fedora -> CentOS Stream -> RHEL -> Rocky/Alma/Oracle linux

Debian -> Ubuntu -> Linux Mint/Kali Linux

### Lesson 2: Using Essential Tools
#### 2.2 Using su and sudo
Using Administrator Priveleges  
- User root is unrestricted and for that reason you should avoid working as root
- It is better to use **sudo**. Users that are a member of the group **wheel** (Red Hat) or **sudo** (Ubuntu) can use **sudo** to run commands with administrator privileges
- Alternatively, **su -** can be used to open a shell as another user
    - Always use **su -** to ensure full access to the target user environment
    - When used without arguements, a root shell is opened after entering the root password
    - When used with a username as arguement, a user shell is opened

What is sudo
- **sudo** allows authorized users to run tasks with escalated priveleges
- To use it, use **sudo command: sudo ls /root**
- To allow sudo usage, users must be a member of the sudo group
    - **wheel** on Red Hat
    - **sudo** on Ubuntu

#### 2.4 Using the Seven Essential Linux Command Line Tools
- whoami
- ls
- ip a
- cat
- cat
- passwd
- touch
- pwd

#### 2.5 Getting Help with man
- The main - but not the only - source for getting information about Linux command usage
- Use **man** to get information about commands, configuration files, and more  

Common Elements of man Pages
- Command summary: shows how to use a command
    - Items between **[brackets]** are optional
    - If you see **{ a | b }** or **a | b** you must choose
    - **...** means you may have more of the preceding item. ex. [OPTION]...
- man pages have sections
    - 1 is for end-user commands
    - 8 is for administrator (root) commands
    - 5 is for configuration files
- Many man pages have examples near the end
- Otherwise, they'll have related items near the end
- Use /sometext to search for sometext; n for next
- Use g to go to top of the man page
- Use G to go to bottom of the man page
- Use q to get out of the man page


#### 2.6 Finding the Right man Page
- man pages are summarized in the mandb
- Use man -k to search the mandb

### Lesson 3: Essential File Management Tools
#### 3.1 Understanding the Linux File System Hierarchy
Why are Files Important?
- On Linux, everything is a file
- Configuration is stored in mostly ASCII text configuration files
- Devices are addressed by using device files

Understanding the Linux Filesystem
- Directories are highly standardized on Linux
    - /usr -> program files; where you'll find all commands  in linux environment
    - /var -> directory services are using to dynamically create files. ex. /var/log, /var/cache
    - /etc -> configuration files
- Complete documentation is in the Filesystem Hierarchy Standard (https://refspecs.linuxfoundation.org/fhs.shtml)
- Regular users have write-access to two directories only:
    - /home
    - /tmp
- Tip! use **touch filename** to verify write access to a location

- **man hier**  to view filesystem hierarchy

#### 3.2 Listing Files with ls
Using ls
- Use **pwd** to find the name of the current directory
- **ls** us used to list files and directories
- **ls -a** will show hidden files
- **ls -l** provides long listing (many file details)
- Use **ls -ld /directory** to see properties and not contents of a directory
- **ls -lrt** shows a time-sorted list of files

#### 3.3 Using Wildcards
- Most file-oriented commands support using wildcards:
    - **ls a***
    - **ls a?***
    - **ls a[nm]***
    - **ls [a-e]***
- Many commands also support working with groups and ranges:
    - **touch file{1..100}**
    - **mkdir /data/{sales,account}**

#### 3.4 Copying Files with cp
- Use **cp /dir/somefile /somedir** to copy a file
- To have the command fail if /somedir doesn't exist, change to **cp /dir/somefile /somedir/** (destination must be a directory)
- Use **cp -a** to copy all files (including hidden); but use the .* wildcard to include hidden files as well
    - **cp -a ~/.* /tmp/**
    - Notice that **tar** (covered later) is a better solution to archive hidden files
- **cp -R** will copy recursively: including subdirectories

#### 3.5 Working with Directories
- **cd** is used to change directory
- **mkdir** creates a directory
- **rmdir** removes an empty directory

#### 3.6 Using Absolute and Relative Paths
Understanding Path Names
- An absolute path contains the full name from the root directory to a file
    - /var/log/messages
- A relative path is related to the current directory and contains the rest that is needed to get to a file
    - log/messages is current directory is set to /var
- In relative paths, **..**  can be used for "one directory up"
- Tip! Use absolute paths to avoid confusion

#### 3.7 Moving Files with mv
- The **mv** command will move a file
- If you move a file, you take up the file with all of its properties and you put it in the new location. That requires sufficient priveleges to take it away from the current location.
- It is also used to rename a file

#### 3.8 Removing Files with rm
- **rmdir** is used to remove empty directories
- **rm** is used to remove files as well as directories with contents
- **rm -i** will prompt for confirmation on all files
- **rm -f** will force and don't ask anthing
- **rm -rf** removes an entire directory tree without asking any confirmation
- **rm -rf / --no-preserve-root** will remove absolutely everything

### Lesson 4: Advanced File Management Tools
#### 4.1 Understanding Hard and Symbolic Links
- A link is a file system entry that refers to another file or directory
- Hard links are pointing to the same inode on the same file system
- Every file in Linux filesystems has an inode
- An inode (index node) is a data structure in a filesystem that stores metadata about a file, such as its size, owner, permissions, and location on disk—but not its name or directory path.
- Symbolic links are shortcuts and add additional flexibility
    - Sybolic links can exist on a directory
    - Cross-device symbolic links can be used

- A hard link is a direct reference to the same inode as the original file. Deleting the original file does not remove the data as long as at least one hard link exists.

- A symlink (symbolic link) is a shortcut pointing to the original file's path. If the original file is deleted, the symlink becomes broken (dangling).

#### 4.2 Managing Hard and Symbolic Links
- **ln TARGET LINK_NAME** to create hard link
- **ls -il FILE** to print inode number + hard link counter
- **ln -s TARGET LINK_NAME** to create symbolic link
- symbolic links should always point to an absolute file name to make them reliable

#### 4.4 Finding Files with find
Demo: Basic find usage
- **find / -name "hosts"**
- **find / -name "hosts\*"**
- **find / -user "linda"**
- **find / -size +2G**
- **find / -user linda -exec cp {} /root/linda \;**
    - {} acts as a placeholder for each file that find locates. 
    - use \; whenever -exec is used in find
- **find / -perm /4000**