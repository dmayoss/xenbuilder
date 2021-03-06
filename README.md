# Xenbuilder README

## Overview

Xenbuilder is a script designed to help you create, start, stop and delete mass numbers of xen
VM's. It is 'mostly automated', meaning that you have to do the groundwork first, and then
your server will work with this script with minimal interference.

It is written in perl and makes extensive use of perl modules where available, and system calls
where not.

This readme file is designed to be read on github. It is written using github's
own flavour of markdown.

## default paths

* base is _/opt/xenbuilder_
* subdirectories:

<TABLE>
<TR>
<TD>./bin</TD><TD>binaries and scripts</TD>
</TR>
<TR>
<TD>./etc</TD><TD>templates and config files</TD>
</TR>
<TR>
<TD>./images</TD><TD>ISO's and saved images</TD>
</TR>
<TR>
<TD>./logs</TD><TD>logs</TD>
</TR>
</TABLE>

## useful files

* in _./bin_  

<TABLE>
<TR>
<TD>xenbuilder</TD><TD>xenbuilder script</TD>
</TR>
<TR>
<TD>xenbuilder.conf</TD><TD>xenbuilder script config file</TD>
</TR>
<TR>
<TD>create_image.sh</TD><TD>example commands for xen-create-image</TD>
</TR>
<TR>
<TD>README</TD><TD>this file</TD>
</TR>
</TABLE>

in _./etc_

<TABLE>
<TR>
<TD>template.template</TD><TD>xen config file (some are templates)</TD>
</TR>
<TR>
<TD>template.conf</TD><TD>config file to enable scripting of template .cfg files</TD>
</TR>
<TR>
<TD>vmname-id.vm</TD><TD>running xen domain from xenbuilder</TD>
</TR>
</TABLE>


## required ubuntu packages
* thin-provisioning-tools
* libconfig-simple-perl
* libparallel-forkmanager-perl
* liblist-moreutils-perl
* libxen-4.3
* xen-tools
* xen-hypervisor-4.3

## useful ubuntu packages
* git-extras
  * for git-ignore
* xen-tools
  * if you wish to use xen-create-image (check create_image.sh)
* debootstrap
  * if you wish to use automated VM creation

## necessary configurations
* create the XEN Volume Group
  * set $vmVG (eg: VG01, XENVG) in xenbuilder.conf
* create /dev/$vmVG/thinpool
  * create this LV as a thin pool (e.g. 250G) for VM's
  * ```example code here```
* create /dev/$vmVG/OPT for xenbuilder to live in
  * mount on /opt/xenbuilder (e.g. 250G)
* create the vm source lvol's using dd or ```your favourite tool here```
  * eg: precise-disk from precise-disk.img

## ubuntu system options:
* add atime to /, /boot, /opt
* add the following to /etc/rc.local (And set to 755)

        /sbin/iptables --table nat --append POSTROUTING --out-interface eth0 -j MASQUERADE  
        /sbin/iptables --append FORWARD --in-interface bridge0 -j ACCEPT  
        mkdir -p /var/run/xen  

## TODO

- [x] Stabilize options (--start,--stop,--create,--delete,--templates)
- [x] fix thin handling via config options
- [x] work out why/how xen-4.x pygrub works properly and/or automate kernel config for templates
- [x] fix templates not to be .cfg but .template
- [] integrate pypxeboot from http://grid.ie/pypxeboot/ (v.0.0.3)

