# Занятие 1. Обновление ядра системы

## Текст задания
1) Запустить ВМ c Ubuntu.
2) Обновить ядро ОС на новейшую стабильную версию из mainline-репозитория.
3) Оформить отчет в README-файле в GitHub-репозитории.

## Основные команды
```bash
# Перед работами проверим текущую версию ядра:
vladimir@ubuntu-server-vm:~$ uname -r
5.15.0-140-generic
```

```bash
# Находим актуальную ссылку и качаем пакеты на виртуальную машину:
vladimir@ubuntu-server-vm:~$ mkdir kernel && cd kernel
vladimir@ubuntu-server-vm:~/kernel$
vladimir@ubuntu-server-vm:~/kernel$ wget https://kernel.ubuntu.com/mainline/v6.15/amd64/linux-headers-6.15.0-061500-generic_6.15.0-061500.202505260036_amd64.deb
--2025-05-30 11:37:17--  https://kernel.ubuntu.com/mainline/v6.15/amd64/linux-headers-6.15.0-061500-generic_6.15.0-061500.202505260036_amd64.deb
Resolving kernel.ubuntu.com (kernel.ubuntu.com)... 185.125.189.75, 185.125.189.76, 185.125.189.74
Connecting to kernel.ubuntu.com (kernel.ubuntu.com)|185.125.189.75|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3689178 (3.5M) [application/x-debian-package]
Saving to: ‘linux-headers-6.15.0-061500-generic_6.15.0-061500.202505260036_amd64.deb’

linux-headers-6.15.0-061500-g 100%[=================================================>]   3.52M   790KB/s    in 4.6s

2025-05-30 11:37:22 (790 KB/s) - ‘linux-headers-6.15.0-061500-generic_6.15.0-061500.202505260036_amd64.deb’ saved [3689178/3689178]
vladimir@ubuntu-server-vm:~/kernel$ wget https://kernel.ubuntu.com/mainline/v6.15/amd64/linux-headers-6.15.0-061500_6.15.0-061500.202505260036_all.deb
--2025-05-30 11:38:13--  https://kernel.ubuntu.com/mainline/v6.15/amd64/linux-headers-6.15.0-061500_6.15.0-061500.202505260036_all.deb
Resolving kernel.ubuntu.com (kernel.ubuntu.com)... 185.125.189.76, 185.125.189.75, 185.125.189.74
Connecting to kernel.ubuntu.com (kernel.ubuntu.com)|185.125.189.76|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14118802 (13M) [application/x-debian-package]
Saving to: ‘linux-headers-6.15.0-061500_6.15.0-061500.202505260036_all.deb’

linux-headers-6.15.0-061500_6 100%[=================================================>]  13.46M  1.70MB/s    in 9.2s

2025-05-30 11:38:22 (1.46 MB/s) - ‘linux-headers-6.15.0-061500_6.15.0-061500.202505260036_all.deb’ saved [14118802/14118802]

vladimir@ubuntu-server-vm:~/kernel$ wget https://kernel.ubuntu.com/mainline/v6.15/amd64/linux-image-unsigned-6.15.0-061500-generic_6.15.0-061500.202505260036_amd64.deb
--2025-05-30 11:39:08--  https://kernel.ubuntu.com/mainline/v6.15/amd64/linux-image-unsigned-6.15.0-061500-generic_6.15.0-061500.202505260036_amd64.deb
Resolving kernel.ubuntu.com (kernel.ubuntu.com)... 185.125.189.74, 185.125.189.75, 185.125.189.76
Connecting to kernel.ubuntu.com (kernel.ubuntu.com)|185.125.189.74|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 15923392 (15M) [application/x-debian-package]
Saving to: ‘linux-image-unsigned-6.15.0-061500-generic_6.15.0-061500.202505260036_amd64.deb’

linux-image-unsigned-6.15.0-0 100%[=================================================>]  15.19M  2.58MB/s    in 5.7s

2025-05-30 11:39:14 (2.66 MB/s) - ‘linux-image-unsigned-6.15.0-061500-generic_6.15.0-061500.202505260036_amd64.deb’ saved [15923392/15923392]

vladimir@ubuntu-server-vm:~/kernel$ wget https://kernel.ubuntu.com/mainline/v6.15/amd64/linux-modules-6.15.0-061500-generic_6.15.0-061500.202505260036_amd64.deb
--2025-05-30 11:39:40--  https://kernel.ubuntu.com/mainline/v6.15/amd64/linux-modules-6.15.0-061500-generic_6.15.0-061500.202505260036_amd64.deb
Resolving kernel.ubuntu.com (kernel.ubuntu.com)... 185.125.189.76, 185.125.189.75, 185.125.189.74
Connecting to kernel.ubuntu.com (kernel.ubuntu.com)|185.125.189.76|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 259891392 (248M) [application/x-debian-package]
Saving to: ‘linux-modules-6.15.0-061500-generic_6.15.0-061500.202505260036_amd64.deb’

linux-modules-6.15.0-061500-g 100%[=================================================>] 247.85M  5.46MB/s    in 96s

2025-05-30 11:41:16 (2.58 MB/s) - ‘linux-modules-6.15.0-061500-generic_6.15.0-061500.202505260036_amd64.deb’ saved [259891392/259891392]
```

