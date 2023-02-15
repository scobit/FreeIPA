
https://manpages.debian.org/experimental/freeipa-server/ipa-replica-install.1.en.html

ipa-replica-install \
--setup-dns \
--no-ntp \
--no-forwarders \ 
--auto-reverse \
--reverse-zone=0.168.192.in-addr.arpa. \
--allow-zone-overlap \
--server=kdc01.kraken.kcell.kz \
--principal admin \
--admin-password "Aa123456" \
-r KRAKEN.KCELL.KZ \
-n kraken.kcell.kz \
--ip-address=192.168.0.52 \
--dirsrv-cert-file=kdc02.p12 \
--dirsrv-pin="bvjhfR%^&$EFUps" \
--http-cert-file=kdc02.p12 \ 
--http-pin="bvjhfR%^&$EFUps" \
--ca-cert-file=kcell_root_ca.crt \
--no-pkinit

ipa-replica-install --setup-dns --no-ntp --no-forwarders \--auto-reverse --reverse-zone=0.168.192.in-addr.arpa. --allow-zone-overlap --server=kdc01.kraken.kcell.kz --principal admin --admin-password "Aa123456" -r KRAKEN.KCELL.KZ -n kraken.kcell.kz --ip-address=192.168.0.52 --dirsrv-cert-file=kdc02.p12 --dirsrv-pin="bvjhfR%^&$EFUps" --http-cert-file=kdc02.p12 --http-pin="bvjhfR%^&$EFUps" --ca-cert-file=kcell_root_ca.crt --no-pkinit
