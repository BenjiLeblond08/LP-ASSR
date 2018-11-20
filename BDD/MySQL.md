# MySQL WiKi

[TOC]

## Introduction

Nom du paquet : mysql-server

## Connect to MySQL from terminal

```
$ mysql -u root -p
```

## SQL Basics

```SQL
-- List databases
mysql> SHOW DATABASES;
-- Select a database to use
mysql> USE <db_name>;
-- List tables from the selected databases
mysql> SHOW TABLES;
-- Display fields (collumn names)
mysql> DESCRIBE <table_name>;
-- Get content of a table
mysql> SELECT <field_name_1>[,<field_name_2>...] 
	-> FROM <table_name> 
	-> WHERE <field_name> = "<value>"
	-> WHERE <field_name> IS NULL;
-- Get ALL content of a table
mysql> SELECT * FROM <table_name>;

```

Note:  
The syntax ```<table_name>``` can be replaced by syntax ```<db_name>.<table_name>```.


## Sauvegardes de bases de données

```
mysqldump --user="${USERNAME}" --password="${PASSWORD}" "${db}" | gzip > "/var/lib/mysql/backup/${db}_$(date +%Y-%m-%d).sql.gz"
```
See my [custom script](https://github.com/BenjiLeblond08/scripts/blob/master/MySQL/mysql_backup.sh)

## Utilisateurs et mot de passe

Création d'utilisateur
```SQL
mysql> CREATE USER 'lpasr'@'%';
-- OU
mysql> CREATE USER 'lpasr'@'%' IDENTIFIED BY 'password';
```
Syntaxe des noms de comptes :

```'user_name'@'host_name'```  
Les Wildcard ```'%'``` et ```'_'``` sont autorisés comme valeures de nom d'hote ou d'adresse IP  
Host_name : ```'198.51.100.% ```, ```'198.51.100.0/255.255.255.0'```, ```'::1'```  
Les masques ne peuvent etre spécifié que pour les adresses IPv4  

Definition du mot de passe
```SQL
mysql> ALTER USER 'lpasr'@'%' IDENTIFIED BY 'password';
-- OU
mysql> SET PASSWORD FOR 'lpasr'@'%' = PASSWORD('password');
```
Changer son propre mot de passe
```SQL
mysql> ALTER USER USER() IDENTIFIED BY 'password';
```

## Droits

Ajout de droits a un utilisateur
```SQL
mysql> GRANT ALL PRIVILEGES ON *.* TO 'lpasr'@'%';
```
ATTENTION, ici ont donne tous les droits sur toutes les bases
