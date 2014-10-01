Управление демонами для systemd в chroot окружении
=====================

Servicectl - bash скрипт старта/остановки демонов для linux использующий systemd в chroot и SysVinit за пределами chroot окружения
servicectl для управления демонами использует service файлы из systemd, такие как /usr/lib/systemd/system/nginx.service

Введение
---

Существует ограничение в systemd, он не работает в chroot окружении:
```bash
sudo systemctl start nginx
Running in chroot, ignoring request.
```
Если ваша базовая система (за пределами chroot):
* использует systemd, то обратитесь в этому руководству: http://0pointer.de/blog/projects/changing-roots
* использует System V init script, то вы можете использовать servicectl.

Требования для chroot системы (внутри chroot):
* установленный systemd

Установка
---

### Packages:
ArchLinux - https://aur.archlinux.org/packages/servicectl/
### Manual:
```bash
    wget https://github.com/smaknsk/servicectl/archive/1.0.tar.gz
    tar -xf 1.0.tar.gz -C /usr/local/lib/
    ln -s /usr/local/lib/servicectl-1.0/servicectl /usr/local/bin/servicectl
    ln -s /usr/local/lib/servicectl-1.0/serviced /usr/local/bin/serviced
```

Использование
---
### servicectl
```bash
sudo servicectl action service
``` 
Эта команда просто запускает ${action} из файла /usr/lib/systemd/system/${service}.service
Если передан action enable или disable, servicectl создаст или удалит symlink на ${service}.service для использования в serviced.

Параметры:
* action - может быть {start, stop, restart, reload, enable, disable}
* service - имя файла из папки /usr/lib/systemd/system/

### serviced
```bash
sudo serviced action
```
Эта команда выполняет ${action} для всех enable services.

Параметры:
* action - по умолчанию start, может быть {start, stop, restart, reload, disable}

Пример
---
Я использую chrome os как базовую систему и archlinux в chroot окружении.
```bash
# inside chroot
sudo servicectl enable nginx php-fpm

# outside chroot: 
# startup chroot and run all enabled daemons
sudo chroot /path/to/chroot serviced
```

Если вы знаете как сделать это лучше, дайте знать =)
Удачи
