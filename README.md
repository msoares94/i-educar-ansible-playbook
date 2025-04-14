# i-Educar Ansible Playbook

Repositório de automação para instalação e configuração completa da aplicação [i-Educar](https://github.com/portabilis/i-educar) utilizando Ansible.

> Status do projeto: Em desenvolvimento :warning:
> 
## 📦 Estrutura do Projeto

```bash
.
├── group_vars/
│   ├── all/
│   │   └── *.yml                       # Variáveis globais
│   ├── development/
│   │   └── main.yml                    # Configuração do ambiente de desenvolvimento
│   ├── staging/
│   │   └── main.yml                    # Configuração do ambiente de staging
│   └── producao/
│       └── main.yml                    # Configuração do ambiente de produção
│
├── roles/
│   ├── common/
│   ├── postgresql/
│   ├── redis-server/
│   ├── php-fpm/
│   ├── nginx/
│   ├── i-educar/
│   ├── i-educar-reports-package/
│   └── i-educar-educacenso-package/
│
├── inventory.ini                       # Inventário dos hosts
└── playbook.yml                        # Playbook principal
```

## 🛠️ Preparando o Playbook

1. Edite o `inventory.ini` e adicione seus servidores, `10.0.0.1` e `10.0.0.2` são exemplos, ajuste para valores reais:

```ini
[ieducar]
10.0.0.1 ansible_user=root
10.0.0.2 ansible_user=ubuntu ansible_become=true ansible_become_method=sudo
```
> Para mais opções de variáveis, consulte o inventory.ini.example ou os arquivos em `group_vars`

## Modo de autenticação no servidor de destino

### 🔐 Chave SSH

```bash
ssh-keygen -t rsa
chmod 400 ~/.ssh/id_rsa
ansible-playbook add-key.yml -i inventory.ini --key-file ~/.ssh/id_rsa --extra-vars "key=~/.ssh/id_rsa.pub"
```

### 🔑 Senha

Sem etapas adicionais.

---

## 🚀 Executando o Playbook

### Com chave SSH:

```bash
ansible-playbook playbook.yml -i inventory.ini --key-file ~/.ssh/id_rsa
```

### Com senha:

```bash
ansible-playbook playbook.yml -i inventory.ini --ask-pass
```

#### Execute com o grupo de hosts desejado (ex: `staging`, `development`, `producao`):

```bash
ansible-playbook playbook.yml -i inventory.ini -l staging
```


## 🛠️ Features

- Instalação do i-Educar com base na branch/tag configurada
- Suporte a múltiplos ambientes (staging, produção etc.)
- Configuração automatizada de:
  - PostgreSQL com otimizações por RAM
  - Redis
  - PHP-FPM com pools customizados
  - NGINX com suporte a domínio, SSL e Let's Encrypt
- Geração do `.env` com variáveis sensíveis
- Permissões adequadas com `ACL`
- Integração opcional com pacote de relatórios da comunidade
- Integração opcional com pacote do educacenso da comunidade

## 📋 Requisitos

- Servidores Ubuntu 22.04+ com acesso via SSH
- Ansible 2.14+
- Acesso com permissões root ou usuário `sudo` configurado

## 📄 Licença

Este projeto segue os princípios de software livre e está sob a [licença GPL v2.0](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html).

---

Para mais informações sobre o i-Educar: [https://ieducar.org](https://ieducar.org)