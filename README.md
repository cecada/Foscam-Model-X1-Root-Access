<table>
<tr>
	<td colspan=2><b><h2 align=center>Equipment Overview
<tr>
	<td colspan=2 align=center>
		<center>
		<img width=300px align=center src="https://raw.githubusercontent.com/cecada/Foscam-Model-X1-Root-Access/main/images/20200626_081025.jpg"> 
		<img width=300px align=center src="https://raw.githubusercontent.com/cecada/Foscam-Model-X1-Root-Access/main/images/20200702_153529.jpg"> 
		<img width=300px align=center src="https://raw.githubusercontent.com/cecada/Foscam-Model-X1-Root-Access/main/images/20200823_081724.jpg">
<tr>
	<td colspan=1> <b>Wifi Camera: 
	<td colspan=1>Foscam Model X1
<tr>
	<td colspan=1><b>Firmware Version:
	<td colspan=1>1.14.2.4
<tr>
	<td colspan=1><b>Linux Version:
	<td colspan=1>3.10.14__isvp_turkey_1.0__
<tr>
	<td colspan=1><b>GCC Version:
	<td colspan=1>4.7.2
<tr>
	<td colspan=1><b>UBoot Version:
	<td colspan=1>2013.07 (May 05 2019 - 17:08:36)
<tr>
	<td colspan=1><b>Busybox Version:
	<td colspan=1>v1.22.1
<tr>
	<td colspan=1 width=25%><b>Busybox Functions:
	<td colspan=1><p align="justify">[, [[, acpid, addgroup, adduser, arp, arping, ash, awk, base64, basename, blkid, bootchartd, brctl, cal, cat, catv, chgrp, chmod, chown, chpasswd, chroot, clear, cmp, cp, cryptpw, cttyhack, date, dd, deallocvt, delgroup, deluser, depmod, devmem, df, dhcprelay, diff, dirname, dmesg, dnsd, dnsdomainname, dos2unix, du, dumpleases, echo, ed, egrep, env, ether-wake, fakeidentd, false, fbset, fdflush, fdformat, fdisk, fgrep, find, findfs, flash_eraseall, flashcp, flock, fold, free, freeramdisk, fsync, ftpget, ftpput, fuser, getopt, getty, grep, groups, gzip, halt, hd, hexdump, hostid, hostname, httpd, hush, hwclock, id, ifconfig, ifdown, ifenslave, ifplugd, ifup, inetd, init, insmod, iostat, ip, ipaddr, ipcalc, ipcrm, ipcs, iplink, iproute, iprule, iptunnel, kill, killall, killall5, klogd, less, linux32, linux64, linuxrc, ln, logger, login, logname, logread, losetup, ls, lsmod, lsof, lsusb, makedevs, md5sum, mdev, mesg, mkdir, mkdosfs, mkfs.vfat, mknod, mkpasswd, mkswap, mktemp, modinfo, modprobe, mount, mountpoint, mpstat, mv, nameif, nbd-client, nc, netstat, nmeter, nslookup, ntpd, passwd, pgrep, pidof, ping, ping6, pivot_root, pkill, pmap, poweroff, printenv, printf, ps, pscan, pstree, pwd, pwdx, rdate, rdev, readlink, readprofile, realpath, reboot, renice, reset, resize, rev, rm, rmdir, rmmod, route, rtcwake, sed, seq, setarch, setconsole, sh, sha1sum, sha256sum, sha512sum, slattach, sleep, smemcap, sort, stat, sulogin, sum, swapoff, swapon, switch_root, sync, sysctl, syslogd, tail, tar, tcpsvd, telnet, telnetd, test, tftp, tftpd, time, timeout, top, touch, tr, traceroute, traceroute6, true, tty, ttysize, tunctl, udhcpc, udhcpd, udpsvd, umount, uname, unix2dos, unzip, uptime, usleep, uudecode, uuencode, vconfig, vi, vlock, volname, watch, watchdog, wc, wget, whoami, whois, xargs, yes, zcip
<tr>
	<td colspan=1><b>CPU Info: 
	<td colspan=1>Ingenic Xburst
<tr>
	<td colspan=1><b>Hardward Access: 
	<td colspan=1>SPI, UART
<tr>
	<td colspan=1><b>SPI Flash:
	<td colspan=1>BY25Q128AS (<a href="https://github.com/cecada/Foscam-Model-X1-Root-Access/blob/main/documents/1904091402_BOYAMICRO-BY25Q128ASSIG_C383794.pdf">Spec Sheet</a>)
<tr>
	<td colspan=1><b>HTTP Admin Access: 
	<td colspan=1> http://Assigned IP Address:88/
</table>

<h2>Boot to Root</h2>

<p align="justify">Though other CVEs and write-ups have greatly detailed Foscam's reliance on static UART/Uboot password the X1 model appears to have bucked the trend. Though dumping the firmware via SPI is trivial, but <a href = "https://github.com/santeri3700/opticam_o8_hacking">as this relatively recent git details</a> the hash $1$xY/YSetV$dbTV4dHv6gWzmAlfYTboG1 isn't known to have been cracked. Also, UBoot is password protected. :(

<p align="justify"><b>Note</b> Flashrom (as of 10/25/2020) does not natively support the Boya Microelectronics SPI chip this camera uses. I added the functionality and created a pull request to the flashrom repo which is currently pending. However, in the meantime, you can <a href = "https://review.coreboot.org/cgit/flashrom.git/commit/?id=fe014acf4418f071e982b49bbcc1c5db4d801dfc">use this patch</a> if you want to compile it yourself.  

<p align="justify">However, upon examining a hex dump of the dumped firmware we get the UBoot password.

```
00034240: 4869 7420 616e 7920 6b65 7920 746f 2073  Hit any key to s
00034250: 746f 7020 6175 746f 626f 6f74 3a20 2532  top autoboot: %2
00034260: 6420 0000 0a25 6473 7420 696e 7075 7420  d ...%dst input 
00034270: 5061 7373 7764 3a00 6970 632e 666f 737e  Passwd:.ipc.fos~
00034280: 0000 0000 0808 0825 3264 2000 3c49 4e54  .......%2d .<INT
00034290: 4552 5255 5054 3e0a 0000 0000 7365 7269  ERRUPT>.....seri
000342a0: 616c 0000 4352 4300 436b 7375 6d00 0000  al..CRC.Cksum...
```
00034270: 5061 7373 7764 3a00 6970 632e 666f 737e  Passwd:.**ipc.fos~**

Now we can login to UBoot and modify the UBoot bootargs enviroment variable to something like:

```
setenv bootargs 'console=ttyS1,115200n8 mem=100M@0x0 rmem=28M@0x6400000 mtdparts=jz_sfc:256k(boot),3072k(kernel),11264k(appfs),1024k(patch),256k(backup),512k(para) lateshell earlyprintk initcall_debug rdinit=/bin/sh'
```

When you reboot you will drop into a shell. However, most of the Foscam components are not loaded yet. However, this is a mere pause. All we want to do is change the admin password; which is accomplished by:

```
echo root:test | /usr/sbin/chpasswd -m && /linuxrc
```

This command changes the root password to test, hashes it in the form expected by the Linux build, and then continues the booting of the camera. Once booted, log in with... you guessed it username: root, password: test.

