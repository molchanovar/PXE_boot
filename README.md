# PXE_boot
Netwok booting on PXE


### Установить и настроить загрузку по сети для дистрибутива CentOS8:

1. Шаги - [centos.org](https://docs.centos.org/en-US/8-docs/advanced-install/assembly_preparing-for-a-network-install) + [github.com/nixuser](https://github.com/nixuser/virtlab/tree/main/centos_pxe)
2. Настроить установку из репозитория HTTP

### Результат:
Полностью автоматизировать не получилось. На PXE сервере не хватает памяти для образа CentOS 8.3 (диск на сервере по умолчанию 10ГБ, образ 9ГБ).
Поэтому для тестирования стенда требуется скачать образ CentOS 8.3 (например [отсюда](http://ftp.mgts.by/pub/CentOS/8.3.2011/isos/x86_64/CentOS-8.3.2011-x86_64-dvd1.iso)) и вручную примонтировать образ на сервер. 

#### Закоментированные строчки в скрипте (скачивание и монтирование образа):
```
curl -O http://ftp.mgts.by/pub/CentOS/8.3.2011/isos/x86_64/CentOS-8.3.2011-x86_64-dvd1.iso
mount -t iso9660 CentOS-8.3.2011-x86_64-dvd1.iso /mnt/centos8-install
```

#### Монтирование внешнего образа (/dev/sr0):
```
[root@pxeserver vagrant]# lsblk 
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT 
sda      8:0    0   10G  0 disk
└─sda1   8:1    0   10G  0 part /
sr0     11:0    1  8.6G  0 rom

mount -t iso9660 /dev/sr0 /mnt/centos8-install/
```

В GUI приложении OracleBox зайти в настройки VM pxeserver -> носители -> добавить загрузочный образ. Еще надо убрать галки с порядка загрузки (Система -> порядок загрузки -> оставить только жесткий диск)


После указанных действий начнется загрузка системы. 


#### Шаринг по NFS:
```
[root@pxeserver vagrant]# showmount -a 
All mount points on pxeserver:
10.0.0.100:/mnt/centos8-install
```


