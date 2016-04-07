# MariaDB 10.1 on CentOS 6 using puppet/hiera

## Using puppetlabs/mysql module

Below is a snippet from my hiera config.  You'll need to adjust the module names for your local hiera-tized defined types.  There's the rest of the config that will need to go in for your local instance as well (apache, more mysql stuff, etc...).

```
classes:
  - mysql::server

#
# MySQL server
#
mysql::server::package_name: 'MariaDB-server' # For MariaDB
mysql::server::package_manage: false # For MariaDB
mysql::server::service_name: 'mysql' # For MariaDB

#
# Files
#
system::files:
  /var/run/mysqld: # For MariaDB
    ensure: directory
    owner: 'mysql'
    group: 'mysql'
    require: 'Package[MariaDB-server]'

#
# Packages
#
system::packages:
  MariaDB-client:
    ensure: present
  MariaDB-server:
    ensure: present
  mysql-libs: # conflict with MariaDB
    ensure: absent
    provider: 'rpm'
    uninstall_options: '--nodeps'

#
# Yum repos
#
system::yumrepos:
  'mariadb':
    descr: 'MariaDB 10.1 CentOS repository'
    gpgkey: 'https://yum.mariadb.org/RPM-GPG-KEY-MariaDB'
    baseurl: 'http://yum.mariadb.org/10.1/centos6-amd64'
```
