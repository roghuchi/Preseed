# Preseed
Auto Debian installation with Preseed


## Disk partition format

When a partition is introduced, 3 numbers will be considered for it. minimum, priority and maximum!
From the left,minimum, priority and maximum number.

Priority is used to prioritize when we have allocated minimums to our partitions and still have space that is not allocated to any partition. In fact, we consider the weight for each partition with priority to determine its priority.

To calculate the weight of each partition, we consider two things.
If the priority number of the partition is less than or equal to the minimum number, the weight of this partition is considered 0.
Otherwise, the absolute value of the difference between priority and minimum weight will be related to our partition.

If I consider the above example:


| weight  | minimum | priority | maximum | name |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| 0  | 8192  | 8241  | 16384  | swap  |
| 2  | 16384  | 16386  | -1  | root  |
| 0  | 8192  | 8241  | 16384  | var  |


After determining the weight of each partition, the minimum value of each partition is assigned to it based on priority. Here, each of the swap and var partitions is assigned a value of 8192M, and then a value of 16384M will be assigned to the root partition.

After assigning a minimum value to each partition, if our hard drive space is still usable and empty, a number (percentage) will be considered for each partition based on the priority of each partition. For example, here we have values ​​that are 49%, 2% and 49%. Therefore, with this division in the next step, it allocates up to the maximum space required by the partition to that space. After our swap and var are maximized, the rest of the remaining space will be allocated to root because its maximum is -1.
In the prepared preseed, the priorities are as follows:

| weight  | minimum | priority | maximum | name |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| 0  | 1  | 1  | 1  | biosgrub  |
| 0  | 256  | 256  | 256  | efi  |
| 0  | 500  | 500  | 500  | boot  |
| 0  | 2048  | 2048  | 2048  | swap  |
| 96  | 4000  | 4096  | 4096  | var  |
| 1  | 1000  | 1000  | -1  | root  |


## Make ISO

```

mkdir isofiles
bsdtar -C isofiles -xf debian-10.8.0-i386-netinst.iso
xorriso -osirrox on -indev debian-10.8.0-i386-netinst.iso -extract / isofiles
7z x -oisofiles debian-10.8.0-i386-netinst.iso

chmod +w -R isofiles/install.386/
gunzip isofiles/install.386/initrd.gz
echo preseed.cfg | cpio -H newc -o -A -F isofiles/install.386/initrd
gzip isofiles/install.386/initrd
chmod -w -R isofiles/install.386/

```
