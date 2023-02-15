The naming convention for full backups is ipa-full-YEAR-MM-DD-HH-MM-SS in the GMT time zone.

The naming convention for data backups is ipa-data-YEAR-MM-DD-HH-MM-SS In the GMT time zone.

#### Место хранения бэкапов по умолчанию
```
/var/lib/ipa/backup
```

#### Бэкап только данных (без перезагрузки сервисов)
```
ipa-backup --data --online
```

