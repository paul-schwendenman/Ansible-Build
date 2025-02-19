# PostgreSQL role

Deploy PostgreSQL database container.

## Usage

Configure the role.

**vars.yml**

```yml
# https://hub.docker.com/_/postgres
postgres_image: postgres:12-alpine
postgres_description: Database for website # default: PostgreSQL
postgres_hostname: postgres01
postgres_volume_name: postgres_data01 # default: "{{ postgres_hostname}}"
postgres_ports:
  - 127.0.0.1:5433:5432 # default: []
postgres_user: example
postgres_password: # default: "{{ vault_postgres_password }}"
postgres_db: example # default: "{{ postgres_user }}"
```

Backup databases.

```yml
postgres_backup_set: # See restic_backup_set var in role restic_client
  - id: "{{ postgres_hostname }} dump"
    type: postgres-dump
    container: "{{ postgres_hostname }}"
    tags:
      - postgres
      - "{{ postgres_hostname }}"
    hour: "1"
```

And include it in your playbook.

```yml
- hosts: postgres
  roles:
  - role: postgres
```

## Docs

### Install PostgreSQL scripts

The installation script requires that you have sudo access to root.

Run `curl -L https://raw.githubusercontent.com/mint-system/ansible-build/master/roles/postgres/files/install | sh` in your terminal.
