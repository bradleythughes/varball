varball - keep /var across reboots on FreeBSD
---------------------------------------------
The idea behind varball is to enable the use of tmpfs(4) or md(4)/mdmfs(8) for
/var without giving up persistence therein.

It uses an rc(8) boot/shutdown script and tar(1) to archive /var into a tar
file on shutdown and to extract it again on boot.

Install varball
---------------
After cloning this repository, you can either
* copy the etc/rc.d/varball script to /etc/rc.d/ or /usr/local/etc/rc.d, or
* configure rc(8) to use your clone by adding 
  `local_startup="$local_startup /path/to/varball/etc/rc.d"` to
  /etc/rc.conf

Enable varball
--------------
To enable varball, add `varball_enable="YES"` to /etc/rc.conf. Set
`varball_file` and `varball_dir` if you want to use values other than the
defaults (`/var.tar` and `/var`, respectively).

Switch /var to tmpfs(4)
-----------------------
After installing and enabling varball, it's time to start using it. The easiest
way to do this is from single-user mode. Use `shutdown now` to ensure that
varball is "stopped" for the first time, creating the initial tar file. After
starting the single-user shell, umount (or remove) /var. Change (or add) the
/var mountpoint in /etc/fstab:
```
tmpfs	/var	tmpfs	rw	0	0
```
Exit single-user mode (or reboot).

Caveats
-------
The tar file is refreshed by rc.shutdown(8), which is in turn called by
shutdown(8). Using reboot(8), halt(8), or any other method that bypasses 
rc.shutdown(8) will prevent the tar file from being updated. This includes
crashes/panics.
