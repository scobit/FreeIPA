#### generate keytab
```
ipa-getkeytab -s KDC_HOSTNAME_OR_IP -p PrincipanName@DOMAIN.DOMAIN.DO -k /DestinationPath
```

#### Обновление сертификата на клиенте с сервера, по пути /etc/ipa/ca.crt
#### Для выполнения команды требуются права root и kinit
ipa-certupdate
