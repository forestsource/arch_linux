# arch_linux

## If vm env, not exist folder.
ls /sys/firmware/efi/efivars

## check drive
fdisk -l

## Split partition
parted /dev/sda mklabel msdos mkpart primary ext4 1MiB 100%
fdisk -l

## Filesystem
mkfs.ext4 /dev/sda1

## mount
mount /dev/sda1 /mnt

## enable dhcp client
systemctl start dhcpcd
ping 1.1.1.1

## ntp
timedatectl set-ntp true

## set repo mirror
copy JP server to top.

## pacstrap
pacstrap /mnt base linux linux-firmware\
  vim man-db man-pages texinfo \
  dhcpcd \
  networkmanager \
  systemd \
  openssh \
  parted \
  grub

## fstab
genfstab -U /mnt >> /mnt/etc/fstab

## chroot
arch-chroot /mnt

## timezone
ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime 
hwclock --systohc --utc

## locale
locale-gen
echo LANG=en_US.UTF-8 > /etc/locale.conf

## hostname
echo arch > /etc/hostname

## hosts
echo 127.0.0.1 arch arch.localdomain >> /etc/hosts

## enabled
systemd enable NetworkManager
systemd enable sshd

## Initramfs
mkinitcpio -P

## set pass
passwd

## setup grub
grub-install --target=i386-pc /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg

