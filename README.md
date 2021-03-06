hdl_dump and hdl_dumb README
=================================


TOC
---
Intro
Networking server
Compilation 
Configuration and list file location
Configuration
New features


Intro
-----

Latest version, as well as some documentation is to be found on the new official
repository -- https://github.com/AKuHAK/hdl-dump/
Me reads w1zard0f07@yahoo.com and psx-scene forums every now and then.
Easy guide for installing games can be found here -- 
http://web.archive.org/web/20120720230755/http://openps2loader.info/hdldump/howto.html


Networking server
-----------------

Purely UDP networking server is available, based on recently released on
psx-scene forums smap driver --
http://ichiba.geocities.jp/ysai187/PS2/smap.htm
It is called hdl_svr_093.elf and is a part of hdl_dump sources.

You might need to punch a hole in your firewall for incoming UDP from port 12345.

Compilation
-----------
Firts of all you have to update your PS2SDK.
Next: you have to run shell script "diskload.sh" for getting latest mini-opl version.
Finally: You just can run shell script in the project folder: "mkrel.sh". It will
compile both gui and normal version for windows.

* Linux: Build and copy executable into a directory of your choice.
	make RELEASE=yes
Advanced Linux build commands:
	make XC=win          # for Windows cross-compilation using mingw32
	make -C gui          # for WineLib compilation (for now doesn't work)
	make -C gui XC=win   # for GUI cross-compilation using mingw32

* FreeBSD and MacOS X: You'll need to have GNU make installed, then
	gmake RELEASE=yes IIN_OPTICAL_MMAP=no
or
	make RELEASE=yes IIN_OPTICAL_MMAP=no

* Windows: You need to have CYGWIN (http://www.cygwin.com/) installed;
then use
	make RELEASE=yes
	make -C gui RELEASE=yes
to compile command-line or GUI version.
You can use this guide for preparing sdk for windows:
http://psx-scene.com/forums/f150/compiling-windows-118947/#post1124987



Configuration and list file location
------------------------------------

You can place this files in folder where is installed hdl_dump for making it portable.

* Windows: (cited names are for English version of Windows)
C:\Documents and Settings\<login name>\Application Data\hdl_dump.conf
C:\Documents and Settings\<login name>\Application Data\hdl_dump.list

* Linux, FreeBSD and MacOS X: (~ is your home dir)
~/.hdl_dump.conf
~/.hdl_dump.list


Configuration
-------------

* disc_database_file -- full path to your disc compatibility database file;


New features
------------

All new stuffs can be used from HDD_OSD or BB Navigator or XMB from PSX DVR.

inject_dvd, inject_cd, install or copy_hdd.

WARNING: BUG: You need to specify dma mode or hdl_dump crashes (*u4, *m2)

You can place boot.elf (which is actually miniopl.elf) in the folder
where is launched from hdl_dump. It is optional you can skip this file.
You also can place list.ico (*.ico from memory card)
and icon.sys (*.sys from memory card).
Or you can write your own icon.sys file.
You can use both files or skip any of them.
If you skip *.ico will be used HDloader default icon.
If you skip *.sys will be used HDloader default icon settings.

This command will install game onto hard disk. Be careful if you choose
command copy_hdd - every HDL game will loose their icons and will use only
one for all.
boot.elf - you can use everything but I prefer miniopl. Miniopl allows you
to launch titles from HDD OSD or BB NAV by pressing on title.
boot.elf injection address - 0x111000
boot.elf size limit - 2 026 464 bytes (thanks to kHn)
--------------------------------------------------------------------------
initialize

When you use this command and place hard drive into your playstation 2 phat
and power it on - it will launch mbr. So it can be used as Free MC Boot
replacement (no need for memory card or modchip for lauching homebrews).

In previous version this command tried to install one __mbr partition - but
there was a bug in the code so this command won't work correctly. I decide to
remove it at all and replace with MBR injection. All that you need - place
MBR.KELF in the corresponding folder. All HDD data will remain intact!!!!!

MBR.KELF injection address - 0x404000 (for compatibility with BB Nav)
MBR.KELF doesn't have any restrictions by themself.
There is no easy way to make MBR.KELFS from common elf.
------------------------------------------------------------------------------
modify_header

Code:
hdl_dump.exe modify_header 192.168.0.10 PP.TEST

This command will inject header attributes in any already existing partition.
It can operate such files:
system.cnf
icon.sys
list.ico
del.ico
boot.kelf
boot.elf
boot.kirx
Any file can be skipped. Program first of all tries to inject boot.kelf. 
If it is not found it will try to inject boot.elf.

Now icon.sys can be in any of 2 formats: Memory Card format or HDD format.

del.ico injection address: 0x041000
If del.ico is used list.ico maximum size 260 096bytes.
boot.elf (or boot.kelf) injection address - 0x111000
boot.elf size limit - 2 026 464 bytes (thanks to kHn)
boot.kelf size limit - 3 076 096 bytes (if boot.kirx not used)
boot.kelf size limit - 2 031 616 bytes (if boot.kirx is used)
boot.kirx injection address - 0x301000
boot.kirx size limit - 1 044 480 bytes
------------------------------------------------------------------------------------
If you want to know more about these files (and their restrictions) you have to
study official ps2sdk document called

hdd_rule_3.0.2.pdf

There is also some undocumented features like this:
- If you want to inject boot.kelf (or boot.elf) you have to change BOOT2 in system.cnf 
Code:
|-----------------|
| BOOT2 = PATINFO |
|-----------------|
This is used for HDL games for example.
If you need to erase boot.elf from PATINFO you have to place zero-sized boot.kelf or elf
in program folder.

-If you want to launch KELF from PFS partition you have to change BOOT2 in system.cnf 
Code:
|---------------------------|
| BOOT2 = pfs:/EXECUTE.KELF |
|---------------------------|
where EXECUTE.KELF - is path to KELF which is placed into partition. Can be changed.

-If you want to inject kirx into partition you have to add a line into system.cnf 
Code:
|-----------------|
| IOPRP = PATINFO |
|-----------------|
Don't ask me about kirx - I don't know where it can be used.

-If you don't want to boot from HDD OSD you have to add such a line into system.cnf 
|----------------|
| BOOT2 = NOBOOT |
|----------------|

Happy gaming.

$Id: README,v 2.0 2015-12-12 20:13:14 AKuHAK Exp $
