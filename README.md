# secure-time
An OpenBSD clone of Whonix's sdwdate.

Dependencies:
- tor -> `rcctl enable tor && rcctl start tor`
- privoxy -> `forward-socks5t / 127.0.0.1:9050 .` in '/etc/privoxy/config'.

Installation:
1. Copy script to '/usr/local/sbin'.
2. Mark script as executable `chmod +x /usr/local/sbin/secure-time.sh`.
3. Set script permissions `chown root:wheel /usr/local/sbin/secure-time.sh`.
4. Write cronjob to root crontab using `doas crontab -e` (setting date and time requires root), should be random within the hour (Whonix `sdwdate` default).
  - `0 * * * * -ns /bin/sleep $(( 0x$(od -An -N2 -tx /dev/urandom | tr -d ' ') % 3600 )) && /usr/bin/timeout 300 /usr/local/sbin/secure-time.sh`
6. Once confirmed working, disable and stop `ntpd` with `rcctl disable ntpd ; rcctl stop ntpd`.

Uninstallation:
1. Re-enable and start `ntpd` with `rcctl enable ntpd && rcctl start ntpd`.
2. Remove 'secure-time' cronjob with `crontab -e` as root.
3. Delete the script with `rm -f /usr/local/sbin/secure-time.sh` (check script path).
