mkgptdrv
========

Script to wipe a block device and throw down a gpt with multiple partitions, plus add syslinux bios and efi files.
Usage:

This is a simple script to turn a block device into a gpt formated disk.
It does require that a unique name for your Project/Brand be passed with 
the -t <project> argument. This name will be used to set up the EFI loaders.

Additionally, you will need to tell it where it can find a syslinux theme.
This is passed as -c <directory with syslinux.cfg>. This file will be scanned for
the neceassary com32 files, and those files will be copied into the appropriate 
locations for each loader, along with the config folder.

Multiple partitions are supported, and can be added with -p <type:size:label>
The order they are entered will be used when creating the ID. At least one
partition will be made. By default, it will be an ESP partition that will
fill the disk and it will be labeled 'EFI System Partition'. At least one ESP
is required.

Sizes can be expressed as <size>B, K, M, G and S for sectors.

Options:
     --config,-c <dir>		   : Path to syslinux.cfg folder.
        --min,-m <size>		   : Minimum disk size allowed.
  --partition,-p <type:size:label> : Partition shorthand.
      --title,-t <Project/Brand>   : Will be used to label the EFI Loader Directory.
    --overlay,-o <dir>		   : Additional directory to merge in at the root level.
       --sync,-s		   : Mount volumes with sync enabled.
     --device,-d <block device>	   : The last argument is interpreted as the device anyways.
       --help,-h		   : Show this usage.

Partition Types:
         linux,l : Standard Ext4 Linux Volume
          home,h : Ext4 Linux Volume with gpt partition type set to home
          swap,s : Linux Swap Partition
          data,d : General Data Partition Formated Fat32
             LVM : Logical Volume Manager Partition
             ESP : EFI System Partition

Examples:
	mkgptpart -c /sys-themes/default -t 'MyProject' -o /build  /dev/loop0
	
	mkgptpart	-c /sys-themes/menu \\
			-t 'BobsDistro' \\
			-o /build \\
			-p ESP:1g:'EFI System Partition' \\
			-p s:2g \\
			-p l:0:System \\
			/dev/sdb

