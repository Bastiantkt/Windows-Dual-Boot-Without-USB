# Dual Boot Windows Installation from a Live System

## Prerequisites
- A bootable Windows installation media (USB/DVD).
- A live system (e.g., Windows PE or any Linux live system with access to `diskpart` and `dism` commands).
- A working knowledge of partitioning and disk management.

## Installation Steps

## 1. Open Command Prompt (or equivalent terminal)

Start by opening the command prompt or terminal in your live system.

---

## 2. Partitioning the Disk

You will use `diskpart` to create a new partition for Windows.

2.1 List Disks

`diskpart`

`list disk`

This will list all available disks. Identify the disk where you want to install Windows / Shrink the partition.

2.2 Select the Disk

`sel disk x`

Replace x with the number of the disk where you want to install Windows.

2.3 List Partitions

`list par`

This will show existing partitions on the selected disk.

2.4 Select the Partition to Shrink

`sel par x`

Replace x with the partition you want to shrink.

2.5 Shrink the Partition

`shrink`

This will shrink the selected partition to create free space for the new partition.

2.6 Create a Primary Partition

`cre par pri`

This creates a new primary partition in the free space.

2.7 Format the Partition

`form quick`

This will quickly format the new partition.

2.8 Assign a Drive Letter to the Partition

`assign letter Z`

This assigns the drive letter Z to the newly created partition.

2.9 List Partitions Again

`list par`

Verify that your new partition has been created successfully.

2.10 Assign a Drive Letter to the EFI Partition

Find the EFI partition (usually labeled as such) and assign it a letter (e.g., W):

`sel par x`

`assign letter W`

Replace x with the partition number of the EFI partition.

## 3. Apply Windows Image

Now you will apply the Windows image to the newly created partition using dism.

3.1 Get Information About the Windows Image

Run the following command to get information about the available Windows editions in your install.wim or install.esd file:

`dism /get-imageinfo /imagefile:install.wim`

or

`dism /get-imageinfo /imagefile:install.esd`

This will show you the available Windows editions. Note the x index for the edition you want to install.

3.2 Apply the Windows Image

Now, apply the selected Windows edition to the new partition:

`dism /apply-image /imagefile:install.wim /index:x /applydir:Z:`

Replace x with the index of the desired Windows edition.

## 4. Set Up Boot Configuration

Once the image is applied, you need to set up the bootloader.

4.1 Configure the Bootloader

`bcdboot Z:\Windows /s W:`

This will copy the necessary boot files to the EFI partition.

## 5. Restart the System

Finally, reboot the system to complete the installation process.

5.1 Shutdown and Restart

`shutdown /t 0 /r`

This will restart the system immediately. You should now see the option to select which operating system to boot into.
