+++
title = "Upgrading"
description = "Upgrading Grafana guide"
keywords = ["grafana", "configuration", "documentation", "upgrade"]
type = "docs"
[menu.docs]
name = "Upgrading"
identifier = "upgrading"
parent = "installation"
weight = 10
+++

# Upgrading Grafana

We recommend everyone to upgrade Grafana often to stay up to date with the latest fixes and enhancements.
In order make this a reality Grafana upgrades are backward compatible and the upgrade process is simple & quick.

Upgrading is generally always safe (between many minor and one major version) and dashboards and graphs will look the same. There can be minor breaking changes in some edge cases which are usually outlined in the [Release Notes](https://community.grafana.com/c/releases) and [Changelog](https://github.com/grafana/grafana/blob/master/CHANGELOG.md)

## Database Backup

Before upgrading it can be a good idea to backup your Grafana database. This will ensure that you can always rollback to your previous version. During startup, Grafana will automatically migrate the database schema (if there are changes or new tables). Sometimes this can cause issues if you later want to downgrade.

#### sqlite

If you use sqlite you only need to make a backup of you `grafana.db` file. This is usually located at `/var/lib/grafana/grafana.db` on unix system.
If you are unsure what database you use and where it is stored check you grafana configuration file. If you
installed grafana to custom location using a binary tar/zip it is usally in `<grafana_install_dir>/data`.

#### mysql

```
backup:
> mysqldump -u root -p[root_password] [grafana] > grafana_backup.sql

restore:
> mysql -u root -p grafana < grafana_backup.sql
```

#### postgres

```
backup:
> pg_dump grafana > grafana_backup

restore:
> psql grafana < grafana_backup
```

### Ubuntu / Debian

If you installed grafana by downloading a debian package (`.deb`) you can just follow the same installation guide
and execute the same `dpkg -i` command but with the new package. It will upgrade your Grafana install.

If you used our APT repository:

```
sudo apt-get update
sudo apt-get install grafana
```

#### Upgrading from binary tar file

If you downloaded the binary tar package you can just download and extract a new package
and overwrite all your existing files. But this might overwrite your config changes. We
recommend you place your config changes in a file named  `<grafana_install_dir>/conf/custom.ini`
as this will make upgrades easier without risking losing your config changes.

### Centos / RHEL

If you installed grafana by downloading a rpm package you can just follow the same installation guide
and execute the same `yum install` or `rpm -i` command but with the new package. It will upgrade your Grafana install.

If you used our YUM repository:

```
sudo yum update grafana
```

### Docker

This just an example, details depend on how you configured your grafana container.
```
docker pull grafana
docker stop my-grafana-container
docker rm my-grafana-container
docker run --name=my-grafana-container --restart=always -v /var/lib/grafana:/var/lib/grafana
```

### Windows

If you downloaded the windows binary package you can just download a newer package and extract
to the same location (and overwrite the existing files). This might overwrite your config changes. We
recommend you place your config changes in a file named  `<grafana_install_dir>/conf/custom.ini`
as this will make upgrades easier without risking losing your config changes.

## Upgrading form 1.x

[Migrating from 1.x to 2.x]({{< relref "installation/migrating_to2.md" >}})

## Upgrading form 2.x

We are not aware of any issues upgrading directly from 2.x to 4.x but to on the safe side go via 3.x.
