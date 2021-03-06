===========
ArchVersion
===========


INTRODUCTION
============
*archversion* is a tool used to compare upstream and downstream versions of archlinux packages.

Downstream packages version can be found in:
- the *Archlinux* web site [#]_;
- the *Archlinux* User Repository [#]_;
- a local pacman databases;
- a local abs sync.

You can also use it to update a PKGBUILD to the upstream version.

It targets Devs and TUs to help them to stay informed of new release of their packages.
It can also be used by AUR maintainers to track update for their packages.


HOW TO USE IT
=============
The first thing to do is to define a list of packages to track by creating a file
~/.config/archversion/packages.conf. The file content looks like an old fashioned INI file.

The following example allow to check the last version of acpid against archlinux
official repositories

|  [acpid]
|  url = http://sourceforge.net/projects/acpid2/files/

You can find more complete examples in the misc/ directory.

Basically, you can run:
*archversion sync* to fetch last versions from upstream and downstream.
*archversion report --new* to display new verions.
*archversion report --sync acpid* to sync and display version report of the acpid package.
*archversion update* to update the current PKGBUILD to the last upstream version.

You can use systemd timers to get a report of packages which need updates:
$ systemctl enable archversion.timer
$ systemctl start archversion.timer

To update a PKGBUILD to the last upstream version, run:
$ archversion update

HOW IT WORKS
============
As simple as possible! *archversion* retrieve the content of the provided upstream
webpage and search for well-known pattern. And then compare it to the reference.


DOWNSTREAM MODES
================

pacman
------
This mode compare a remote upstream version against a local package version from
pacman databases.
This software is not responsible of syncing pacman databases. Please do it yourself.
This mode is recommended.

archweb
-------
This mode compare a remote upstream version against a remote package version
on *www.archlinux.org*.
Getting version is done using the json ouput of packages pages.
Unfortunatly, Archweb doesn't offer a RPC, so find the right URL for a package
need a lot of call. As a consequency it's slow and load the archlinux servers.
So, I recommend to avoid using this mode in favour of pacman mode!

aur
---
This mode compare a remomte upstream version against a remote package version
from the *Archlinux User Repository*.
AUR provides a JSON-RPC which allow to easily query about packages.

abs
---
This mode compare a remote upstream version against a local package version from
a synced ABS filesystem tree.
This is not responsible of syncing ABS tree. Please do it yourself.
This is **DANGEROUS** because PKGBUILD are *partially* executed to guess the package version!
So, prefer pacman mode!

none
----
This mode is a fake one, it only retrieves upstream version without any comparaison.


DEPENDENCIES
============

- Python 3.x [#]_
- PyXDG [#]_
- PyALPM [#]_


SOURCES
=======
*archversion* sources are available on github [#]_.

Once you get the git tree, you can build a test package:

|  ./bootstrap
|  ./configure
|  make pkg

LICENSE
=======
*archversion* is licensied under the term of GPL v2 [#]_.


AUTHOR
======
*archversion* was started by Sébastien Luttringer in July 2012.


LINKS
=====
.. [#] http://www.archlinux.org/
.. [#] http://aur.archlinux.org/
.. [#] http://python.org/download/releases/
.. [#] http://freedesktop.org/wiki/Software/pyxdg
.. [#] https://projects.archlinux.org/users/remy/pyalpm.git
.. [#] https://github.com/seblu/archversion/
.. [#] http://www.gnu.org/licenses/gpl-2.0.html
