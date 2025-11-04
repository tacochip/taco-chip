🧱 Linux Disk Utilities Cheat Sheet
📦 df – Disk Free
Displays disk space usage.
Common Flags
Flag	Description	Example
-h	Human-readable sizes (KB, MB, GB)	df -h
-T	Show filesystem type	df -T
-a	Show all filesystems (including pseudo, duplicate, or inaccessible)	df -a
-i	Show inode information instead of block usage	df -i
--total	Show a total row at end of output	df -h --total
🪛 parted – Partition Editor
Modern command-line tool for managing partition tables.
parted [DEVICE] [COMMAND]
Useful Flags
Flag	Description	Example
-l	List all block devices and partitions	parted -l
-s	Script mode (no prompts)	parted -s /dev/sda print
-a	Set alignment mode (e.g. optimal)	parted -a optimal /dev/sda mkpart primary ext4 1MiB 100%
Tip: Once inside the parted shell, use commands like print, mkpart, rm, etc.
🧩 fdisk – Partition Table Manipulator (MBR)
Classic partitioning tool, mainly for MBR (but can list GPT with warnings).
Common Flags
Flag	Description	Example
-l	List partition tables	fdisk -l
-u	Use sectors instead of cylinders	fdisk -u /dev/sda
-t	Specify partition table type	fdisk -t gpt /dev/sda
-s	Print partition size in blocks	fdisk -s /dev/sda1
Interactive Commands (inside fdisk):
p – Print partition table
n – Create new partition
d – Delete partition
w – Write changes and exit
q – Quit without saving
⚙️ gdisk – GPT fdisk
Similar to fdisk, but for GPT partition tables.
Common Flags
Flag	Description	Example
-l	List partition table	gdisk -l /dev/sda
-v	Verify disk	gdisk -v /dev/sda
-e	Move GPT data structures to end of disk	gdisk -e /dev/sda
-?	Display help	gdisk -?
Interactive Commands (inside gdisk):
p – Print partition table
n – Add new partition
d – Delete partition
w – Write table to disk and exit
q – Quit without saving
