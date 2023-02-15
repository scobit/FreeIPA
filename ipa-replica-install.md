
https://manpages.debian.org/experimental/freeipa-server/ipa-replica-install.1.en.html

ipa-replica-install 

--setup-dns 
--no-ntp 
--no-forwarders 
--auto-reverse 
--reverse-zone={{reverse_zone}} 
--allow-zone-overlap 
--server={{ hostvars[groups['kdc-master'][0]]['ansible_hostname'] }}.{{freeipa_domain}} 
--principal admin 
--admin-password {{ kerberos_admin_password }} 
--setup-dns 
--no-ntp 
--no-forwarders 
--allow-zone-overlap 
-r {{ freeipa_domain | upper }} 
-n {{ freeipa_domain }} 
--ip-address {{ansible_default_ipv4.address}} 
--dirsrv-cert-file /etc/pki/CA/ia.p12 
--dirsrv-pin {{ssl_cert_pass}} 
--http-cert-file /etc/pki/CA/ia.p12 
--http-pin {{ssl_cert_pass}} 
--no-pkinit
