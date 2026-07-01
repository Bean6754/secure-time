# secure-time

An OpenBSD rewrite of Whonix's 'sdwdate' and 'bootclockrandomization'.


__Dependencies:__

**`oniondate` (sdwdate-clone)**

- tor -> `rcctl enable tor && rcctl start tor`
- privoxy -> `forward-socks5t / 127.0.0.1:9050 .` in '/etc/privoxy/config'.

**`bootclockrandom` (bootclockrandomization-clone)**

- N/A (standard modern up-to-date OpenBSD install)


__Installation:__

**`oniondate` (sdwdate-clone)**

1. Copy script to '/usr/local/sbin'.
2. Mark script as executable `chmod +x /usr/local/sbin/oniondate`.
3. Set script permissions `chown root:wheel /usr/local/sbin/oniondate`.
4. Write random time cronjob to root crontab (setting date and time requires root), example is found in repo `var/cron/tabs/root`.
5. Once confirmed working, disable and stop `ntpd` with `rcctl disable ntpd ; rcctl stop ntpd`.

**`bootclockrandom` (bootclockrandomization-clone)**

1. Copy script to '/usr/local/sbin'.
2. Mark script as executable `chmod +x /usr/local/sbin/bootclockrandom`.
3. Set script permissions `chown root:wheel /usr/local/sbin/bootclockrandom`.
4. Write startup block to `/etc/rc.local` found in repo `etc/rc.local`.
5. Check `/var/log/messages` after reboot to confirm working.
  - `cat /var/log/messages | grep bootclockrandom`


__Checking and debugging:__

You can check the status of 'oniondate' or 'bootclockrandom' by looking at `/var/log/messages`, or by running the scripts manually in a shell.


__Uninstallation:__

**`oniondate` (sdwdate-clone)**

1. Re-enable and start `ntpd` with `rcctl enable ntpd && rcctl start ntpd`.
2. Remove 'oniondate' cronjob with `crontab -e` as root.
3. Delete the script with `rm -f /usr/local/sbin/oniondate` (check script path).

**`bootclockrandom` (bootclockrandomization-clone)**

1. Remove startup block from `/etc/rc.local`, if `/etc/rc.local` is now empty then you can safely delete it.
2. Delete the script with `rm -f /usr/local/sbin/bootclockrandom` (check script path).
