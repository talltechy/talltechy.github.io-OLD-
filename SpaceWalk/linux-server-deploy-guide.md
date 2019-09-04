# Linux Server Deploy Guide

## post-os install

- [Linux Server Deploy Guide](#linux-server-deploy-guide)
  - [post-os install](#post-os-install)
  - [Firewall](#firewall)
    - [Once all rules have been added](#once-all-rules-have-been-added)
  - [Deploy SpaceWalk Client](#deploy-spacewalk-client)
    - [RHEL / CentOS 7 clients](#rhel--centos-7-clients)
    - [Step 2](#step-2)
    - [STEP 3](#step-3)
  - [Networking](#networking)
    - [Find the gateway/route](#find-the-gatewayroute)
    - [Check the resolv.conf file for DNS](#check-the-resolvconf-file-for-dns)
    - [Setup network on CentOS 7 minimal](#setup-network-on-centos-7-minimal)
    - [Change the Hostname](#change-the-hostname)
    - [Display the current hostname information](#display-the-current-hostname-information)
    - [Set a static Hostname](#set-a-static-hostname)
  - [Common errors](#common-errors)
  - [Install Open VM Tools (VMWare Tools)](#install-open-vm-tools-vmware-tools)
    - [Wait for install to complete and reboot](#wait-for-install-to-complete-and-reboot)
  - [Monitor Repo Sync status](#monitor-repo-sync-status)
    - [The Boring Method](#the-boring-method)
    - [The Much Better Method](#the-much-better-method)
      - [This uses the log file I set up for the base CentOS7 Channel as an example](#this-uses-the-log-file-i-set-up-for-the-base-centos7-channel-as-an-example)
  - [Install the SSL Cert to a client machine](#install-the-ssl-cert-to-a-client-machine)
    - [Centos7 / RedHat7](#centos7--redhat7)
      - [Install](#install)

## Firewall

Allow PGSQL-SERVER to connect on port 5432 (this is the default port, configure as you see fit) for PostgreSQL

```bash
firewall-cmd --permanent --zone=public --add-rich-rule='
  rule family="ipv4"
  source address="192.168.10.245/32"
  port protocol="tcp" port="5432" accept'
```

### Once all rules have been added

Check the zone file later to inspect the XML configuration

```bash
[root@SPACEWALK-SERVER ~]# cat /etc/firewalld/zones/public.xml
<?xml version="1.0" encoding="utf-8"?>
<zone>
  <short>Public</short>
  <description>For use in public areas. You do not trust the other computers on networks to not harm your computer. Only selected incoming connections are accepted.</description>
  <service name="ssh"/>
  <service name="dhcpv6-client"/>
  <service name="http"/>
  <service name="https"/>
  <port protocol="tcp" port="5222"/>
  <port protocol="tcp" port="69"/>
  <port protocol="tcp" port="5269"/>
  <rule family="ipv4">
    <source address="192.168.10.245/32"/>
    <port protocol="tcp" port="5432"/>
    <accept/>
  </rule>
</zone>
[root@SPACEWALK-SERVER ~]#
```

Reload the firewall

```bash
[root@SPACEWALK-SERVER ~]# firewall-cmd --reload
success
```

Check the open ports

```bash

```

## Deploy SpaceWalk Client

Note: This will replace mose of the below with autoconfiguration in coming releases

### RHEL / CentOS 7 clients

We need to add with official client repositories for RHEL 7 based clients too.

```bash
# https://copr-be.cloud.fedoraproject.org/results/%40spacewalkproject/spacewalk-2.8-client/epel-7-x86_64/00742644-spacewalk-repo/spacewalk-client-repo-2.8-11.el7.centos.noarch.rpm
# rpm -Uvh http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

### Step 2

In the /etc/yum/pluginconf.d/rhnplugin.conf file simply create a section corresposnding to the channel name for which you want to disable checks.

Most likely you only need to change gpgcheck for [main]

```bash
[root@EXAMPLE-CENTOS7-CLIENTSERVER pluginconf.d]# cat rhnplugin.conf
[main]
enabled = 1
gpgcheck = 0
timeout = 120

# You can specify options per channel, e.g.:
#
#[rhel-i386-server-5]
#enabled = 1
#
#[some-unsigned-custom-channel]
#gpgcheck = 0
```

### STEP 3

Install support program packages for spacewalk clients.

```bash
# yum install rhn-client-tools rhn-check rhn-setup rhnsd m2crypto yum-rhn-plugin -y
```

```bash
sudo rhnreg_ks --serverUrl=https://SPACEWALK-SERVER.EXAMPLEDOMAIN.TLD/XMLRPC --sslCACert=/<-LOCATION HERE-> --activationkey=<-KEY HERE->
```

## Networking

### Find the gateway/route

```bash
ip r
```

OR

```bash
netstat -r -n
```

### Check the resolv.conf file for DNS

```bash
cat /etc/resolv.conf
```

### Setup network on CentOS 7 minimal

First, type "nmcli d" command in your terminal for quick list ethernet card installed on your machine:

```bash
nmcli d
```

Type “nmtui” command in your terminal to open Network manager. After opening Network manager chose “Edit connection” and press Enter (Use TAB button for choosing options):

```bash
sudo nmtui
```

### Change the Hostname

For SpaceWalk the hostname must be in lowercase

### Display the current hostname information

```bash
hostnamectl
```

### Set a static Hostname

```bash
hostnamectl set-hostname your-new-hostname --static
```

## Common errors

```bash
/usr/bin/applydeltarpm not installed.
```

You don't need to do something, but you can, especially if you have a slow internet connection. If you install deltarpm support:

```bash
yum install deltarpm
```

you will download only the differences with older versions of already installed packages. This is done at the cost of increased processing time.

## Install Open VM Tools (VMWare Tools)

```bash
sudo yum install open-vm-tools -y
```

### Wait for install to complete and reboot

```bash
sudo shutdown --reboot now
```

## Monitor Repo Sync status

### The Boring Method

As long as you have not exited the window you can click the big blue button that takes you to an output log file via HTTP

This requres manaual refresh and works poorly

### The Much Better Method

From CLI:

#### This uses the log file I set up for the base CentOS7 Channel as an example

```bash
tail -f /var/log/rhn/reposync/centos7-base.log
```

## Install the SSL Cert to a client machine

### Centos7 / RedHat7

```bash
curl --insecure -O https://SPACEWALK-SERVER.EXAMPLEDOMAIN.TLD/pub/rhn-org-trusted-ssl-cert-1.0-2.noarch.rpm
```

#### Install

```bash
rhnreg_ks --serverUrl=https://SPACEWALK-SERVER.EXAMPLEDOMAIN.TLD/XMLRPC --sslCACert=/usr/share/rhn/RHN-ORG-TRUSTED-SSL-CERT --activationkey=1-PLACEACTIVATIONKEYHERE
```
