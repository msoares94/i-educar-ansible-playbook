# Instale o i-Educar com o Ansible
Projeto para automatizar a instalação de Software Público [i-Educar](https://ieducar.org).

> Status do projeto: Em desenvolvimento :warning:

### Índice
    
   * [Descrição do projeto](#descrição-do-projeto)
   * [Pré-requisitos](#pré-requisitos)
   * [Dependências](#dependências)
   * [Modo de autenticação no servidor de destino](#modo-de-autenticação-no-servidor-de-destino)
   * [Executando o manual](#executando-o-manual)
   * [Variáveis de função](#variáveis-de-função)
   * [Licença](#licença)

--------------

### Descrição do projeto
Projeto para automatizar a instalação de Software Público [i-Educar](https://ieducar.org).

--------------

### Pré-requisitos
-  O comando não pode ser executado no servidor de destino da instalação, pois durante o processo de instalação o servidor é reiniciado.

- O arquivo `inventory.example` deve ser renomeado para `inventory`

--------------

### Dependências
------------

   - Ansible ^2.10
        
            Alguns módulos fazem parte das coleções:
                ansible.posix
                community.general
                community.postgresql

            Talvez você já tenha esta coleção instalada se estiver usando o pacote ansible, não está incluído em ansible-core.
            
            Para verificar os módulos instalados, execute ansible-galaxy collection list

            Para instalá-lo, execute:
                ansible-galaxy collection install ansible.posix
                ansible-galaxy collection install community.general
                ansible-galaxy collection install community.postgresql
            
   - Python ^3.8

--------------

### Modo de autenticação no servidor de destino

- #### Chaves SSH

  Gerar chave SSH
    - ```ssh-keygen -t rsa```

  Definir permissão de chave
    - ```chmod 400 ~/.ssh/id_rsa```

  Adicionar chave no servidor
    - ```ansible-playbook add-key.yml -i inventory --key-file ~/.ssh/id_rsa --extra-vars "key=~/.ssh/id_rsa.pub"```

- #### Senha

    Não há etapas a serem executadas!

--------------

### Executando o manual

- #### Servidor de destino com chaves ssh
    ```ansible-playbook playbook.yml -i inventory --key-file ~/.ssh/id_rsa```

- #### Servidor de destino com senha
    ```ansible-playbook playbook.yml -i inventory --ask-pass```

--------------

### Variáveis de função

    # System
    system_locale: pt_BR.UTF-8
    system_language: pt_BR.UTF-8
    system_time_zone: America/Sao_Paulo

    # Composer
    composer_version: 2.3.5

    # PHP
    php_version: 8.0

    # i-Educar
    ieducar_version: 2.7.2

    # Postgresql
    postgresql_version: 14
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

### Licença
-------
[GNU GENERAL PUBLIC LICENSE v3](LICENSE)