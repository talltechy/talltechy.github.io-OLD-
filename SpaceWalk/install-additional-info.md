# Spacewalk Install Additional Info (unsorted)

[SpaceWalk: Home](./README)

/etc/pki/tls/private/spacewalk.key

ln -s /root/ssl/stardot_DOMAINNAME_TLD_fullchain.pem /etc/pki/tls/private/spacewalk.key

mv /etc/pki/tls/private/spacewalk.key /etc/pki/tls/private/spacewalk.key.self-signed
