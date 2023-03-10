### Пример настройки одной ноды + samba + клиент (Centos 7)

### Теория

Kerberos очень требователен к времени, расхождение времени не должно превышать 5 минут.

При аутентификации и авторизации через kerberos используется зашифрованное время.

Т.е. в ключе есть зашифрованное время - т.к. пароль шифрует текущее время и если время расходится больше чем на 5 минут, то ничего работать не будет.

Новогодняя песня про 5 минут - про kerberos =)

FreeIPA server and FreeIPA dns принято ставить на одной машине, но не запрещается ставить на разных.

DNS нужен для указания адресов каки-либо служб - аналогично в Active Directory.

Классика жанра - домен контролер и DNS в одном флаконе.

REALM name - domain name в профиль для KERBEROS.

FreeIPA может работать с Windows, но плохо (нужно создавать вручную пользователей с UID).

Samba DC - является гетерогенным, для Windows лучше использовать его.

kinit - kerberos init - приложение для аутентификации на сервере kerberos.

Срок действия билета Kerberos можно изменить.

kinit работает не с доменом, а с REALM, т.е. если вводим дополнительные данные, то используем заглавные буквы. admin@MYDOMAIN.LOCAL

hostname клиента желательно должен быть в формате FQDN.

Все сервисы, которые поддерживают Pam, теоретически могут работать с FreeIPA.

## FreeIPA Server node

#### Обновляем пакеты
```
yum -y update
```

#### Проверка статуса selinux
```
sestatus
```

#### Отключение selinux
```
vim /etc/sysconfig/selinux

SELINUX=disabled
```

#### Проверка, остановка, отключение firewall
```
systemctl stop firewalld

systemctl disable firewalld

systemctl status firewalld
```

#### Синхронизация времени через chrony 
```
yum install chrony

systemctl enable chronyd

systemctl start chronyd

date
```

#### Синхронизация времени через ntpdate (для автоматизации синхронизации, команду синхронизации можно установить в cron)
```
yum install ntpdate

ntpdate ru.pool.ntp.org
```

#### Установка hostname
```
hostname

hostnamectl set-hostname fqdn

reset
```

#### Установка FreeIPA server and FreeIPA dns
```
yum -y install ipa-server ipa-server-dns

```

#### Запсук мастера установки
```
ipa-server-install
```

#### Проверка Kerberos
```
kinit admin
```

#### Проверка текущих билетов
```
klist
```

#### Установка компонентов для связи с другими доменами
```
yum -y install ipa-server-trust-ad
```

#### Добавление компонента Trust во FreeIPA server
```
ipa-adtrust-install --add-sids
```

#### Добавляем сервис типа cifs с hostname, где расположен Samba server
```
ipa service-add cifs/hostname.doamin.local
```


## Настройки на сервере Samba

#### Обновление пакетов
```
yum -y update
```
#### Проверка статуса selinux
```
sestatus
```

#### Отключение selinux
```
vim /etc/sysconfig/selinux

SELINUX=disabled
```

#### Проверка, остановка, отключение firewall
```
systemctl stop firewalld

systemctl disable firewalld

systemctl status firewalld
```

#### Установка hostname
```
hostname

hostnamectl set-hostname fqdn

reset
```

#### В качестве nameserver указываем IP FreeIPA dns server.
echo "nameserver FreeIPA_IP" > /etc/resolv.conf

#### Установка Freeipa client
```
yum -y install freeipa-client
```

#### Запуск мастера настройки, для создания домашнего каталога при логине пользователя используется аргумент --mkhomedir
```
ipa-client-install --mkhomedir
```

#### Добавление пользователя через консоль (предварительно нужно провести kinit). 
#### где first - имя, last - фамилия, password аргумент, который запросит пароль для пользователя. По умолчанию пароль на задаётся.
```
ipa user-add Name --first=FirstName --last=LastName --password
```

#### Установка ПО для Samba
```
yum -y install ipa-client sssd-libwbclient samba samba-client
```

#### Бэкап текущей конфигурации Samba
```
cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
```

#### Редактируем конфиг Samba
```
vim /etc/samba/smb.conf

[global]
	workgroup = QWE
	realm = QWE.LOCAL
	dedicated keytab file = FILE:/etc/samba/samba.keytab
	kerberos method = dedicated keytab
	log file = /var/log/samba/log.%m
	security = user
	dns proxy = No
	passdb backend = tdbsam
	vfs objects = acl_xattr
	map acl inherit = yes
	store dos attributes = yes
	allow dns updates = nonsecure
	client min protocol = NT1
	client max protocol = SMB2
	winbind rpc only = Yes
	client use spnego = no
	client ntlmv2 auth = no
	
[homes]
	comment = Home Directories
	valid users = %S, %D%w%S
	browseable = No
	read only = No
	inherit acls = Yes
[share]
	path = /share
	writable = yes
	browsable = yes
	
```

#### Создаём каталог для share и настраиваем права
```
mkdir /share

chmod -R 777 /share
```

#### Запрашиваем keytab с сервера FreeIPA
```
ipa-getkeytab -s c2.qwe.local -p cifs/c3.qwe.local -k /etc/samba/samba.keytab
```

#### Перезагружаем сервисы Samba
```
systemctl restart smb

systemctl restart nmb
```


## Действия на Samba client
yum -y install ipa-client sssd-libwbclient samba-client

vim /etc/krb5.conf

в раздел libdefaults

default_ccache_name = FILE:/tmp/krb5cc_%{uid}

при повторном kinit (для той же учётки) тикет обновляется а не дублируется

kinit UserName

smbclient -k //samba.mkm.local/share