```bash
# Устанавливаем все пакеты сразу:
vladimir@ubuntu-server-vm:~/kernel$ sudo dpkg -i *.deb
[sudo] password for vladimir:
Selecting previously unselected package linux-headers-6.15.0-061500.
(Reading database ... 80777 files and directories currently installed.)
Preparing to unpack linux-headers-6.15.0-061500_6.15.0-061500.202505260036_all.deb ...
Unpacking linux-headers-6.15.0-061500 (6.15.0-061500.202505260036) ...
Selecting previously unselected package linux-headers-6.15.0-061500-generic.
Preparing to unpack linux-headers-6.15.0-061500-generic_6.15.0-061500.202505260036_amd64.deb ...
Unpacking linux-headers-6.15.0-061500-generic (6.15.0-061500.202505260036) ...
Selecting previously unselected package linux-image-unsigned-6.15.0-061500-generic.
Preparing to unpack linux-image-unsigned-6.15.0-061500-generic_6.15.0-061500.202505260036_amd64.deb ...
Unpacking linux-image-unsigned-6.15.0-061500-generic (6.15.0-061500.202505260036) ...
Selecting previously unselected package linux-modules-6.15.0-061500-generic.
Preparing to unpack linux-modules-6.15.0-061500-generic_6.15.0-061500.202505260036_amd64.deb ...
Unpacking linux-modules-6.15.0-061500-generic (6.15.0-061500.202505260036) ...
Setting up linux-headers-6.15.0-061500 (6.15.0-061500.202505260036) ...
dpkg: dependency problems prevent configuration of linux-headers-6.15.0-061500-generic:
 linux-headers-6.15.0-061500-generic depends on libc6 (>= 2.38); however:
  Version of libc6:amd64 on system is 2.35-0ubuntu3.9.
 linux-headers-6.15.0-061500-generic depends on libelf1t64 (>= 0.144); however:
  Package libelf1t64 is not installed.
 linux-headers-6.15.0-061500-generic depends on libssl3t64 (>= 3.0.0); however:
  Package libssl3t64 is not installed.

dpkg: error processing package linux-headers-6.15.0-061500-generic (--install):
 dependency problems - leaving unconfigured
Setting up linux-modules-6.15.0-061500-generic (6.15.0-061500.202505260036) ...
Setting up linux-image-unsigned-6.15.0-061500-generic (6.15.0-061500.202505260036) ...
I: /boot/vmlinuz is now a symlink to vmlinuz-6.15.0-061500-generic
I: /boot/initrd.img is now a symlink to initrd.img-6.15.0-061500-generic
Processing triggers for linux-image-unsigned-6.15.0-061500-generic (6.15.0-061500.202505260036) ...
/etc/kernel/postinst.d/initramfs-tools:
update-initramfs: Generating /boot/initrd.img-6.15.0-061500-generic
/etc/kernel/postinst.d/zz-update-grub:
Sourcing file `/etc/default/grub'
Sourcing file `/etc/default/grub.d/init-select.cfg'
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-6.15.0-061500-generic
Found initrd image: /boot/initrd.img-6.15.0-061500-generic
Found linux image: /boot/vmlinuz-5.15.0-140-generic
Found initrd image: /boot/initrd.img-5.15.0-140-generic
Warning: os-prober will not be executed to detect other bootable partitions.
Systems on them will not be added to the GRUB boot configuration.
Check GRUB_DISABLE_OS_PROBER documentation entry.
done
Errors were encountered while processing:
 linux-headers-6.15.0-061500-generic
```

