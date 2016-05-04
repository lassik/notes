# How to make a bootable DBAN (Darik's Boot and Nuke) USB stick on Mac OS X

## 1. Download the official DBAN ISO image (i.e. CD image)

From <http://www.dban.org/download>. The file name is like
`dban-2.3.0_i586.iso` (the version number may be different).

## 2. Download and install Unetbootin for Mac

Unetbootin is an application that makes USB sticks from images meant
for CDs (ISO images).

You can get it from [Homebrew Cask](https://caskroom.github.io/)

    brew update
    brew cask install unetbootin

Or by downloading manually from <https://unetbootin.github.io/>

## 3. Insert a USB stick and format it as MS-DOS FAT ##

NOTE: This will destroy all data on the USB stick so make sure you
have backups.

1. Go into Disk Utility
2. Select the USB drive from the list on the left (the drive itself,
   not any partition under the drive)
3. Go into the Partition tab on the right.
4. Choose a single partition (i.e. only one partition that fills the
   entire disk).
5. Make sure the partition's file system type is MS-DOS (FAT). NOT
   HFS/Apple.
6. Press go to erase the disk.
7. Eject the USB stick and then plug it back in.

## 4. Run Unetbootin

Homebrew Cask should have installed it into either `/Applications` or
`~/Applications`. Either way, you should be able to launch it by
typing `unetbootin` into Spotlight and pressing Enter.

It will ask for your administrator password at the beginning.

From the Unetbootin main window:

1. Make sure *Type: USB drive* is selected.
2. From *Drive:* select your USB stick. If the *Drive:* selection box
   is empty with no choices, it means the USB stick hasn't been
   formatted as FAT (MS-DOS) properly. Unetbootin doesn't recognize
   USB sticks that aren't FAT-formatted.
3. Select *Diskimage: ISO* and click the browse button (three dots) to
   the right. Find the `dban-2.3.0_i586.iso` file you downloaded
   (version number may differ) and click OK.
4. Back in the main window, click OK. Unetbootin will write DBAN and a
   boot loader to the USB stick.

## 5. Fix up the boot loader configuration made by Unetbootin

(Suggested by *zero4281* at
<http://ubuntuforums.org/archive/index.php/t-1701757.html>)

Edit the file `syslinux.cfg` on the USB stick using a text editor (you
should be able to find the file in the Mac Finder or on the command
line under `/Volumes`).

1. Replace every occurrence of the word `ubninit` with the text `ISOLINUX.BIN`
2. Replace every occurrence of the word `ubnkern` with the text `DBAN.BZI`

(If you don't make these edits, you can still boot from the USB stick
and get as far as the boot loader menu, but you won't be able to
actually run any of the choices on the menu. You'll just get the error
message `Could not find ramdisk image:/ubninit` when you try to select
any of the choices.)

## 5. Eject the USB stick, plug it into the computer where you want to run DBAN, and boot
