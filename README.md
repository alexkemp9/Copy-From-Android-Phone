# Copy-From-Android-Phone
A BASH Script to Copy Files from an Android Mobile to a Desktop

## *Basics*
This script file is designed for use by any-user (no root access required) within a Terminal. It’s principal purpose is to make it easy & reliable to mount & then copy all files within specific dirs in the phone across to a desktop dir. It has been used extensively by myself across many years to transfer all photos & speech files stored using *OSMTracker* whilst mapping for [OpenStreetMap](https://www.openstreetmap.org/user/alexkemp/diary) (using an ancient Android-6 phone) to disc under first *Debian* & then *Devuan 4* (a systemD-free version of *Debian*). In addition to BASH, the main utilities required are FUSERMOUNT, JMTPFS, MOUNTPOINT & RSYNC. Note that zero efforts are made to test for the presence of any of these Apps before use, so check that their location is correct at the top of the script.

## *Begin*
The following will assume that you have created a dir `~/.local/sbin` within which you store the bash-file; this is to set the script as executable for current user only:

```bash
$ chmod 0700 ~/.local/sbin/copyFromPhone
$ la ~/.local/sbin/copyFromPhone
-rwx------ 1 user user 2287 Jul 12  2021 /home/user/.local/sbin/copyFromPhone
```
## *JMTPFS, MTP & FUSERMOUNT*
All of the Apps used are standard for a Linux desktop system, with the exception of *JMTPFS* (and possibly FUSERMOUNT), although it can be installed from standard Repositories:

```bash
$ apt search jmtpfs
Sorting... Done
Full Text Search... Done
jmtpfs/stable,now 0.5-3 amd64 [installed]
  FUSE based filesystem for accessing MTP devices
```
If you look at [Github JMTPFS](https://github.com/JasonFerrara/jmtpfs) it is described as:

> jmtpfs is a FUSE and libmtp based filesystem for accessing MTP (Media Transfer
> Protocol) devices. It was specifically designed for exchanging files between Linux
> (and Mac OS X) systems and newer Android devices that support MTP but not USB Mass Storage.

The point is that early Android phones could be directly mounted via their USB port as *Mass Storage*; that gave Linux direct access to the system and allowed (as one example) easy undo of deleted files. Modern phones  can still be mounted via the USB port, but only via *MTP*, which essentially is connecting via a modem.

***MTP***     
MTP may well need to be explicitly switched ON in your phone. For me (ancient Android 6), that was done by:–

1. The `Settings > Developer options` menu-option is hidden until you touch `Build number` in the `About phone` menu 7 times (no kidding).
2. Open `Developer options
3. (if necessary) Switch ON at the top
4. Scroll down to `Select USB Configuration`
(select MTP over PTP or other setting)

