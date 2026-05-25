# ansible-server-config

> Phase 2 Project 2 — automated server configuration with Ansible roles

![Ansible](https://img.shields.io/badge/Ansible-EE0000?style=flat&logo=ansible&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-18.04-orange)

## What this does

Configures any number of Ubuntu servers identically with one command:

```
ansible-playbook site.yml
        │
        ├── Role: common
        │     installs packages, creates devops user
        │
        ├── Role: docker
        │     installs Docker, starts service
        │
        └── Role: app
              pulls Docker image, starts container,
              confirms app is live on port 3000
```

## Quick start

```bash
# 1. Start 3 fake Ubuntu servers
docker-compose -f docker-compose.test.yml up -d

# 2. Generate SSH key
ssh-keygen -t ed25519 -f ~/.ssh/ansible_key -N ""

# 3. Copy key to all servers (password: root)
ssh-copy-id -i ~/.ssh/ansible_key.pub -p 2221 root@localhost
ssh-copy-id -i ~/.ssh/ansible_key.pub -p 2222 root@localhost
ssh-copy-id -i ~/.ssh/ansible_key.pub -p 2223 root@localhost

# 4. Test connectivity
ansible all -m ping

# 5. Run the full playbook
ansible-playbook site.yml

# 6. Verify on all servers
ansible webservers -m shell -a "docker ps"
```

## Structure

```
inventory/hosts.ini        server IPs
group_vars/webservers.yml  shared variables
roles/common/              system setup
roles/docker/              Docker install
roles/app/                 app deployment
site.yml                   master playbook
ansible.cfg                Ansible settings
```

## Lessons learned
See docs/lessons-learned.md
