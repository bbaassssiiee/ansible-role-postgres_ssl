# postgres
The postgres role will install Postgresql software and configure databases and users.
PostgreSQL will run with SSL.

## Role Variables

postgres_enabled: true


### Postgresql users and databases/schemas
```yaml
database:
  postgres:
    name: postgres
    owner: postgres
    username: postgres
    password: "{{ lookup('env', 'DEV_PASS') }}"
    enabled: true
```

By default, the [_pg_hba.conf_](https://www.postgresql.org/docs/15/auth-pg-hba-conf.html) client authentication file is configured for open access for development purposes through the _postgres_allowed_hosts_ variable:

```
postgres_allowed_hosts:
  - { type: "host", database: "all", user: "all", address: "127.0.0.1/0", method: "trust"}
```

## Example Playbook
```
---
- hosts: database
  collections:
    - community.postgresql
    - community.crypto
    - community.general
  roles:
    - postgres
```
