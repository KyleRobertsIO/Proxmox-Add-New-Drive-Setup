# **Proxmox SSD Setup**

This is a quick and basic guide to setting up a new SSD or hard drive inside of Proxmox.

---

## **Accessing Proxmox Node Shell**

You will need to SSH into your Proxmox host machine like below.

    ssh <username>@<10.0.0.1> -p 22

## **Viewing Disks On Machine**

To view the disked currently attached to the host system, use the follow command.

    lsblk

This will display details about the drives (disk), their partitions (part) and logical volume managers (lvm)

## **Partition Drive**

To make use of the drive, you will need to first setup a partition for storage.
From the `lsblk` command use the drive name in the command below.

    fdisk /dev/<insert-drive-name>

To configure the partition, enter `n`.

For the following prompts, just use the default values unless you know what you
are doing.

After the prompts, enter `w` to write to the partition table.

## **Creating The Physical Volume & Group Volume**

You can create a physical volume with the command below.

    pvcreate /dev/<insert-drive-name><volume-number>
    Example: /dev/sda1

Next we need to make a volume group that Proxmox can reference for storage.

    vgcreate <volume-group-name> /dev/<insert-drive-name><volume-number>
    Example: samsung-ssd-870-qvo-drive-1 /dev/sda1

## **Add Drive In Proxmox**

Inside your Proxmox web UI, you want to go to `Data Center > Storage > Add > LVM`.
Inside of the field `Volume group` selected you created Group Volume. In the `ID`
field you want to name your drive accordingly.