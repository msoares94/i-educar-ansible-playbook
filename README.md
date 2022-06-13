>[In Portuguese](README_pt-BR.md)

# Install i-Educar with Ansible
Project to automate the installation of Public Software [i-Educar](https://ieducar.org).

> Project Status: Under development :warning:

### Table of Contents
    
   * [Project description](#project-description)
   * [Prerequisites](#prerequisites)
   * [Authentication mode on the target server](#authentication-mode-on-the-target-server)
   * [Running the playbook](#running-the-playbook)
   * [Role variables](#role-variables)
   * [Dependencies](#dependencies)
   * [License](#license)
   * [Author information](#author-information)
   * [Donate](#donate)

--------------

### Project description
Project to automate the installation of Public Software [i-Educar](https://ieducar.org).

--------------

### Prerequisites
-  The command cannot be executed on the installation target server as during the installation process the server is restarted.

--------------

### Authentication mode on the target server

- #### SSH keys

  SSH Generate Key
    - ```ssh-keygen -t rsa```

  Set key permission
    - ```chmod 400 ~/.ssh/id_rsa```

  Add key in server
    - ```ansible-playbook add-key.yml -i inventory --key-file ~/.ssh/id_rsa --extra-vars "key=~/.ssh/id_rsa.pub"```

- #### Password

    There are no steps to be performed!

--------------

### Running the playbook

- #### Server target with ssh keys
    ```ansible-playbook playbook.yml -i inventory --key-file ~/.ssh/id_rsa```

- #### Server target with password
    ```ansible-playbook playbook.yml -i inventory --ask-pass```

--------------

### Role variables

    # System
    system_locale: pt_BR.UTF-8
    system_language: pt_BR.UTF-8
    system_time_zone: America/Sao_Paulo

    # Composer
    composer_version: 2.2.11

    # PHP
    php_version: 7.4

    # i-Educar
    ieducar_version: 2.6.9

    # Postgresql
    postgresql_version: 13
    postgresql_encoding: 'UTF-8'
    postgresql_locale: 'pt_BR.UTF-8'
    postgresql_recreate_cluster: true

    # pg_hba.conf
        postgresql__pg_hba_entries:
        - { type: local, database: all, user: postgres, address: '', auth_method: peer }
        - { type: local, database: all, user: all, address: '', auth_method: peer }
        - { type: host, database: all, user: all, address: '127.0.0.1/32', auth_method: md5 }
        - { type: host, database: all, user: all, address: all, auth_method: md5 }
        - { type: host, database: all, user: all, address: '::1/128', auth_method: md5 }
        # starting version 10 there is replication role
        - { type: local, database: replication, user: all, address: '', auth_method: peer }
        - { type: host, database: replication, user: all, address: '127.0.0.1/32', auth_method: md5 }
        - { type: host, database: replication, user: all, address: '::1/128', auth_method: md5 }
    
    # postgresql.conf
        postgresql__global_conf_options:
        - option: listen_addresses
            value: '*'
        - option: log_min_duration_statement
            value: 1000
        - option: max_connections
            value: 250
        - option: shared_buffers
            value: 256MB
        - option: effective_cache_size
            value: 768MB
        - option: maintenance_work_mem
            value: 64MB
        - option: checkpoint_completion_target
            value: 0.9
        - option: wal_buffers
            value: 7864kB
        - option: default_statistics_target
            value: 100
        - option: random_page_cost
            value: 1.1
        - option: effective_io_concurrency
            value: 200
        - option: work_mem
            value: 524kB
        - option: min_wal_size
            value: 1GB
        - option: max_wal_size
            value: 4GB

Dependencies
------------

   - Ansible Core 2.12.4
   - Python 3.8.10

License
-------
[GNU GENERAL PUBLIC LICENSE v3](LICENSE)

Author information
------------------

[Marcos Oliveira Soares](https://github.com/marcosoliveirasoares94)

Donate
------------------
Help keep this project going!
>
Key PIX: 1d60a324-d92f-400e-b3df-40a427288b4b