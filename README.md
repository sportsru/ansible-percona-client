## percona-client 

[![Build Status](https://travis-ci.org/Oefenweb/ansible-percona-client.svg?branch=master)](https://travis-ci.org/Oefenweb/ansible-percona-client) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-percona--client-blue.svg)](https://galaxy.ansible.com/Oefenweb/percona-client)

Set up a [percona-server](https://www.percona.com/software/mysql-database/percona-server) client in Debian-like systems.

#### Requirements

None

#### Variables

* `percona_client_version`: [default: `8.0.29-21-1`]: Full package version to install, without the distribution suffix (e.g. `8.0.39-30-1`, `8.4.11-11-1`). Its major version (`8.0`, `8.4`) selects the repository
* `percona_client_repository_url`: [default: `http://repo.percona.com`]: Base URL of the Percona repositories (e.g. a local mirror)
* `percona_client_repository_names_map`: [default: see `defaults/main.yml`]: Repositories (`<url>/<name>/apt`) per major version; adding a key adds a supported major version. Keep in line with `percona_server_repository_names_map` of the percona-server role
* `percona_client_repository_remove_others`: [default: `true`]: Whether or not to remove repositories of the other major versions (e.g. `ps-80` when installing `8.4`)
* `percona_client_repository_keyring` / `percona_client_repository_key_id`: [default: see `defaults/main.yml`]: Repositories are added as `deb [signed-by=<keyring>] <url>/<name>/apt <codename> main` (the same lines percona-release and the percona-server role write); other lines of the same repositories are removed first
* `percona_client_hold`: [default: `true`]: Hold `percona-server-client` and `percona-server-common`, so that `apt upgrade` does not change them; they are unheld automatically when their version changes. On percona-server hosts keep `percona_client_version` equal to `percona_server_version`
* `percona_client_install`: [default: `[]`]: Additional packages to install

* `percona_client_my_cnf_files`: [default: `[]`]: `.my.cnf` files to configure
* `percona_client_my_cnf_files.{n}.dest`: [optional, default: `~owner/.my.cnf'`]: The remote path of the file to copy
* `percona_client_my_cnf_files.{n}.owner`: [required]: The name of the user that should own the file
* `percona_client_my_cnf_files.{n}.group`: [optional, default: `owner`]: The name of the group that should own the file
* `percona_client_my_cnf_files.{n}.mode`: [optional, default: `0600`]: The mode of the file
* `percona_client_my_cnf_files.{n}.login_host`: [optional, default: `localhost`]: The host running the server
* `percona_client_my_cnf_files.{n}.login_port`: [optional, default: `3306`]: The port of the server
* `percona_client_my_cnf_files.{n}.login_user`: [optional, default: `owner`]: The username used to authenticate with
* `percona_client_my_cnf_files.{n}.login_password`: [required]: The password used to authenticate with

* `percona_client_my_cnf_files.{n}.ssl`: [optional]: Whether or not to use SSL when connection

* `percona_client_my_cnf_files.{n}.ssl_ca`: [optional, default: `ca-cert`]: The identifier of the ca certificate file in ssl map
* `percona_client_my_cnf_files.{n}.ssl_cert`: [optional, default: `client-cert`]: The identifier of the ssl certificate file in ssl map
* `percona_client_my_cnf_files.{n}.ssl_key`: [optional, default: `client-key`]: The identifier of the ssl key file in ssl map

* `percona_client_ssl_map`: [default: `{}`]: SSL declarations
* `percona_client_ssl_map.key`: [required]: The identifier of the file (e.g. `ca-cert`)
* `percona_client_ssl_map.key.src`: [required]: The local path of the file to copy, can be absolute or relative (e.g. `../../../files/percona-client/etc/mysql/ca-cert.pem`)
* `percona_client_ssl_map.key.dest`: [required]: The remote path of the file to copy (e.g. `/etc/mysql/ca-cert.pem`)
* `percona_client_ssl_map.key.owner`: [optional, default `root`]: The name of the user that should own the file
* `percona_client_ssl_map.key.group`: [optional, default `mysql`]:The name of the group that should own the file
* `percona_client_ssl_map.key.mode`: [optional, default `0640`]: The mode of the file

## Dependencies

None

## Recommended

* `percona-server` ([see](https://github.com/Oefenweb/ansible-percona-server))

#### Example(s)

##### Simple

```yaml
---
- hosts: all
  roles:
    - percona-client
```

##### With .my.cnf file(s)

```yaml
---
- hosts: all
  roles:
    - percona-client
  vars:
    percona_client_my_cnf_files:
      - dest: '~root/.my.cnf'
        owner: root
        group: root
        mode: '0600'
        login_host: localhost
        login_port: 3306
        login_user: root
        login_password: 'pw4Root'

      - owner: vagrant
        login_password: 'pw4Vagrant'
```

#### License

MIT

#### Author Information

Mischa ter Smitten

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-percona-client/issues)!
