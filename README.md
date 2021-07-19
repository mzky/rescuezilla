The debug branch of the ARM64 platform!
编译成功后在国产主板上始终无法启动，使用统信和麒麟系统部署deb包后也无法启动
暂时换用再生龙arm版：http://free.nchc.org.tw/clonezilla-live/experimental/arm/2.6.6-11/ 
再生龙自动化配置：


\boot\grub\grub.cfg
```
set pref=/boot/grub
set default="0"
set color_normal=white/black
set color_highlight=cyan/black
loadfont $pref/unicode.pf2
 
menuentry "PCM Disk Tools for aarch64 -- Automatic Backup Disk" {
  search --set -f /live/vmlinuz
  set imagename="???"
  linux /live/vmlinuz boot=live union=overlay config components noeject quiet noswap edd=on nomodeset noprompt keyboard-layouts=NONE ocs_live_batch="yes" locales=zh_CN.UTF-8 vga=788 toram=filesystem.squashfs ip=frommedia nosplash ocs_prerun="mount `ls /dev/sdb?|tail -1` /mnt;mount –o remount,rw /mnt;mount --bind /mnt/home/partimag /home/partimag" ocs_live_extra_param="" ocs_live_run="ocs-sr -q2 -j2 -nogui -gs -z1p -i 4096 -sfsck -senc -p poweroff savedisk ${imagename} sda" 
  initrd /live/initrd.img
  ###########################################################################
  # Will ??? Change the folder name of the [Backup] image 
  # Press Ctrl + X to execute
  ###########################################################################
}

menuentry "PCM Disk Tools for aarch64 -- Automatic Recovery Disk"{
  search --set -f /live/vmlinuz
  set imagename="???"
  linux /live/vmlinuz boot=live union=overlay config components noeject quiet noswap edd=on nomodeset noprompt keyboard-layouts=NONE ocs_live_batch="yes" locales=zh_CN.UTF-8 vga=788 toram=filesystem.squashfs ip=frommedia nosplash ocs_prerun="mount `ls /dev/sdb?|tail -1` /mnt;mount –o remount,rw /mnt;mount --bind /mnt/home/partimag /home/partimag" ocs_live_run="ocs-live-restore" ocs_live_extra_param="-g auto -e1 auto -e2 -nogui -r -j2 -cs -p poweroff restoredisk ${imagename} sda"
  initrd /live/initrd.img
  ###########################################################################
  # Will ??? Change to the name of the mirrored folder required for [Restore]
  # Press Ctrl + X to execute
  ###########################################################################
}

menuentry "PCM Disk Tools for aarch64 -- Manual Operation"{
  search --set -f /live/vmlinuz
  linux /live/vmlinuz boot=live union=overlay config components quiet noswap edd=on nomodeset noprompt keyboard-layouts=NONE ocs_live_batch="no" locales=zh_CN.UTF-8 vga=788 ip=frommedia nosplash ocs_prerun="lsblk" ocs_prerun1="echo Press ctrl+c to switch to the command line" ocs_prerun2="sleep 6" ocs_live_run="ocs-live-general" ocs_live_extra_param=""
  initrd /live/initrd.img
  ###########################################################################
  # Manually backup or restore a disk or partition
  # Press Ctrl + X to execute
  ###########################################################################
}

menuentry "PCM Disk Tools for x86_64 --- Automatic Backup Disk" {
  search --set -f /casper/vmlinuz
  set imagename="???"
  linux /casper/vmlinuz boot=casper union=overlay config components noeject quiet noswap edd=on nomodeset noprompt keyboard-layouts=NONE ocs_live_batch="yes" locales=zh_CN.UTF-8 vga=788 toram=filesystem.squashfs ip=frommedia nosplash ocs_prerun="mount `ls /dev/sdb?|tail -1` /mnt;mount –o remount,rw /mnt;mount --bind /mnt/home/partimag /home/partimag" ocs_live_extra_param="" ocs_live_run="ocs-sr -q2 -j2 -nogui -gs -z1p -i 4096 -sfsck -senc -p poweroff savedisk ${imagename} sda" 
  initrd /casper/initrd.img
  ###########################################################################
  # Will ??? Change the folder name of the [Backup] image 
  # Press Ctrl + X to execute
  ###########################################################################
}

menuentry "PCM Disk Tools for x86_64 --- Automatic Recovery Disk"{
  search --set -f /casper/vmlinuz
  set imagename="???"
  linux /casper/vmlinuz boot=casper union=overlay config components noeject quiet noswap edd=on nomodeset noprompt keyboard-layouts=NONE ocs_live_batch="yes" locales=zh_CN.UTF-8 vga=788 toram=filesystem.squashfs ip=frommedia nosplash ocs_prerun="mount `ls /dev/sdb?|tail -1` /mnt;mount –o remount,rw /mnt;mount --bind /mnt/home/partimag /home/partimag" ocs_live_run="ocs-live-restore" ocs_live_extra_param="-g auto -e1 auto -e2 -nogui -r -j2 -cs -p poweroff restoredisk ${imagename} sda"
  initrd /casper/initrd.img
  ###########################################################################
  # Will ??? Change to the name of the mirrored folder required for [Restore]
  # Press Ctrl + X to execute
  ###########################################################################
}

menuentry "PCM Disk Tools for x86_64 --- Manual Operation"{
  search --set -f /casper/vmlinuz
  linux /casper/vmlinuz boot=casper union=overlay config components quiet noswap edd=on nomodeset noprompt keyboard-layouts=NONE ocs_live_batch="no" locales=zh_CN.UTF-8 vga=788 ip=frommedia nosplash ocs_prerun="lsblk" ocs_prerun1="echo Press ctrl+c to switch to the command line" ocs_prerun2="sleep 6" ocs_live_run="ocs-live-general" ocs_live_extra_param=""
  initrd /casper/initrd.img
  ###########################################################################
  # Manually backup or restore a disk or partition
  # Press Ctrl + X to execute
  ###########################################################################
}

menuentry "Local Disk Boot" --id local-disk {
  echo "Booting first local disk..."
  configfile /boot/grub/boot-local-efi.cfg
  echo "No uEFI boot loader was found!"
  sleep 15
}

menuentry "BIOS Setup" --id uefi-firmware {
  fwsetup
  echo "No uEFI boot loader was found!"
  sleep 15
}
```

