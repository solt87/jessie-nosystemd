# Converting a Debian 8 "Jessie" completely systemd-free

This is a "compilation" of a few web sources (see at bottom) of how to
_completely_ remove systemd from a Debian 8 system _while_ retaining
desktop functionality (with the likely exception of Gnome).

I did this to my LXDE Jessie system, and so far all works well.

** Only try this at your own risk! **


## Removing systemd and installing sysv-init

* First, install sysv-init:

    `apt-get install sysvinit-core sysvinit sysvinit-utils`

* Reboot
* Purge systemd:

    `apt-get remove --purge --auto-remove systemd`

* Prevent installation of systemd packages:

    `/bin/echo -e 'Package: *systemd*\nPin: origin ""\nPin-Priority: -1' > /etc/apt/preferences.d/systemd`

* Add the "angband repo" to your repo lists:

    `/bin/echo -e 'deb http://angband.pl/debian nosystemd main\ndeb-src http://angband.pl/debian nosystemd main' > /etc/apt/sources.list.d/angband.list`

* Save and add the repo's signing key from [http://angband.pl/deb/archive.html](http://angband.pl/deb/archive.html)

    `apt-key add <the saved keyfile>`

    `apt-get update; apt-get dist-upgrade`

* Search for any systemd "leftovers", and purge them:

    `dpkg --get-selections | grep systemd`
    
    `apt-get remove --purge --auto-remove <remaining systemd packages>`


## Sources

* [http://without-systemd.org/wiki/index.php/How_to_remove_systemd_from_a_Debian_jessie/sid_installation](http://without-systemd.org/wiki/index.php/How_to_remove_systemd_from_a_Debian_jessie/sid_installation)
* [http://lkcl.net/reports/removing_systemd_from_debian/](http://lkcl.net/reports/removing_systemd_from_debian/)
* [https://lists.debian.org/debian-devel/2015/02/msg00189.html](https://lists.debian.org/debian-devel/2015/02/msg00189.html)
