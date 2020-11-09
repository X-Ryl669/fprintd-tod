# What is this ?

This is a repack of Ubuntu's delivered package for Goodix Fingerprint USB reader so it can be used on any AMD64 machine. It should work with VID:27c6 and PID:533c.
The initial package does not provide source code for the reader, so this is currently a black box. Hopefully, one day, fprint developers will sort this out.

# Installation

Fetch the repository files and save them in `/opt/fprintd-goodix/` (mandatory, or change the path in the service file)

Then you'll have to disable the system genuine `fprintd` service via `sudo systemctl disable fprintd && sudo systemctl stop fprintd`

Then you'll have to symlink the service for it to be usable in systemd: `sudo ln -sf /opt/fprintd-goodix/fprintd-tod.service /etc/systemd/system/fprintd-tod.service`

And enable it (if not done yet): `sudo systemctl enable fprintd-tod && sudo systemctl start fprintd-tod`

Check if it's working: `sudo systemctl status fprintd-tod`


It should returns something like this:
```
● fprintd-tod.service - Fprintd DBUS daemon
     Loaded: loaded (/opt/fprintd-goodix/fprintd-tod.service; enabled; vendor preset: disabled)
     Active: active (running) since Mon 2020-11-09 14:49:57 CET; 1h 55min ago
   Main PID: 3337 (fprintd)
      Tasks: 6 (limit: 38203)
     Memory: 24.6M
     CGroup: /system.slice/fprintd-tod.service
             └─3337 /opt/fprintd-goodix/fprintd -t

nov. 09 14:49:57 Kapital systemd[1]: Started Fprintd DBUS daemon.
```

Then try to enroll your fingerprint: `fprintd-enroll`

It should detect the device and tell you that it captured your fingerprint.