```bash
# Проверяем, что ядро появилось в /boot.
vladimir@ubuntu-server-vm:~/kernel$ ls -al /boot
total 340156
drwxr-xr-x  4 root root      4096 May 30 11:44 .
drwxr-xr-x 20 root root      4096 May 28 06:16 ..
-rw-r--r--  1 root root    262297 Apr 12 05:33 config-5.15.0-140-generic
-rw-r--r--  1 root root    297509 May 26 00:36 config-6.15.0-061500-generic
drwxr-xr-x  5 root root      4096 May 30 11:45 grub
lrwxrwxrwx  1 root root        32 May 30 11:43 initrd.img -> initrd.img-6.15.0-061500-generic
-rw-r--r--  1 root root 109974633 May 28 07:02 initrd.img-5.15.0-140-generic
-rw-r--r--  1 root root 193770646 May 30 11:44 initrd.img-6.15.0-061500-generic
lrwxrwxrwx  1 root root        29 May 28 06:16 initrd.img.old -> initrd.img-5.15.0-140-generic
drwx------  2 root root     16384 May 28 06:13 lost+found
-rw-------  1 root root   6297991 Apr 12 05:33 System.map-5.15.0-140-generic
-rw-------  1 root root  10049224 May 26 00:36 System.map-6.15.0-061500-generic
lrwxrwxrwx  1 root root        29 May 30 11:43 vmlinuz -> vmlinuz-6.15.0-061500-generic
-rw-------  1 root root  11731208 Apr 12 05:39 vmlinuz-5.15.0-140-generic
-rw-------  1 root root  15884800 May 26 00:36 vmlinuz-6.15.0-061500-generic
lrwxrwxrwx  1 root root        26 May 28 06:16 vmlinuz.old -> vmlinuz-5.15.0-140-generic
```

```bash
# Обновить конфигурацию загрузчика:
vladimir@ubuntu-server-vm:~$ sudo update-grub
[sudo] password for vladimir:
Sourcing file `/etc/default/grub'
Sourcing file `/etc/default/grub.d/init-select.cfg'
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-6.15.0-061500-generic
Found initrd image: /boot/initrd.img-6.15.0-061500-generic
Found linux image: /boot/vmlinuz-5.15.0-140-generic
Found initrd image: /boot/initrd.img-5.15.0-140-generic
Warning: os-prober will not be executed to detect other bootable partitions.
Systems on them will not be added to the GRUB boot configuration.
Check GRUB_DISABLE_OS_PROBER documentation entry.
done
```

```bash
# Выбрать загрузку нового ядра по-умолчанию:
vladimir@ubuntu-server-vm:~$ sudo grub-set-default 0
[sudo] password for vladimir:
```

```bash
# После перезагрузки снова проверяем версию ядра (версия должна стать новее):
vladimir@ubuntu-server-vm:~$ uname -r
6.15.0-061500-generic
```