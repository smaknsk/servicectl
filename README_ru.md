Control services (daemons) for systemd in chroot() environment
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
Если ваша базовая система (за пределами chroot) использует systemd,
то обратитесь в этому руководству: http://0pointer.de/blog/projects/changing-roots

Если ваша базовая система (за пределами chroot) использует Sys V init script,
то вы можете использовать servicectl:

Использование
---
```bash
sudo servicectl action service
``` 
action - может быть {start, stop, restart, reload},
service - имя файла из папки /usr/lib/systemd/system/

Пример:
Я использую chrome os как базовую систему и archlinux в chroot окружении.
```bash
sudo servicectl start nginx php-fpm
sudo servicectl stop nginx php-fpm
sudo servicectl restart nginx php-fpm
sudo servicectl reload nginx php-fpm
```

Если вы знаете как сделать это лучше, дайте знать =)
Удачи