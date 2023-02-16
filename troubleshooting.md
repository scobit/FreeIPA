#### Для работы FreeIPA необходимо, чтобы ОС поддерживала IPv6. Если отключить поддержку в ядре, при установке получим следующую ошибку.

ipapython.admintool: ERROR    IPv6 stack has to be enabled in the kernel and some interface has to have ::1 address assigned. Typically this is 'lo' interface. If you do not wish to use IPv6 globally, disable it on the specific interfaces in sysctl.conf except 'lo' interface.
ipapython.admintool: ERROR    The ipa-server-install command failed. See /var/log/ipaserver-install.log for more information

#### При установке FreeIPA, чтобы обойти эту ошибку, необходимо чётко указать ядру, что lo интерфейс использует IPv6. Дополнительно можно убрать IPv6 для основного интерфейса.

ipapython.admintool: ERROR    IPv6 stack is enabled in the kernel but there is no interface that has ::1 address assigned. Add ::1 address resolution to 'lo' interface. You might need to enable IPv6 on the interface 'lo' in sysctl.conf.
ipapython.admintool: ERROR    The ipa-server-install command failed. See /var/log/ipaserver-install.log for more information

```
vim /etc/sysctl.conf

net.ipv6.conf.lo.disable_ipv6=0
net.ipv6.conf.TypeInterfaceNameHere.disable_ipv6=1
```
```
sysctl -p 
```
