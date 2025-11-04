🧱 Linux Disk Utilities Cheat Sheet
📦 df – Disk Free
Displays disk space usage.
Common Flags
Flag	Description	Example
-h	Human-readable sizes (KB, MB, GB)	df -h
-T	Show filesystem type	df -T
-a	Show all filesystems (including pseudo or duplicate)	df -a
-i	Show inode usage instead of block usage	df -i
--total	Display a total line at the end	df -h --total
🪛 parted – Partition Editor
Modern command-line tool for managing partitions.
Syntax:
parted [DEVICE] [COMMAND]
Useful Flags
Flag	Description	Example
-l	List devices and partitions	parted -l
-s	Script mode (no user prompts)	parted -s /dev/sda print
-a	Set alignment mode (e.g. optimal)	parted -a optimal /dev/sda mkpart primary 1MiB 50%
Inside parted, you use commands like print, mkpart, rm, etc.
🧩 fdisk – Partition Table Manipulator (MBR)
Classic tool for working with MBR disks (can view GPT).
Common Flags
Flag	Description	Example
-l	List partition tables	fdisk -l
-u	Use sectors instead of cylinders	fdisk -u /dev/sda
-t	Specify partition table type (e.g. gpt)	fdisk -t gpt /dev/sda
-s	Show size of partition in blocks	fdisk -s /dev/sda1
Interactive Commands in fdisk:
p – Print partition table
n – Add new partition
d – Delete partition
w – Write changes to disk and exit
q – Quit without saving changes
⚙️ gdisk – GPT fdisk
A GPT-specific version of fdisk.
Common Flags
Flag	Description	Example
-l	List partitions	gdisk -l /dev/sda
-v	Verify partition table	gdisk -v /dev/sda
-e	Move GPT data structures to end of disk	gdisk -e /dev/sda
-?	Show help	gdisk -?
Interactive Commands in gdisk:
p – Print partition table
n – Create new partition
d – Delete partition
w – Write changes and exit
q – Quit without saving changes
