# FTP files between Mac and Windows using Pure-FTPd and FileZilla

## Install the Pure-FTPD server on the Mac side

### Alternative 1: Use Homebrew

brew install pure-ftpd

### Alternative 2: Manual

* Download latest .tar.gz from http://download.pureftpd.org/pub/pure-ftpd/releases/
* Untar and build it:

```
tar -xf pure-ftpd-1.0.42.tar.gz
cd pure-ftpd-1.0.42
./configure --with-pam
make
```

## Prepare the Pure-FTPD server on the Mac side

To get Pure-FTPd to authenticate against system users on OSX, you have to use PAM. (From Pure-FTPD README.MacOS-X)

Create a `/etc/pam.d/pure-ftpd` file (`sudo -e /etc/pam.d/pure-ftpd`):

    # pure-ftpd: auth account password session
    auth       required       pam_opendirectory.so
    account    required       pam_permit.so
    password   required       pam_deny.so
    session    required       pam_permit.so
    
Disable firewall from OS X System Preferences -> Security.

Start the FTP server:

    sudo src/pure-ftpd -lpam -B -A

I highly recommend `htop` for monitoring and killing the pure-ftpd process. It can be installed from Homebrew.

## Character encoding compatibility

Pure-FTPD has `--fscharset` and `--clientcharset` options, but you don't need to use them for UTF-8 with FileZilla. They would require that you compile with `./configure --with-rfc2640`. See `README` in Pure-FTPD documentation for details.

## Using FileZilla on Windows

I highly recommend using (Ninite)[https://ninite.com/] to install FileZilla (and other apps).

* Start FileZilla
* FileZilla main menu -> File -> Site Manager...
  * New Site button
    * General tab
      * Protocol -> FTP (i.e. not SFTP)
      * Logon Type -> Normal
    * Charset tab
      * Force UTF-8 -> Select this choice
  * Connect button
* FileZilla main menu -> Transfer
  * Default file exists action...
    * Downloads/Uploads: Overwrite file if size differs or source file is newer
    * Do *NOT* choose "Resume file transfer" here. Otherwise FileZilla will pause for several seconds every 10 files or so.
  * Transfer type -> Binary
  * Preserve timestamps of transferred files -> tick this box
* Then just transfer the files as usual!
