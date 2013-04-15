Control daemons for systemd in chroot environment
=====================

Servicectl - bash script start/stop service (daemons) for linux using systemd in chroot and SysVinit outside the chroot environment.
servicectl management daemon uses the service files of systemd, such as /usr/lib/systemd/system/nginx.service

Introduction
---

Systemd is not working in chroot environment:
```bash
sudo systemctl start nginx
Running in chroot, ignoring request.
```
If your base system (outside chroot) uses systemd,
please refer to this guide: http://0pointer.de/blog/projects/changing-roots

If your base system (outside chroot) uses Sys V init script,
then you can use servicectl:

Usage
---
```bash
sudo servicectl action service
```
action - can be {start, stop, restart, reload},
service - file name in folder /usr/lib/systemd/system/

Example:
I'm using chrome os as the base system and archlinux in chroot environment.
```bash
sudo servicectl start nginx php-fpm
sudo servicectl stop nginx php-fpm
sudo servicectl restart nginx php-fpm
sudo servicectl reload nginx php-fpm
```

If you know how to do it better, let me know =)
Good luck
