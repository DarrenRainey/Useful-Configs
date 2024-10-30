# Basic configuration for authentication with LDAP via SSSD

```
File: /etc/sssd/sssd.conf
Note: Remove ldap_auth_disable_tls_never_use_in_production in a production enviroment with you have proper SSL certificates setup
Tested against: FreeIPA
```
```
[sssd]
config_file_version = 2
domains = lab.net

[domain/lab.net]
override_shell = /bin/bash
id_provider = ldap
auth_provider = ldap
ldap_uri = ldap://ldap.lab.net
cache_credentials = True
ldap_search_base = dc=lab,dc=net
ldap_auth_disable_tls_never_use_in_production = true
```
