# How to Integrate RHEL 7 or CentOS 7 with Windows Active Directory

In Most of the Organizations users and groups are created and managed on Windows Active Directory.  We can integrate our RHEL 7 and CentOS 7 servers with AD(Active Directory) for authenticate purpose. In other words we can join our CentOS 7 and RHEL 7 Server on Windows Domain so that system admins can login to these Linux servers with AD credentials. While creating UNIX users on AD we can map these users to a specific group so that level of access is controlled centrally from AD.

In this article we discuss how to integrate CentOS 7.x & RHEL 7.x with AD(Windows Server 2008 R2 & Windows Server 2012 R2). Following steps are applicable for both CentOS 7 and RHEL 7.

## Step:1 Install the required packages using yum command

Use the yum command to install following packages from the command line.

```bash
[root@exampleserver.local ~]# yum install sssd realmd oddjob oddjob-mkhomedir adcli samba-common samba-common-tools krb5-workstation openldap-clients policycoreutils-python
```

Update the /etc/hosts file and /etc/resolv.conf so that dns name or hostname of AD server gets resolved correctly. In my case AD server hostname is “example-dc1.local“, so place the below line in /etc/hosts file

```bash
192.168.10.240      example-dc1.local     example-dc1
```

Contents of resolv.conf should be something like below. Just replace the domain name and ip address of dns server as per your setup

```bash
[root@exampleserver.local ~]# cat /etc/resolv.conf
search exampledomain.local
nameserver
nameserver
[root@exampleserver.local ~]#
```

## Step:2 Now Join Windows Domain or Integrate with AD using realm command

When we install above required packages then realm command will be available. We will use beneath realm command to integrate CentOS 7 or RHEL 7 with AD via the user “tech”. tech is a bind user which have required privileges on AD or  we can also administrator user of AD Server for integration purpose.

```bash
[root@exampleserver.local ~]# realm join --user=tech adserver.example.com
Password for tech:
[root@exampleserver.local ~]#
```

 Now verify whether our server has joined the Windows domain or not. Simply run the command ‘realm list‘

```bash
 [root@exampleserver.local ~]# realm list
example.com
  type: kerberos
  realm-name: EXAMPLE.COM
  domain-name: example.com
  configured: kerberos-member
  server-software: active-directory
  client-software: sssd
  required-package: oddjob
  required-package: oddjob-mkhomedir
  required-package: sssd
  required-package: adcli
  required-package: samba-common-tools
  login-formats: %U@example.com
  login-policy: allow-realm-logins
[root@exampleserver.local ~]#
```

 Whenever we run ‘realm join’ command it will automatically configure ‘/etc/sssd/sssd.conf‘ file.

## Step:3 Check and Verify  AD users on REHL 7 or CentOS 7 Servers

With ‘id‘ command on Linux we can verify the user’s uid and gid and their group information. At this point of time our server is now the part of windows domain. Use below command to verify AD users details.

```bash
[root@exampleserver.local ~]# id exampleaccount@exampledomain.local
uid=164579865(exampleaccount) gid=643728654(domain users) groups=643728654(domain users),743768654(organization management)
[root@exampleserver.local ~]#
```

You might have noticed in above command that i have mentioned domain name as well along with user name because this is controlled by ‘/etc/sssd/sssd.conf’ file. If we execute id command without domain name then we will not get any details for user

```bash
[root@exampleserver.local ~]# id exampleaccount
id: exampleaccount: no such user
[root@exampleserver.local ~]#
```

We can change this behavior by editing the file /etc/sssd/sssd.conf.

Change the following parameters from

```bash
use_fully_qualified_names = True
fallback_homedir = /home/%u@%d
```

to

```bash
use_fully_qualified_names = False
fallback_homedir = /home/%u
```

Restart the sssd service using following systemctl command

```bash
[root@exampleserver.local ~]# systemctl restart sssd
[root@exampleserver.local ~]# systemctl daemon-reload
```

Now run the id command and see whether you are able get AD user details without mentioning domain name

```bash
[root@exampleserver.local ~]# id exampleaccount@exampledomain.local
uid=164579865(exampleaccount) gid=643728654(domain users) groups=643728654(domain users),743768654(organization management)
[root@exampleserver.local ~]#
```

Let’s try ssh CentOS 7 or RHEL 7 Server with AD credentials

```bash
[root@exampleserver.local ~]# ssh exampleaccount@exampleserver.local
The authenticity of host 'exampleserver.local (192.168.10.241)' can't be established.
ECDSA key fingerprint is SHA256:.........
ECDSA key fingerprint is MD5:........
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'exampleserver.local,192.168.10.241' (ECDSA) to the list of known hosts.
exampleaccount@exampleserver.local's password:
Last login: Wed Jul 17 09:59:09 2019 from testcomputer.exampledomain.local
[exampleaccount@exampleserver.local ~]$
```

## Step:4 Sudo rights for AD users on CentOS 7 or RHEL 7

### This was not configured

In case you want to configure sudo rights for AD users then the best way is to create a group on AD with name sudoers and add Linux/UNIX users in that group and on Linux Server create a file with name “sudoers” under the folder /etc/sudoers.d/

Put the following content in the file.

```bash
[root@exampleserver.local ~]# cat /etc/sudoers.d/sudoers
%sudoers    ALL=(ALL)       ALL
[root@exampleserver.local ~]#
```

In my case I have given all the rights to the users which are part of sudoers group. Once your done with these changes re-login to your server with AD credentials and see whether user is part of sudoers group.
