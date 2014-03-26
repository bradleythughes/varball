varball - keep /var across reboots on FreeBSD
---------------------------------------------
The idea behind varball is to enable the use of tmpfs(4) or md(4)/mdmfs(8) for
/var without giving up persistence therein.

It uses an rc(8) boot/shutdown script and tar(1) to archive /var into a tar
file on shutdown and to extract it again on boot.
