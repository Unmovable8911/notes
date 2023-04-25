# I. Install Kali Linux Live boot USB
## 1. Download Kali Live `iso`
Download the Kali Live `iso` from [Kali Linux official website](https://www.kali.org/get-kali/)
## 2. Image the `iso` to your USB drive
1. Navigate to the directory where the *Kali Linux Live Image iso* is downloaded  
2. Use `lsblk` command to find out the device file name of the USB drive. For example, `/dev/sda`
3. Use `dd` command to image the `iso` to your USB drive
```
# dd if=kali-linux-2023.1-live-amd64.iso of=/dev/sda conv=fsync bs=4M status=progress
```
# II. Adding encrypted persistence
## 1. Creating a new partition for *persistence*
1. Find out the device file of the USB drive using `lsblk`. For illastration, we'll asume that it is `/dev/sda`
2. Type in this command `fdisk /dev/sda`. This will bring you to the `fdisk` prompt.
3. Type `n` and hit `Enter` for creating a new partition.
4. Choose between *primary* and *extended*. The *persistence* should be a primary patition, so type in `p` and hit `Enter`.
5. Specify the partition number, for here, just use the default, which should be `3`. Hit `Enter`.
6. Determine the *starting sector* for the new partition, again, leave it with default and hit `Enter`.
7. Specify the *ending sector* for the new partition. Generally, this is where we decide how much space we want to give to *persistence*. Say, we want 4GB, so type in `+4G` and hit `Enter`.
8. Type `w` to save the changes.  
And now the new partition is created, the next step is to add a LUKS encryption for this partition.
## 2. Encrypting patition
```
# cryptsetup --verbose --verify-passphrase luksFormat /dev/sda3
```  
This command will return a prompt for pass-phrase, type in your pass-phrase, and hit enter, and type in your pass-phrase again to verify it.  
Now, open the encrypted partition:  
```
# cryptsetup luksOpen /dev/sda3 my_usb
```  
`my_usb` is the name you give to the partition which you will refer to later, substitute it with anything you prefer.
**If you ever wanted to change your pass-phrase, you can do it by using `cryptsetup luksChangeKey /dev/sda3`.**
## 3. Creating `Ext4` filesystem
Create a `Ext4` filesystem and label it as persistence
```
# mkfs.ext4 -L persistence /dev/mapper/my_usb
# e2label /dev/mapper/my_usb persistence
```
## 4. Mount the partition and create `persistence.conf`
```
# mkdir /mnt/my_usb
# mount /dev/mapper/my_usb /mnt/my_usb
# echo "/ union" | tee /mnt/my_usb/persistence.conf
# umount /mnt/mapper/my_usb
```
## 5. Close the encrypted parition
```
# cryptsetup luksClose /dev/mapper/my_usb
```
**Now your USB drive is ready to plug in and reboot into Live USB Encrypted Persistence mode.**
