#### Аргументы при установке
```
# Создание DNS зоны (если её нет) и конфигурация DNS сервера. Опция требует указания как минимум одного DNS forwarder через --forwarder
# Или указание их отсутсвия опцией --no-forwarders
# Создаёт DNS зону при
# DNS можно установить после установке сервера, с помощью ipa-dns-install
--setup-dns

####

```
create DNS zone specified by --domain, and fill it with service records necessary for IPA deployment. In cases where the IPA server name does not belong to the primary DNS domain and is not resolvable using DNS, create a DNS zone containing the IPA server name as well.
This option requires that you either specify at least one DNS forwarder through the --forwarder option or use the --no-forwarders option.

Note that you can set up a DNS at any time after the initial IPA server install by running ipa-dns-install (see ipa-dns-install(1)). IPA DNS cannot be uninstalled.
