# HardwareLab
<p>ARM Assembly, C, C++, reverse engineering, exploit development and debugging</p>
<p>This lab is catered towards learning ARM assembly with intent to understand reverse engineering and exploit development.</p>

## Manual Setup
### Using Virtual Machine
This setup involves emulating a raspberry pi on QEMU with our preferred ARM version. 
You'll need the following:

#### Requirements
<!-- OL -->

1. Download the latest Ubuntu image and setup an Ubuntu VM. <!-- Links -->
[https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop)
1. Download Raspbian Image <!-- Links -->
[http://downloads.raspberrypi.org/raspbian/images/raspbian-2017-04-10/](http://downloads.raspberrypi.org/raspbian/images/raspbian-2017-04-10/)  and latest Qemu Kernel <!-- Links -->
[https://github.com/dhruvvyas90/qemu-rpi-kernel](https://github.com/dhruvvyas90/qemu-rpi-kernel)

#### Verify Download
<p>During this process make sure the files are in the same directory for gpg and verify against the original on the network that they match completely.</p>

<p>Verifying with sha256
<code>
sudo cat 2017-04-10-raspbian-jessie.zip.sha256
</code></p>

<p> Verifying with gpg 
<code>
gpg --verify 2017-04-10-raspbian-jessie.zip.sig
</code></p>

<p>Comparing 
<code>
sudo cat 2017-04-10-raspbian-jessie.zip
</code></p> 
</br>

#### Inside the Ubuntu VM
<p> Install the emulator  <code> sudo apt-get install qemu-system </code></p>
<p>Create a new folder<code>$ mkdir ~/qemu_vms/</code></p>
<p>Place the Raspbian image and qemu-kernel to<code>qemu_vms </code> folder</p>
<p>Unzip image file <code>unzip [image-file].zip</code></p>
<p>Configure drive <code>fdisk -l [image-file]</code> you'll see something like the one below</p>

```
Disk 2017-03-02-raspbian-jessie.img: 4.1 GiB, 4393533440 bytes, 8581120 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x432b3940

Device                          Boot  Start     End Sectors Size Id Type
2017-03-02-raspbian-jessie.img1        8192  92159  83968  41M  c W95 FAT32 (LBA)
2017-03-02-raspbian-jessie.img2      92160 8369151 8276992   4G 83 Linux
```

<p>Multiply the value of filesystem (.img2) that start at <code>137216</code> by <code>512</code>. Use the resulting <code>47185920</code> value as an offset in the following command:

```
sudo mkdir /mnt/raspbian

sudo mount -v -o offset=47185920 -t ext4 ~/qemu_vms/<your-img-file.img> /mnt/raspbian
```

<p>Once you have made sure that it mounts, we'll unmount and emulate it on Qemu with the following command:</p>
<p>unmount <code>sudo umount /mnt/raspbian</code></p>

<p>Emulating Raspbian image on Qemu</p>

```
qemu-system-arm -kernel ~/qemu_vms/<your-kernel-qemu> -cpu arm1176 -m 256 -M versatilepb -serial stdio -append "root=/dev/sda2 rootfstype=ext4 rw" -hda ~/qemu_vms/<your-jessie-image.img> -net user,hostfwd=tcp::5022-:22 -no-reboot

```

<p>The Qemu will launch a GUI of the Raspbian OS as shown below</p>

  
![RoQ](https://user-images.githubusercontent.com/76940116/140804233-5bff2435-f9df-4697-9e43-6e59221d839e.PNG)


### Using Raspberry Pi
In this method we'll use Raspberry Pi Imager
#### Requirements
<!-- OL -->

1. Raspberry Pi
1. SD card and SD card reader
1. USB cable for powering
#### Setup and configuration
Format SD card and check with disk management to make sure all of our partitions are deleted. Download [https://www.raspberrypi.com/software/](https://www.raspberrypi.com/software/) and run Raspberry Pi Imager, choose the recommended Raspberry Pi OS and choose our SD card as the Storage. Before we hit Write, press <code>Ctrl+shift+X</code> to configure our WiFi then hit Write. After the process us complete insert the SD card into the Raspberry Pi and power on.





