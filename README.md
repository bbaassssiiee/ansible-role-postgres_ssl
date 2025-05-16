# postgres_ssl
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
    password: "{{ lookup('env', 'DB_PASS') }}"
    enabled: true
```

By default, the [_pg_hba.conf_](https://www.postgresql.org/docs/15/auth-pg-hba-conf.html) client authentication file is configured for open access for development purposes through the _postgres_allowed_hosts_ variable:

```yaml
# Set the hosts that can access the database
# The first allows SSL with password from the same subnet
# The second does not require SSL
postgres_allowed_hosts:
  - {
    type: "hostssl",
    database: "all",
    user: "all",
    address: "samenet",
    method: "password",
  }
  - {
    type: "host",
    database: "all",
    user: "all",
    address: "127.0.0.1/0",
    method: "password"
  }
  - {
    type: "hostnossl",
    database: "all",
    user: "all",
    address: "0.0.0.0/0",
    method: "reject",
  }
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
