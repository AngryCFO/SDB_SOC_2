# Домашнее задание к занятию "Защита хоста" - `Александра Бужор`

---

### Задание 1

Установите eCryptfs.
Добавьте пользователя cryptouser.
Зашифруйте домашний каталог пользователя с помощью eCryptfs.
В качестве ответа пришлите снимки экрана домашнего каталога пользователя с исходными и зашифрованными данными.

### Решение:

```bash
root@vm1:/home/udjin# apt-get install ecryptfs-utils -y

root@vm1:/home/udjin# adduser cryptouser
info: Adding user `cryptouser' ...
info: Selecting UID/GID from range 1000 to 59999 ...
info: Adding new group `cryptouser' (1001) ...
info: Adding new user `cryptouser' (1001) with group `cryptouser (1001)' ...
info: Creating home directory `/home/cryptouser' ...
info: Copying files from `/etc/skel' ...

root@vm1:/home/udjin# ecryptfs-migrate-home -u cryptouser
INFO:  Checking disk space, this may take a few moments.  Please be patient.
INFO:  Checking for open files in /home/cryptouser
Enter your login passphrase [cryptouser]:

************************************************************************
YOU SHOULD RECORD YOUR MOUNT PASSPHRASE AND STORE IT IN A SAFE LOCATION.
  ecryptfs-unwrap-passphrase ~/.ecryptfs/wrapped-passphrase
THIS WILL BE REQUIRED IF YOU NEED TO RECOVER YOUR DATA AT A LATER TIME.
************************************************************************

Done configuring.
```

```bash
cryptouser@vm1:/home/udjin$ ls /home/cryptouser
cryptouser@vm1:/home/udjin$ touch /home/cryptouser/test.txt
cryptouser@vm1:/home/udjin$ ls /home/cryptouser
test.txt

udjin@vm1:~$ ls /home/cryptouser
ls: cannot open directory '/home/cryptouser': Permission denied

root@vm1:/home/udjin# ls /home/cryptouser
Access-Your-Private-Data.desktop  README.txt
root@vm1:/home/udjin# cat /home/cryptouser/Access-Your-Private-Data.desktop
[Desktop Entry]
_Name=Access Your Private Data
_GenericName=Access Your Private Data
Exec=/usr/bin/ecryptfs-mount-private
Terminal=true
Type=Application
Categories=System;Security;
X-Ubuntu-Gettext-Domain=ecryptfs-utils
root@vm1:/home/udjin#
```

---

### Задание 2

Установите поддержку LUKS.
Создайте небольшой раздел, например, 100 Мб.
Зашифруйте созданный раздел с помощью LUKS.
В качестве ответа пришлите снимки экрана с поэтапным выполнением задания.

### Решение:

```bash
root@vm1:/home/udjin# apt-get install cryptsetup -y

root@vm1:/home/udjin# cryptsetup luksFormat /dev/sdb1
WARNING: Device /dev/sdb1 already contains a 'ext4' superblock signature.

WARNING!
========
This will overwrite data on /dev/sdb1 irrevocably.

Are you sure? (Type 'yes' in capital letters): YES
Enter passphrase for /dev/sdb1:
Verify passphrase:

root@vm1:/home/udjin# cryptsetup open /dev/sdb1 encrypted
Enter passphrase for /dev/sdb1:

root@vm1:/home/udjin# mkfs.ext4 /dev/mapper/encrypted
mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 126720 4k blocks and 126720 inodes
Filesystem UUID: bdd5222e-8c67-4040-8f11-930a751371de
Superblock backups stored on blocks:
        32768, 98304

Allocating group tables: done
Writing inode tables: done
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done

root@vm1:/home/udjin# mkdir /mnt/encrypted

root@vm1:/home/udjin# mount /dev/mapper/encrypted /mnt/encrypted

root@vm1:/home/udjin# touch /mnt/encrypted/test.txt

root@vm1:/home/udjin# ls /mnt/encrypted/
lost+found  test.txt

```

---

### Задание 3*

Установите apparmor.
Повторите эксперимент, указанный в лекции.
Отключите (удалите) apparmor.
В качестве ответа пришлите снимки экрана с поэтапным выполнением задания.

### Решение:
![image](https://github.com/user-attachments/assets/7f95dc2d-a2f3-49d2-845a-cf9eb978c719)
