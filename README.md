# Ansible Role: NoCodeDB

Ansible role for installing and configuring NoCodeDB, a no-code database management platform.

## Requirements

Eventually setup an ansible vault to store `vault_nocodb_db_password` if you plan to use postgres or mariadb-backend 

## Role Variables

Available variables are listed below, along with their default values (located in `defaults/main.yml`):

| Variable                       | Default Value                  | Description                             |
| ------------------------------ | ------------------------------ | --------------------------------------- |
| `nocodb_dir.root`               | `/opt/nocodb`                        | Location of nocodb root directory    |
| `nocodb_dir.data`          | `/opt/nocodb/data`  | Location of nocodb data directory   |
| `nocodb_dir.conf`          | `/opt/nocodb/conf`  | Location of nocodb configuration directory   |
| `nocodb_dir.bin`          | `/opt/nocodb/bin`  | Location of nocodb binary directory   |
| `nocodb_user.name`           | `nocodb`                  | System user name use to run nocodb service |
| `nocodb_user.group`           | `nocodb`                  | System user group use to run nocodb service |
| `nocodb_config`        | `see defaults/main.yml`                        | Dictionnary to declare nocodb environnement variable according to [nocodb documentation](https://docs.nocodb.com/getting-started/environment-variables/)|
| `nocodb_backend`        | `sqlite`                        | The database backend to use. Possible value sqlite, mariadb, pg (postgres sql) |
| `nocodb_db.name`         | ``                          | Name of the database to use     |
| `nocodb_db.user`         | ``                          | Name of the user to access the database     |
| `nocodb_db.port`         | ``                          | Port number to access db backend (not needed for sqlite)    |
| `nocodb_db_url`    | ``                    | Url to access db backend (not needed for sqlite) |
| `vault_nocodb_db_password`       | ``                        | Password to access db backend /!\ you MUST store this variable into an ansible vault        |
| `postgresql_databases`       | `see vars/main.yml`             | Use for postgresql role dependency, custom this variable if using postgres backend     |
| `postgresql_users`       | `see vars/main.yml`             | Use for postgresql role dependency, custom this variable if using postgres backend     |
| `postgresql_unix_socket_directories`       | `see vars/main.yml`             | Use for postgresql role dependency, custom this variable if using postgres backend     |

## Dependencies

role: geerlingguy.redis
role: geerlingguy.postgresql

## Example Playbook

```yaml
- hosts: servers
  vars:
    postgresql_global_config_options:
      - option: data_directory
        value: '/var/lib/postgresql/15/main'
      - option: log_directory
        value: '/var/log/postgresql/'
    nocodb_config: 
      NC_PUBLIC_URL: "http://nocodb.example.com"
      PORT: 8080
      NC_DISABLE_TELE: true
      NC_REDIS_URL: "redis://127.0.0.1:6379"
      NC_SMTP_FROM: support@example.com
      NC_SMTP_HOST: mail.example.com
      NC_SMTP_PORT: 465
      NC_SMTP_USERNAME: support@example.com
      NC_SMTP_PASSWORD: "somepassword"
      NC_SMTP_SECURE: true



    nocodb_backend: "pg"
    nocodb_db: {
      name: "nocodb",
      user: "nocodb",
      port: 5432
    }
    nocodb_db_url: "pg://localhost:{{ nocodb_db.port }}?user={{ nocodb_db.user }}&password={{ vault_nocodb_db_password }}&d={{ nocodb_db.name }}"

  roles:
    - chadek.nocodb
```

## License

MIT

## Author Information

This role was created by chadek.
