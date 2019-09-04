# Unsorted General Notes

## Uninstalling a RPM Package

You can use either the rpm or yum command to remove RPM packages. Note that removing a package does not damage the Advanced Server data directory

### Uninstalling a RPM Package with RPM

Include the -e option on the rpm command to remove installed packages; the command syntax is:

```shell
rpm -e package_name [package_name…]
```

Where package_name is the name of the package that you would like to remove. The package name is the name of the .rpm file used when installing the package, with the version number and file extension (.rpm) removed; for example, the command:

```shell
rpm -e edb-as96
```

Removes the package installed with the command:

```shell
rpm -i edb-as96-9.6-x.rhel6.rpm
```

To instruct rpm to remove multiple packages, provide a list of packages you wish to remove when invoking the command.

### Uninstalling a RPM Package with YUM

You can use the yum remove command to remove an RPM package. To remove a package, open a terminal window, assume superuser privileges, and enter the command:

```shell
yum remove package_name
```

Where package_name is the name of the package that you would like to remove. The package name is the name of the .rpm file used when installing the package, with the file extension (.rpm) removed; for example, the command:

```shell
yum remove edb-as96
```

Removes the package installed with the command:

```shell
yum install edb-as96-9.6.x.x.rhel6.rpm
```

Note: yum and RPM will not remove a package that is required by another package. If you attempt to remove a package that satisfies a package dependency, yum or RPM will provide a warning.

## Tricks and Tips

### Determining Your Current Directory with PWD

As you browse directories, it is easy to get lost or forget the name of your current directory. By default, the Bash prompt in CentOS shows only your current directory, not the entire path. For example, the path to the application gedit is /usr/bin/gedit. The path to user someone's home directory is /home/someone/. Refer to Section the section about Paths for more information on paths.

The Command pwd Displays Your Location

To display the location of your current working directory, enter the command **pwd**.

The output should look similar to:

```shell
[root@spacewalkserver reposync]# pwd
/var/log/rhn/reposync
```

### A "live" view of a logfile on Linux

This approach works for any linux operating system, including Ubuntu, and is probably most often used in conjunction with web development work.

```shell
tail -f /path/thefile.log
```

This will give you a scrolling view of the logfile. As new lines are added to the end, they will show up in your console screen.

For Ruby on Rails, for instance, you can view the development logfile by running the command from your project directory:

```shell
tail -f log/development.log
```

As with all linux apps, Ctrl+C will stop it.

### Next Section
