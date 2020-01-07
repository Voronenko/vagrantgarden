For better experience add to `/etc/sudoers.d/YOURUSER` replacing slavko with your username

```
#slavko ALL=(ALL) NOPASSWD: ALL

# vagrant-hostsupdater
Cmnd_Alias VAGRANT_HOSTS_ADD = /bin/sh -c echo "*" >> /etc/hosts
Cmnd_Alias VAGRANT_HOSTS_REMOVE = /usr/bin/sed -i -e /*/ d /etc/hosts
slavko ALL=(root) NOPASSWD: VAGRANT_HOSTS_ADD, VAGRANT_HOSTS_REMOVE

# vagrant-nfs
Cmnd_Alias VAGRANT_EXPORTS_ADD = /usr/bin/tee -a /etc/exports
Cmnd_Alias VAGRANT_NFSD = /sbin/nfsd restart
Cmnd_Alias VAGRANT_EXPORTS_REMOVE = /usr/bin/sed -E -e /*/ d -ibak /etc/exports
slavko ALL=(root) NOPASSWD: VAGRANT_EXPORTS_ADD, VAGRANT_NFSD, VAGRANT_EXPORTS_REMOVE

slavko ALL=(ALL) NOPASSWD: /usr/bin/truecrypt
slavko ALL=(ALL) NOPASSWD: /bin/systemctl
slavko ALL=(ALL) NOPASSWD: /sbin/poweroff, /sbin/reboot, /sbin/shutdown
slavko ALL=(ALL) NOPASSWD: /etc/init.d/nginx, /etc/init.d/mysql, /etc/init.d/mongod, /etc/init.d/redis, /etc/init.d/php-fpm, /usr/bin/pritunl-client-pk-start
slavko ALL=(ALL) NOPASSWD:SETENV: /usr/bin/docker, /usr/sbin/docker-gc, /usr/bin/vagrant

```


# cloud-init perks

Password can be created with

```
openssl passwd -1 -salt SaltSalt secret
```

example

```
openssl passwd -1 -salt slavko Passw0rd1
$1$slavko$mhTAXUOoJfrnQeSlO2AVR.
```

# ESXi notes

For ESXi following cloud-init package needs to be installed

https://github.com/vmware/cloud-init-vmware-guestinfo

## Dealing with naked centos-es

Check adapters

```
nmcli d
```

if exist, type “nmtui” command in your terminal to open Network manager. After opening Network manager chose “Edit connection” 


After activated,  use 
```
ip a
```

to validate, that address was really assigned.

If you are going to use that image as further ESXi template,
consider installing vmware cloud init 

```
curl -L -O ./cloudinit.rpm https://bit.ly/esxi-cloud-init
yum install ./cloudinit.rpm
```

target machine can be exported as ova template using

```
ovftool vi://$GOVC_USERNAME:$GOVC_PASSWORD@$GOVC_URL/VMNAME ./
```

target output can be packed into OVA image using 

```
tar -cvf centos7.ova *.ovf *.vmdk *.nvram *.mf
```

Note: order ovf, vmdk, nvram mf might be important

## ESXi terraform-provider-esxi plugin

I am using patched version of the great terraform-provider-esxi by https://github.com/josenk/terraform-provider-esxi
My original PR was slightly modified by author, and in a current master solution does not work (at least for me)

I have that in a backlog, but until that I am sticking to my version 1.5.4bis

https://github.com/Voronenko/terraform-provider-esxi/tree/1.5.4.bis 

which works nicely on my HomeLab environment
