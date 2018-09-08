How to install Android on CHUWI Hi13
====================================
This repo is solution that installed android on Hi13. <br>
I want to install only android on Hi13 but many posts say "How to install android with Ubuntu". <br><br>
Why Ubuntu? I don't want to other linux. <br>
So, I find solution without no install ubuntu or other linux and share everyone. <br><br>
Thanks to Mr. Lee, American Megatrends Inc. and Joost van der Wulp <br>

I'm Seunghyun "Philip" Baek, Master Student at Kunsan National Univ. in RoK.

## Partitioning
I talk to you 2 solutions.

### First Solution: How to reuse an existing partition.

mmcblk1p1 is default bootloader partition in hi13 F/W.

```
mmcblk1p1 => EFI System
mmcblk1p2 => Windows RE
mmcblk1p3 => Windows
....
```

When install android, you have to delete two partitions and create new on.
```
Delete: mmcblk1p2, mmcblk1p3. not mmcblk1p1
Create: maybe new partition is mmcblk1p2 or 1p3. volume label is that you want name.
```

### Second Solution: How to clear all partition and install android.
if all partition clear and install android, you make fat32 partition first.
```
Delete: all
Create: mmcblk1p1 => 100MB(Recommand)
        mmcblk1p2 => All
```
And follow below guideline.

## Install

### First, Install Android and boot Ubuntu Live.
It's same this [post](http://chuwi-hi13-install-ubuntu.blogspot.com/2017/06/how-to-install-android-on-chuwi-hi13.html)

### Second, Type below cmd
```
sudo mount /dev/mmcblk1p1 /mnt
cd /mnt
cp -R This-repository-files .
```
Finished to install rEFInd!

### Configure: refind.conf
```
nano refind.conf
```
Go to EOF, modify volume
```
menuentry "Android 7.1" {
    volume "your volume label name"
    ...
}

```

What? you don't remember the name?
Don't worry.
you find your android label.
```
e2label /dev/mmcblk1p1
```
Do you want to change? yes, you can.
```
e2label /dev/mmcblk1p1 new-name
```

## Finally
Go to your Android. Come on!

## Important!
My Hi13 has Goodix touch:gdix1001 so I didn't test gdix1002(I think serial number after 17070001, 17070001 included) <br>
If your Hi13 is gdix1002, This [thread](https://techtablets.com/forum/topic/updating-bios-and-installing-linux/) may be helpful.
