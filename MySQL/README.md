# MySQL WiKi

## Connect to MySQL from terminal

```
$ mysql -u root -p
```

## List Databases

```SQL
SHOW DATABASES;
```

## Sauvegardes de bases de données

```
mysqldump --user="${USERNAME}" --password="${PASSWORD}" "${db}" | gzip > "/var/lib/mysql/backup/${db}_$(date +%Y-%m-%d).sql.gz"
```
See my [custom script](https://github.com/BenjiLeblond08/scripts/blob/master/MySQL/mysql_backup.sh)
