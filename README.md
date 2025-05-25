# arduino

#Understanding the Unix Philosophy: "Everything is a File"
One of the core philosophies of Unix and Linux is:

"Everything is a file."

This doesn’t just mean documents or scripts — it includes:

Text files
Directories
Hardware devices
Network sockets
Processes (as files in /proc)

#What Does This Mean?
You can interact with hardware the same way you interact with files:

Open a serial port like you open a file.

Read from /dev/ttyUSB0 like you read from a file.

Write to /dev/null to discard output like a sink.

Under the hood, Linux represents many resources as files so that standard system calls (like open(), read(), write()) can be used consistently, regardless of what you’re accessing.

#Where Are These Special Files?
Device files are located in the /dev/ directory.

#Common Examples:
Device File	Description
/dev/sda1	First partition on a hard drive
/dev/ttyACM0	USB serial port (e.g., for Arduino)
/dev/ttyUSB0	USB-to-serial adapter
/dev/ttyS0	Built-in serial port (COM1 equivalent)
/dev/null	Black hole for data (discards all input)

#Types of Device Files
In a long listing (ls -l), the first character tells you what kind of file you're looking at.

#Prefix	Type	Description
-	Regular file	  Text, binary, images, scripts, etc.
d	Directory	      Folder
c	Character device  Data flows one byte/character at a time (e.g. keyboard, serial port)
b	Block device      Data transferred in blocks (e.g. hard drives, SSDs)
l	Symbolic link	  Shortcut to another file

#Let's Inspect a Device File
Check if /dev/ttyACM0 exists and what it is: (I just plugged in my Arduino Uno)
aruna@debian:~$ ls -lh /dev/ttyACM0
crw-rw---- 1 root dialout 166, 0 May 25 18:31 /dev/ttyACM0
The leading **c** means it's a character device.
root dialout shows that it's owned by root, and users in the dialout group can access it.
166, 0 are the major and minor device numbers.



