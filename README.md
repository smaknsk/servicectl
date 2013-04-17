Control daemons for systemd in chroot environment
=====================

Servicectl - bash script start/stop service (daemons) for linux using systemd in chroot and SysVinit outside the chroot environment.
servicectl management daemon uses the service files of systemd, e.g. /usr/lib/systemd/system/nginx.service

Introduction
---

Systemd is not working in chroot environment:
```bash
sudo systemctl start nginx
Running in chroot, ignoring request.
```
If your base system (outside chroot):
1. used systemd, please refer to guide: http://0pointer.de/blog/projects/changing-roots
2. used SysVinit script, then you can use servicectl.

Requare for chroot system (inside chroot):
1. installed systemd

Usage
---
### servicectl
```bash
sudo servicectl action service
```
This command just exec ${action} from file /usr/lib/systemd/system/${service}.service
If passed action enable or disable, servicectl create or delete symlink on ${service}.service for use serviced.

Params:
* action - can be {start, stop, restart, reload, enable, disable},
* service - file name in folder /usr/lib/systemd/system/

### serviced
```bash
sudo serviced action
```
This command exec ${action} for all enable services.

Params:
* action - by default start, can be {start, stop, restart, reload, disable}

Example:
I'm using chrome os as the base system and archlinux in chroot environment.
```bash
# inside chroot
sudo servicectl enable nginx php-fpm

# outside chroot: 
# init chroot and run daemons
sudo chroot /path/to/yoursystem /usr/local/bin/serviced
```

If you know how to do it better, let me know =) 
Good luck
