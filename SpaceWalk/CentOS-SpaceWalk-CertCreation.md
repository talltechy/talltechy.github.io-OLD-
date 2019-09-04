# Steps to creating Certs

## Overview

The Spacewalk Server provides all services to customers through the setting
of entitlements. The entitlements available through Spacewalk are determined
by the Entitlement Certificate which contains the precise set of entitlements
attributed to your organization. The entitlement certificiate is encryped
with a GPG key.

For Red Hat supported channels, the certificiate is encryped with Red Hat's
security GPG key.

It is possible to create your own GPG key and certificate for private non Red
Hat channels. This process involves creating a GPG key and signing a
certificate for use with Spacewalk.

The GPG signed certificate is used to activate the Spacewalk using
`rhn-satellite-activate`. A certificate template and signing script is
included for your use.

## Generate GPG keys

First create a GPG key

```console
gpg --gen-key
```

Then see if your key appears in the output of the following commands. For example, we generated a key for Scooby below

```console
[root@exampleserver ~]# gpg --list-keys
/root/.gnupg/pubring.gpg
------------------------
pub   1024D/F24F1B08 2002-04-23 [expired: 2004-04-22]
uid                  Red Hat, Inc (Red Hat Network) <rhn-feedback@redhat.com>

pub   2048R/47A979DA 2019-07-18
uid                  user-example <user-example@exampledomain.local>
sub   2048R/C439F341 2019-07-18

[root@exampleserver ~]# gpg --list-secret-keys
/root/.gnupg/secring.gpg
------------------------
sec   2048R/47A979DA 2019-07-18
uid                  user-example <user-example@exampledomain.local>
ssb   2048R/C439F341 2019-07-18
```

Finally, export the public and secret keys.

```console
[root@exampleserver ~]# gpg --export -a 47A979DA > swcertkey.gpg
[root@exampleserver ~]# gpg --export-secret-keys -a 47A979DA > swsecretkey.gpg
```

## Install GPG keys into webapp

Set the `web.gpg_keyring` in `/usr/share/rhn/config-defaults/rhn_web.conf` to
your newly exported keyring.

```console
[root@exampleserver ~]# vi /usr/share/rhn/config-defaults/rhn_web.conf
```

Alternatively, you can add your keys to the `/etc/webapp-keyring.gpg`
keyring.  Here are some commands to help you accomplish this:

```console
[root@exampleserver ~]# gpg --keyring /etc/webapp-keyring.gpg --no-default-keyring --import swcertkey.gpg swsecretkey.gpg
gpg: key 47A979DA: public key "user-example <user-example@exampledomain.local>" imported
gpg: key 47A979DA: already in secret keyring
gpg: Total number processed: 2
gpg:               imported: 1  (RSA: 1)
gpg:       secret keys read: 1
gpg:  secret keys unchanged: 1
gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
```

To see the keys in this keyring, use the following command:

```console
[root@exampleserver ~]# gpg --keyring /etc/webapp-keyring.gpg --no-default-keyring --list-keys
/etc/webapp-keyring.gpg
-----------------------
pub   1024D/06947932 2004-02-18 [expires: 2023-02-05]
uid                  Red Hat Network (Satellite Certificate Signing Key) <rhn-feedback@redhat.com>
sub   2048g/C71F2F5C 2004-02-18 [expires: 2023-02-05]

pub   1024D/F8116329 2012-11-16 [expires: 2022-11-14]
uid                  Red Hat Network (Development Satellite Certificate Signing Key) <ams-list@redhat.com>
sub   2048g/A99575E6 2012-11-16 [expires: 2022-11-14]

pub   2048R/47A979DA 2019-07-18
uid                  user-example <user-example@exampledomain.local>
sub   2048R/C439F341 2019-07-18
```

NOTE: The `--no-default-keyring` option excludes the user's keyrings (usually `~/.gnupg/*.gpg`) from the operation, otherwise they would be included as the `--keyring` option merely adds the specified keyring to the list of default ones.

## Signing the Certificate

Download the [`https://github.com/talltechy/talltechy.github.io/blob/master/SpaceWalk/scripts/gen-oss-sat-cert.pl`](https://github.com/talltechy/talltechy.github.io/blob/master/SpaceWalk/scripts/gen-oss-sat-cert.pl) script to your Spacewalk.

Modify the `spacewalk-public.cert` or create a new certifcate to sign

Review the usage statement from `scripts_gen-oss-sat-cert.pl`:

```console
Usage: ./scripts_gen-oss-sat-cert.pl --orgid <org_id> --owner <owner_name> --signer <signer> --no-pass-phrase --output <dest> --expires <when> --slots <num> [ --provisioning-slots <num> ] [ --channel-family label=n ] [ --satellite-version X.Y ] [ --resign ]
```

Make sure you import your public and secret keys as the user you are running the script as

```console
gpg --import mycertkey.gpg
gpg --import mysecretkey.gpg
```

Resign/sign the cert, for example:

```console
perl scripts_gen-oss-sat-cert.pl --signer 74E73973 --resign template-eval.cert
```

## Activating the Spacewalk locally (optional)

If you came to this page from the HowToInstall page, activation will occur
during `spacewalk-setup`, so you can safely ignore this section. Otherwise,
continue reading.

You should use the options below to accomplish the following tasks in this order:

Validate the Entitlement Certificate's sanity (or usefulness).
Activate the Spacewalk locally by inserting the Entitlement Certificate into the local database.

Here are some examples depicting use of the tool and these options.

To validate an Entitlement Certificate's sanity only:

```console
rhn-satellite-activate --sanity-only --rhn-cert=/path/to/demo.cert
```

To validate an Entitlement Certificate and populate the local database:

```console
rhn-satellite-activate --disconnected --rhn-cert=/path/to/demo.cert
```

Once you run this final command, the Spacewalk is running and able to serve packages locally.
