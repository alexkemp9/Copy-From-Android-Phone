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
All of the Apps used are standard for a Linux desktop system, with the exception of *JMTPFS* (and possibly *FUSERMOUNT*), although it can be installed from standard Repositories:

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
MTP may well need to be explicitly switched ON in your phone. For me (ancient Android 6 and then — when that model died suddenly in June 23 — in Android 12), that was done by:–

1. The `Settings > Developer options` menu-option is hidden until you touch `Build number` in the `About phone` menu 7 times (no kidding).
2. Open `Developer options`    
(Android 12: found bottom of `Settings | System`)
3. (if necessary) Switch ON at the top
4. Scroll down to `Select USB Configuration`    
(select MTP over PTP or other setting)    
(Android 12: `Default USB Configuration | File Transfer`)

***FUSERMOUNT***     
I'm uncertain precisely how to install *FUSERMOUNT*, because I installed it so many years ago & now can neither remember nor locate. It looks like it may have been auto-installed when I installed something else, with `libfuse2` the most likely culprit (check out also [GitHub libfuse](https://github.com/libfuse/libfuse)):

```bash
$ apt search libfuse2
Sorting... Done
Full Text Search... Done
libfuse2/stable,now 2.9.9-5 amd64 [installed,automatic]
  Filesystem in Userspace (library)

$ /bin/fusermount -V
fusermount version: 2.9.9
```

## *Usage*
Once setup, usage is dead easy:

1. Unlock the phone & connect via a USB cable to your desktop computer    
(at this stage some phones ask to choose protocol, with *MTP* as the required option)
2. Launch `copyFromPhone`:

```bash
$ ~/.local/sbin/copyFromPhone
!! REMEMBER TO UNLOCK SCREEN !!
!! Unmount phone if necessary !!
Mount phone at '/media/user/disk'; copy files at 'phone:Camera' to '/home/user/Pictures/Android'. Continue? (y/n): 
```

The phone device should now appear within a File Manager (mine is *thunar*) but if auto-mounted needs to be *unmounted* before pressing *‘y’*.

3. Press *‘y’*    
(any new files will auto-transfer via rsync):

