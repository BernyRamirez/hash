## Linux Permissions

> Required Reads: How to manage users in Linux Ubuntu. 

The intention of this article is to provide you with a quick summary on the most important topics you should focus to successfully manage file and directory permissions. 

**Access Control Lists (ACLs):** Allow the administrator to enforce extended security attributes on files and directories.

**Standard Access Permissions:** Exist as the default method which works on individual files and directories. 

User access rights are form of **Permission Classes and Types**

> **Classes:** *user* (u), *group* (g), *other* (o)

> **Types:** *read*(r), *write*(w), *execute*(x)

> **Permission Modes:** add (+), revoke (-), assign (=)

The following is the structure of permissions:

| Type of File | User | Group| Other |
| - | - | - | - |
| d (dir)<br /> - (file) | rwx | rwx | rwx |

| Octal | Sym | Description |
| - | - | - |
| 0 | --- | No permissions |
| 1 | --x | Execute Only |
| 2 | -w- | Write only |
| 3 | -wx | Write Execute |
| 4 | r-- | Read Only |
| 5 | r-w | Read Execute |
| 6 | rw- | Read Write |
| 7 | rwx | Read Write Execute |

| X | X | X |
| - | - | - |
| 4 | 2 | 1 |

You can modify permissions in two ways by using the command ***chmode*** which sets the permissions for files and directories

Consider the following file:

> ls -ll | grep file
> 
> -r--r--r--  1 server server        0 May 31 20:29 file

**Symbolic Form**
```
server@server:~$ chmod u+x file -v
mode of 'file' changed from 0444 (r--r--r--) to 0544 (r-xr--r--)
server@server:~$ 
```
In the same way, you can use a combination of *Class, Type and Mode*

i.e  ugo=x  o=r  g-x ... etc. 

**Octal Form**
```
server@server:~$ chmod 544 file -v
mode of 'file' changed from 0777 (rwxrwxrwx) to 0544 (r-xr--r--)
```

The concept of **UMASK**:

Linux assigns default permissions to new files and directories based on the ***umask*** value. 

The ***umask*** is a three-digit octal value that refers to the u,g,o, the default Values are:
* 0022 - Root
* 0002 - All normal users

```
server@server:~$ umask
0002

server@server:~$ umask -S
u=rwx,g=rwx,o=rx
```
The default inital values that Linux assigned are:

* Files: 666 (rw-rw-rw-)
* Directories: 777 (rwxrwxrwx)

Because of the ***umask*** value, your file will be created as 664 and your directory as 775. 
The way you calculate this is by subtracting the ***umask*** value to the default initial values. 

For example, if you want no changes on any new file or directory you create then you apply the following command:

```
server@server:~$ umask -S
u=rwx,g=rwx,o=rx
server@server:~$ umask 000
server@server:~$ umask -S
u=rwx,g=rwx,o=rwx
```
Notice that now our files and directories will be created with the default initial values. 

> I don't recommend umask 000, in fact you should set your ***umask*** based on your security needs. 

## Special File Permissions:

Lastly, there are three more Bits that need to be taken in consideration:

| Bit |  |  | |
| - | - | - | - |
| setuid | Set on binary executable files at the owner level | Allows the file to be executed by no-owners with the same privileges as the user | -rw**S**rwxrwx |
| setgid | Set on binary executable files at the group level | Allows to execute the file by no-owners with the same privileges as the group | -rwxrw**S**rwx |
| sticky | Set on public and shared writable directories | Protect files and subdirectories from being deleted or moved by other users | drwxrwxrw**T** |

Example: setuid
```
server@server:~$ chmod -v u+s file 
mode of 'file' changed from 0544 (r-xr--r--) to 4544 (r-sr--r--)

server@server:~$ chmod -v +4000 file 
mode of 'file' retained as 4544 (r-sr--r--)
```
Example: setgid
```
server@server:~$ chmod -v g+s file
mode of 'file' changed from 4544 (r-sr--r--) to 6544 (r-sr-Sr--)

server@server:~$ chmod -v +2000 file
mode of 'file' retained as 6544 (r-sr-Sr--)
```
Example: sticky
```
server@server:~$ ll | grep dir
drwxrwxrwx  2 server server     4096 May 31 21:16 testdir/
server@server:~$ chmod -v +1000 testdir/
mode of 'testdir/' changed from 0777 (rwxrwxrwx) to 1777 (rwxrwxrwt)
```


## This is it, I hope you enjoyed it !!!