==========================================


[![Rescuezilla banner](docs/images/banner.big.png)](https://rescuezilla.com) 

# Rescuezilla [![Build Status](https://api.travis-ci.com/rescuezilla/rescuezilla.svg?branch=master)](https://travis-ci.com/github/rescuezilla/rescuezilla) [![Translation status](https://hosted.weblate.org/widgets/rescuezilla/-/svg-badge.svg)](https://hosted.weblate.org/engage/rescuezilla/)

Rescuezilla is an easy-to-use disk cloning and imaging application that's fully compatible with Clonezilla — the industry-standard trusted by tens of millions.

Yes, Rescuezilla is the Clonezilla GUI (graphical user interface) that you might have been looking for.

Disk imaging is the process of making a backup of your computer's hard drive which is managed as files stored on an external hard drive, and 'disk cloning' is the process of making a direct copy without needing a third drive for temporary storage.

For many people, the alternative open-source tools such as Clonezilla are intimidating and difficult to use, so Rescuezilla provides an easy-to-use graphical environment like the leading commercial tools, Acronis True Image and Macrium Reflect.

It's worth noting that hard drive imaging and cloning is a very specialized task that's not necessarily the best solution for every user: it's worth researching whether a traditional file-based backup approach is more suitable for the specific problem you are looking to solve.

Rescuezilla can be booted on any PC or Mac from a USB stick, and has been carefully developed to be fully interoperable with the Clonezilla. This means Rescuezilla can restore backups created by Clonezilla, and backups created by Rescuezilla can be restored using Clonezilla!

Rescuezilla is an fork of _Redo Backup and Recovery_ (now called _Redo Rescue_) after it had been abandoned for 7 years.

## Features

* Simple graphical environment anyone can use
* Creates backup images that are fully interoperable with the industry-standard Clonezilla
* Supports images made by all known open-source imaging frontends, including Clonezilla (see 'compatibility' section of download page)
* Also supports virtual machine images: VirtualBox (VDI), VMWare (VMDK), Hyper-V (VHDx), Qemu (QCOW2), raw (.dd, .img) and many more
* Access files from within images (including virtual machine images) using 'Image Explorer (beta)'
* Fully supports advanced environments such as Linux md RAID, LVM and no partition table (filesystem directly-on-disk)
* Supports cloning (for direct 'device-to-device' mode without needing a third drive for temporary storage)
* Boots from Live USB stick on any PC or Mac
* Full system backup, bare metal recovery, partition editing, data protection, web browsing, and more
* Extra tools for hard drive partitioning, factory reset, undeleting files
* Web browser for downloading drivers, reading documentation
* File explorer for copying and editing files even if system won't boot
* Based on Ubuntu and partclone

Note: Rescuezilla does NOT yet _automatically_ shrink partitions to restore to disks _smaller_ than original. This feature will be added in future version.

## Supported Languages

Rescuezilla has been translated into the following languages:

* Dansk (da-DK) (Translation in-progress)
* Deutsch (de-DE)
* Ελληνικά (el-GR)
* English (en-US)
* Español (es-ES)
* Français (fr-FR)
* Italiano (it-IT)
* 日本語 (ja-JP)
* Norsk Bokmål (nb-NO) (Translation in-progress)
* Polski (pl-PL)
* Português brasileiro (pt-BR)
* Русский (ru-RU) (Translation in-progress)
* Svenska (sv-SE)
* Türkçe (tr-TR)
* 中文(简体) (zh-CN)
* 中文(繁體) (zh-Hant)

Rescuezilla uses Weblate for translation. **Please see [Translations HOWTO](https://github.com/rescuezilla/rescuezilla/wiki/Translations-HOWTO) to submit or update a translation.**

## Screenshots

<a href="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/2.png"><img src="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/2.png" alt="Easy point and click interface anyone can use"></a>
<a href="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/1.png"><img width=308 height=229 src="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/1.png" alt="Select video mode, check CD integrity, or test RAM at boot"></a>
<a href="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/4.png"><img width=308 height=229 src="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/4.png" alt="Save to hard drive, network shared folder, or FTP server"></a>
<a href="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/5.png"><img width=308 height=229 src="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/5.png" alt="Select partitions to backup"></a>
<a href="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/6.png"><img width=308 height=229 src="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/6.png" alt="Many powerful tools available from the start button"></a>
<a href="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/7.png"><img width=308 height=229 src="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/7.png"  alt="Provides browser access even if you can't log into your PC"></a>
<a href="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/3.png"><img width=308 height=229 src="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/3.png" alt="Backups up only the used space"></a>
<a href="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/8.png"><img width=308 height=229 src="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/8.png" alt="Provides many more tools including GParted Partition Editor, undelete and more!"></a>
<a href="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/9.png"><img width=308 height=229 src="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/9.png" alt="Easily mount Clonezilla images"></a>
<a href="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/10.png"><img src="https://raw.githubusercontent.com/rescuezilla/rescuezilla.github.io/master/media/screenshots/10.png" alt="Extract individual files from Clonezilla images"></a>

## History

Below table shows an abridged history of Rescuezilla. For more information, see the [CHANGELOG](https://raw.githubusercontent.com/rescuezilla/rescuezilla/master/CHANGELOG).

| Release             | Release Date | Operating System | Notes |
| ------------------- | ---------- | ---------------- | ---------------------------------- |
| Rescuezilla 2.2     | 2021-06-04 | Ubuntu 21.04     | Added 'Clone', VM image support. [Download page](https://github.com/rescuezilla/rescuezilla/releases/latest)
| Rescuezilla 2.1     | 2020-12-12 | Ubuntu 20.10     | Added Image Explorer (beta)
| Rescuezilla 2.0     | 2020-10-14 | Ubuntu 20.04     | Added Clonezilla image support
| Rescuezilla 1.0.6   | 2020-06-17 | Ubuntu 20.04     |
| Redo Rescue 2.0.0   | 2020-06-12 | Debian 9 Stretch | Redo author [resurfaces](https://sourceforge.net/p/redobackup/discussion/general/thread/d0e37c4750/) after 7.5 year absence
| Rescuezilla 1.0.5.1 | 2020-03-24 | Ubuntu 18.04.4   |
| Rescuezilla 1.0.5   | 2019-11-08 | Ubuntu 18.04.3   | Rescuezilla [fork](https://sourceforge.net/p/redobackup/discussion/general/thread/116063b485/?limit=25#610c) revives project
| Community updates   | Various    | Various          | Sporadic community [updates](https://github.com/rescuezilla/rescuezilla/wiki/Bugs-in-unofficial-Redo-Backup-updates#identifying-redo-backup-versions) for Redo
| Redo Backup 1.0.4   | 2012-11-20 | Ubuntu 12.04.1   | Last release by Redo author for 7.5 years
| Redo Backup 0.9.8   | 2011-03-10 | Ubuntu 10.10     | Redo author [deleted](https://sourceforge.net/p/redobackup/discussion/help/thread/4ea6ca31/) v0.9.2-v0.9.7
| Redo Backup 0.9.2   | 2010-06-24 | xPUD             |

## Reviews and testimonials

Please consider posting a review of Rescuezilla on the very useful website [AlternativeTo.Net](https://alternativeto.net/software/rescuezilla/reviews/). Consider giving the project a like too! :-)

## Building 

See [Building ISO image](docs/build_instructions/BUILD.ISO.IMAGE.md)

## Future development

Rescuezilla features are prioritized according to the [roadmap](https://github.com/rescuezilla/rescuezilla/wiki/Rescuezilla-Project-Roadmap). Please consider becoming a [Patreon to help fund Rescuezilla's continued development](https://www.patreon.com/join/rescuezilla)!

## Support

If you need help, start by checking the [frequently asked questions](https://rescuezilla.com/help.html), then proceed to the [Rescuezilla forum](https://sourceforge.net/p/rescuezilla/discussion/).

## Downloads

[Download the latest Rescuezilla ISO image on the GitHub Release page](https://github.com/rescuezilla/rescuezilla/releases/latest)
