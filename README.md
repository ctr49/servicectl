Control daemons for systemd in chroot environment
=====================

Servicectl - bash script start/stop service (daemons) for linux using systemd in chroot and SysVinit outside the chroot environment.
servicectl management daemon uses the service files of systemd, e.g. /lib/systemd/system/nginx.service

Introduction
---

Systemd is not working in chroot environment:
```bash
sudo systemctl start nginx
Running in chroot, ignoring request.
```
If your base system (outside chroot):
* uses systemd, please refer to guide: http://0pointer.de/blog/projects/changing-roots
* uses SysVinit script, then you can use servicectl.

Requirement for chroot system (inside chroot):
* installed systemd

Installation
---

### Manual:
```bash
wget https://github.com/ctr49/servicectl/tarball/master
tar --strip-components 1 -C /usr/local/bin --wildcards -zxf master */service*
mkdir /etc/init.d-chroot
```

Usage
---
### servicectl
```bash
sudo servicectl action service
```
This command just executes ${action} from file /usr/lib/systemd/system/${service}.service
If passed action enable or disable, servicectl will create or delete symlink on ${service}.service to /etc/init.d-chroot for use by serviced.

Params:
* action - can be {start, stop, restart, reload, enable, disable}
* service - file name in folder /usr/lib/systemd/system/

### serviced
```bash
sudo serviced action
```
This command exec ${action} for all enable services.

Params:
* action - by default start, can be {start, stop, restart, reload, disable}

Example
---
```bash
# inside chroot
sudo servicectl enable nginx php-fpm

# outside chroot: 
# init startup and run all enabled daemons
sudo chroot /path/to/chroot serviced
```
