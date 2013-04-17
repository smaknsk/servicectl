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
1. установленный systemd

Использование
---
### servicectl
```bash
sudo servicectl action service
``` 
This command just exec ${action} from file /usr/lib/systemd/system/${service}.service
If passed action enable or disable, servicectl create or delete symlink on ${service}.service for use serviced.

Параметры:
* action - может быть {start, stop, restart, reload},
* service - имя файла из папки /usr/lib/systemd/system/

### serviced
```bash
sudo serviced action
```
Эта команда выполняет ${action} для всех enable services.

Параметры:
* action - по умолчанию start, может быть {start, stop, restart, reload, disable}

Пример:
Я использую chrome os как базовую систему и archlinux в chroot окружении.
```bash
# inside chroot
sudo servicectl enable nginx php-fpm

# init chroot and run daemons
sudo chroot /path/to/yoursystem /usr/local/bin/serviced
```

Если вы знаете как сделать это лучше, дайте знать =)
Удачи
