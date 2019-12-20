# arch_linux

## Enable network 
```
systemctl start dhcpcd
ping 1.1.1.1
passwd
systemctl start sshd
```

## Check Boot mode.
```
## If vm env, not exist folder. (BIOS) 
ls /sys/firmware/efi/efivars 
```

## Check device directory.
```
fdisk -l
```

## Make partitions.
```
parted
mklabel msdos
mkpart primary ext4 1MiB 100MiB
mkpart primary ext4 100MiB 100%
q

## check device name
fdisk -l

## make filesystem
mkfs.ext4 /dev/sda1
mkfs.ext4 /dev/sda2
```

## Mount
```
mount /dev/sda2 /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
```

## ntp
```
timedatectl set-ntp true
```

## set repo mirror
```
## insert mirror into first empty line.
sed -ie '6s/^/Server=http:\/\/ftp.tsukuba.wide.ad.jp\/Linux\/archlinux\/$repo\/os\/$arch/g' /etc/pacman.d/mirrorlist
```

## pacstrap
```
# basic
pacstrap /mnt base linux linux-firmware

## troubleshoot
pacstrap /mnt \
  vim man-db man-pages texinfo \
  dhcpcd \
  networkmanager \
  systemd \
  openssh \
  parted \
  grub

## for ansible
pacstrap /mnt git ansible sshpass sudo 
```

## fstab
```
genfstab -U /mnt >> /mnt/etc/fstab
```

## chroot
```
arch-chroot /mnt
```

## timezone
```
ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime 
hwclock --systohc --utc
```

## locale
```
locale-gen
echo LANG=en_US.UTF-8 > /etc/locale.conf
```

## hostname
```
echo arch > /etc/hostname
```

## hosts
```
echo 127.0.0.1 arch arch.localdomain >> /etc/hosts
```

## enabled
```
systemctl enable NetworkManager
systemctl enable sshd
```

## Initramfs
```
mkinitcpio -P
```

## set pass
```
passwd
```

## setup grub
```
grub-install --target=i386-pc /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
```

## prepare anible
```
# create ansible user
useradd -m ansible
passwd ansible

# setup sshd
touch /home/ansible/.authorized_keys
echo 'your public key' >> /home/ansible/.authorized_keys

# add ansible to sudoers
EDITOR=/usr/bin/vim visudo # grant some privilege

# download ansible
cd /home/ansible
git clone https://github.com/forestsource/dev_env_ansible.git
```

## reboot
```
exit # exit chroot.
reboot
```

# Ansible 
```
EDITOR=vim ansible-vault create secret.yml
ansible-playbook -i local site.yml -u ansible -k --ask-vault-pass
```
