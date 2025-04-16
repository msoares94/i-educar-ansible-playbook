
# i-Educar Ansible Playbook

Repositório de automação para instalação e configuração completa da aplicação [i-Educar](https://github.com/portabilis/i-educar) utilizando Ansible.

---

## 📦 Estrutura do Projeto

```bash
.
├── group_vars/
│   ├── all/
│   │   └── *.yml                       # Variáveis globais
│   ├── development/
│   │   └── main.yml                    # Configuração do ambiente de development
│   ├── staging/
│   │   └── main.yml                    # Configuração do ambiente de staging
│   └── production/
│       └── main.yml                    # Configuração do ambiente de production
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

---

## 🛠️ Preparando o Playbook

1. Edite o arquivo `inventory.ini` e adicione seus servidores. Exemplo:

```ini
[ieducar]
10.0.0.1 ansible_user=root
10.0.0.2 ansible_user=ubuntu ansible_become=true ansible_become_method=sudo
```

> Para mais opções de configuração, consulte o arquivo `inventory.ini.example` ou os arquivos em `group_vars`.

---

## 🔐 Modo de Autenticação no Servidor de Destino

### Chave SSH (recomendado)

```bash
ssh-keygen -t rsa
chmod 400 ~/.ssh/id_rsa
ansible-playbook add-key.yml -i inventory.ini --key-file ~/.ssh/id_rsa --extra-vars "key=~/.ssh/id_rsa.pub"
```

### Senha (alternativo)

```bash
ansible-playbook playbook.yml -i inventory.ini --ask-pass
```

---

## 🚀 Executando o Playbook

### Com chave SSH

```bash
ansible-playbook playbook.yml -i inventory.ini --key-file ~/.ssh/id_rsa
```

### Com senha

```bash
ansible-playbook playbook.yml -i inventory.ini --ask-pass
```

### Selecionando o ambiente

```bash
ansible-playbook playbook.yml -i inventory.ini -l staging
```

---

## 🛠️ Funcionalidades

- Instalação do i-Educar com base na branch ou tag desejada
- Suporte a múltiplos ambientes (`development`, `staging`, `producao`)
- Instalação automática de:
  - PostgreSQL otimizado para RAM disponível
  - Redis
  - PHP-FPM
  - NGINX com suporte a domínio e SSL (Let's Encrypt ou manual)
- Geração automática do `.env` com variáveis sensíveis
- Permissões ajustadas com `ACL` para usuário e grupo `www-data`
- Integração opcional com:
  - Pacote de relatórios da comunidade
  - Pacote Educacenso da comunidade
- Geração automática de certificados SSL com Certbot (quando habilitado)
- Cópia automática de certificados fornecidos manualmente (quando configurado)
- Geração condicional de arquivos NGINX com base nas variáveis `ieducar_with_domain`, `ieducar_use_ssl` e `ieducar_use_letsencrypt`
- Definição automática de variáveis adicionais como `APP_URL`, `ASSETS_SECURE` e `APP_DEFAULT_HOST` com base nas configurações de domínio e protocolo

---

## 📋 Requisitos

- Ubuntu Server 22.04+ com acesso via SSH
- Ansible 2.14+
- Permissões de root ou usuário `sudo`

---

## 🧩 Considerações sobre Domínio e SSL

### Acesso via IP (sem domínio)
- `ieducar_with_domain: false`

### Acesso via domínio **sem SSL**
- `ieducar_with_domain: true`
- `ieducar_use_ssl: false`

### Acesso via domínio com **SSL automático (Let's Encrypt)**
- `ieducar_with_domain: true`
- `ieducar_use_ssl: true`
- `ieducar_use_letsencrypt: true`
- Requer que o domínio esteja apontado para o IP do servidor

### Acesso via domínio com **certificado próprio**
- `ieducar_with_domain: true`
- `ieducar_use_ssl: true`
- `ieducar_use_letsencrypt: false`
- Informar:
  - `ieducar_ssl_certificate_path`
  - `ieducar_ssl_certificate_key_path`
  - `ieducar_ssl_certificate_src`
  - `ieducar_ssl_certificate_key_src`

---

## 📄 Licença

Este projeto segue os princípios de software livre e está sob a [licença GPL v2.0](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html).

---

Para mais informações sobre o i-Educar: [https://ieducar.org](https://ieducar.org)