```bash
Mount phone at '/media/alexk/disk'; copy files at 'phone:Camera' to '/home/alexk/Pictures/Android'. Continue? (y/n): y
Script continues…
Device 0 (VID=19d2 and PID=0307) is a ZTE V880E.
Android device detected, assigning default bug flags
IMG_20230525_160030.jpg  VID_20230525_161106.mp4
sending incremental file list
created directory /home/user/Pictures/Android/DCIM
Camera/
Camera/IMG_20230525_160030.jpg
        763,896 100%   99.61MB/s    0:00:00 (xfr#1, to-chk=1/3)
Camera/VID_20230525_161106.mp4
    142,370,707 100%  159.32MB/s    0:00:00 (xfr#2, to-chk=0/3)
sending incremental file list
Records/
2019-04-17_10-17-18  data
sending incremental file list
osmtracker/
osmtracker/2019-04-17_10-17-18/
osmtracker/2019-04-17_10-17-18/2019-04-17_10-17-18.gpx
      2,275,800 100%   89.13MB/s    0:00:00 (xfr#1, to-chk=93/97)
osmtracker/2019-04-17_10-17-18/2019-04-17_10-18-55.3gpp
         36,726 100%   51.38kB/s    0:00:00 (xfr#2, to-chk=92/97)
osmtracker/2019-04-17_10-17-18/2019-04-17_10-20-33.3gpp
…
osmtracker/2019-04-17_10-17-18/2019-04-17_14-02-00.3gpp
         19,194 100%   33.12kB/s    0:00:00 (xfr#78, to-chk=16/97)
osmtracker/data/
osmtracker/data/files/
osmtracker/data/files/track7/
osmtracker/data/files/track7/2021-08-01_08-46-06.3gpp
         21,246 100%   21.93kB/s    0:00:00 (xfr#79, to-chk=13/97)
osmtracker/data/files/track7/2021-08-01_08-46-19.jpg
      2,342,566 100%    1.84MB/s    0:00:01 (xfr#80, to-chk=12/97)
osmtracker/data/files/track7/2021-08-01_08-47-16.jpg
      2,473,874 100%    7.02MB/s    0:00:00 (xfr#81, to-chk=11/97)
osmtracker/data/files/track7/2021-08-01_08-49-19.3gpp
         31,290 100%   61.11kB/s    0:00:00 (xfr#82, to-chk=10/97)
osmtracker/data/files/track7/2021-08-01_08-49-53.jpg
      1,818,791 100%    2.25MB/s    0:00:00 (xfr#83, to-chk=9/97)
osmtracker/data/files/track7/2021-08-01_08-49-55.3gpp
         34,926 100%   36.25kB/s    0:00:00 (xfr#84, to-chk=8/97)
osmtracker/data/files/track7/2021-08-01_08-50-25.jpg
      2,425,962 100%    1.87MB/s    0:00:01 (xfr#85, to-chk=7/97)
osmtracker/data/files/track7/2021-08-01_08-50-46.3gpp
         30,138 100%  151.71kB/s    0:00:00 (xfr#86, to-chk=6/97)
osmtracker/data/files/track7/2021-08-01_08-51-13.jpg
      2,638,148 100%    5.07MB/s    0:00:00 (xfr#87, to-chk=5/97)
osmtracker/data/files/track7/2021-08-01_08-52-05.3gpp
         30,246 100%   40.30kB/s    0:00:00 (xfr#88, to-chk=4/97)
osmtracker/data/files/track7/2021-08-01_08-52-23.jpg
      2,361,978 100%    2.11MB/s    0:00:01 (xfr#89, to-chk=3/97)
osmtracker/data/files/track7/2021-08-01_08-53-32.3gpp
         29,958 100%  152.37kB/s    0:00:00 (xfr#90, to-chk=2/97)
osmtracker/data/files/track7/2021-08-01_08-54-28.3gpp
         35,610 100%  102.58kB/s    0:00:00 (xfr#91, to-chk=1/97)
osmtracker/data/files/track7/2021-08-01_08-55-10.jpg
      2,099,899 100%    3.15MB/s    0:00:00 (xfr#92, to-chk=0/97)
```

4. (the phone is unmounted by the script after transfers complete, so the phone can now be detached)

## *Problems*
Life has been easy with this script until I got an Android 12 phone. It’s tendency is to switch off services that are not regularly used. That is a problem for an old codger like me that does not use the phone much. This hit me recently as I tried to copy a picture of my water meter to the computer so that I could send it to *Severn Trent* (use email on my phone? no thanks). I'm not fully certain that all of what is below was necessary to fix the issue, but here it is anyway.

*Symptom:* An error occured whilst attempting to mount the phone (after pressing *‘y’* to *“Continue”*) (the first comes from the script, whilst the other comes from JMTPFS):

```bash
Error whilst mounting the phone/camera
No mtp devices found
```

*Basics:*    
- The phone must NOT be mounted before running the script    
(that error is spotted in the script & how to unmount using FUSERMOUNT is suggested)
- The phone screen must be unlocked    
(the normal main reason for errors)
- The phone must be recognised by the computer when USB is plugged in

*Android 12 Checklist:*
Run the following immediately after plugging USB from the phone into the computer:

```bash
$ sudo dmesg | tail
[673466.819292] usb 3-1.3: new high-speed USB device number 7 using ehci-pci
[673466.929404] usb 3-1.3: New USB device found, idVendor=1782, idProduct=4003, bcdDevice= 4.04
[673466.929422] usb 3-1.3: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[673466.929430] usb 3-1.3: Product: A52
[673466.929435] usb 3-1.3: Manufacturer: Blackview
[673466.929440] usb 3-1.3: SerialNumber: A52EEA0000035524
```
If you do not see something like the above, then try the steps below on your phone:

In `System | Developer options`:–    
- USB debugging :: ON
- Disable adb authorisation timeout :: OFF
- Verify apps over USB :: OFF
- Default USB configuration :: File transfer
- Reset notification importance
- Standby apps :: Files :: ACTIVE

Before the above I discovered that the phone was NOT discovered by the system (dmesg) after it's usb was plugged into the computer. After the above (many changes) removing & re-plugging the USB caused the phone to immediately be recognised, and the script operated normally.

HTH
