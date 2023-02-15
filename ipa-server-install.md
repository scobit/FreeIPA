### Аргументы при установке

#### Полезные ссылки
https://linux.die.net/man/1/ipa-server-install
https://manpages.debian.org/unstable/freeipa-server/ipa-server-install.1.en.html#auto~2
https://www.freeipa.org/page/V3/CA-less_install


#### Шаблон запуска
```
ipa-server-install \
--setup-dns \
--no-ntp \ 
--forwarder=192.168.0.1 \
--auto-reverse \
--reverse-zone=0.168.192.in-addr.arpa. \
--allow-zone-overlap \
-r KRAKEN.KCELL.KZ \
-n kraken.kcell.kz \
-p Aa123456 \
-U \
-a Aa123456 \
--ip-address=192.168.0.150 \
--dirsrv-cert-file=kdc01.p12 \
--dirsrv-pin="hjgh&%^IGYJHph#$%Dv" \
--http-cert-file=kdc01.p12 \ 
--http-pin="hjgh&%^IGYJHph#$%Dv" \
--ca-cert-file=kcell_root_ca.crt \
--no-pkinit

ipa-server-install --setup-dns --no-ntp --forwarder=192.168.0.1 --forwarder=8.8.8.8 --auto-reverse --reverse-zone=0.168.192.in-addr.arpa. --allow-zone-overlap -r KRAKEN.KCELL.KZ -n kraken.kcell.kz -p Aa123456 -U -a Aa123456 --ip-address=192.168.0.150 --dirsrv-cert-file=kdc01.p12 --dirsrv-pin="hjgh&%^IGYJHph#$%Dv" --http-cert-file=kdc01.p12 --http-pin="hjgh&%^IGYJHph#$%Dv" --ca-cert-file=kcell_root_ca.crt --no-pkinit


```


#### Создание DNS зоны (если её нет) и конфигурация DNS сервера. Опция требует указания как минимум одного DNS forwarder через --forwarder Или указание их отсутсвия опцией --no-forwarders Создаёт DNS зону через опцию --domain и заполнит её служебными записями для работы IPA деплоя. DNS можно установить после установке сервера, с помощью ipa-dns-install
```
--setup-dns
```

#### Не устаналивать ntp сервер
```
--no-ntp
```

#### Добавление DNS forwarder.
```
--forwarder=IP_ADDRESS
```

#### Попробовать разрешить обратные записи и обратные зоны для IP-адресов серверов. Если нет, создание обратных зон.
```
--auto-reverse
```

#### Указание обратной DNS зоны, может использоваться несколько раз для создания различных зон
```
--reverse-zone=REVERSE_ZONE
--reverse-zone=213.168.192.in-addr.arpa.
```

#### Разрешить создание обратной зоны, даже если зона уже разрешена. Не рекомендуется, так как впоследствии это приведет к проблемам с разрешением доменного имени.
```
--allow-zone-overlap 
```
#### Kerberos REALM name для IPA сервера. Обязательно заглавными буквами, если условие будет нарушено, создание доверенных отношений с AD будет невозможно. Не может быть изменён после установки.
```
-r REALM_NAME
--realm=REALM_NAME
```

#### Указание доменного имени, к примеру example.com DNS домен будет содержать записи SRV, сгенерированные программой установки сервера IPA. Указанный домен DNS не должен содержать записей DNS любой другой системы управления на основе LDAP или Kerberos (например, Active Directory или MIT Kerberos). Настоятельно рекомендуется использовать имя области IPA Kerberos в нижнем регистре. Имя основного домена DNS нельзя изменить после установки.
```
-n DOMAIN_NAME
--domain=DOMAIN_NAME
```

#### Пароль, который будет использоваться Directory Server для пользователя Directory Manager.
```
-p DM_PASSWORD
--ds-password=DM_PASSWORD
```

#### Автоматическая установка, которая никогда не будет запрашивать ввод данных пользователем.
```
-U
--unattended
``` 
#### Пароль для пользователя admin IPA системы.
```
-a ADMIN_PASSWORD
--admin-password=ADMIN_PASSWORD
```

#### IP-адрес сервера. Если этот адрес не совпадает с адресом резолва хоста и --setup-dns не выбран, установка завершится ошибкой. Если имя хоста сервера не разрешается, запись для имени хоста и IP_ADDRESS добавляется в /etc/hosts. Эту опцию можно использовать несколько раз, чтобы указать больше IP-адресов сервера (например, многосетевого и/или двухстекового сервера)
```
--ip-address=IP_ADDRESS
```

#### Файл, содержащий SSL-сертификат Directory Server и закрытый ключ. Файлы принимаются в формате сертификата PEM и DER, цепочки сертификатов PKCS#7, PKCS#8 и необработанного закрытого ключа и форматов PKCS#12. Эту опцию можно использовать несколько раз.
```
--dirsrv-cert-file=FILE
```

#### Пароль для разблокировки закрытого ключа Directory Server.
```
--dirsrv-pin=PIN
```

#### Файл, содержащий SSL-сертификат Apache Server и закрытый ключ. Файлы принимаются в формате сертификата PEM и DER, цепочки сертификатов PKCS#7, PKCS#8 и необработанного закрытого ключа и форматов PKCS#12. Эту опцию можно использовать несколько раз.
```
--http-cert-file=FILE
```

#### Пароль для разблокировки закрытого ключа Apache Server.
```
--http-pin=PIN
```

#### Файл, содержащий сертификат центра сертификации, который выдал сертификаты Directory Server, Apache и Kerberos KDC. Файл принимается в форматах сертификатов PEM и DER и цепочки сертификатов PKCS#7. Эту опцию можно использовать несколько раз. Используйте этот параметр, если сертификат ЦС отсутствует в файлах сертификатов. Note: --root-ca-file was replaced with --ca-cert-file 
```
--ca-cert-file=FILE
```

#### Отключение этапов настройки pkinit.
```
--no-pkinit
```


## Опции для удаления

#### Удаление сервера
```
--uninstall
```
#### Удаление без запросов пользователю
```
-U 
--unattended
```
