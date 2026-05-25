# Lessons learned

## What I built
An Ansible setup that configures 3 Ubuntu servers identically:
- Role common: installs packages, creates devops user
- Role docker: installs Docker, starts service
- Role app: pulls image, starts container, health check
- Inventory with group variables shared across all servers

## Key concepts

### Idempotency
Running the playbook twice gives the same result.
Install Docker only runs if Docker is not already installed.
Safe to run repeatedly without side effects.

### Roles
Roles split playbooks into reusable chunks.
common/ docker/ app/ can be reused in other projects.

### Inventory + group_vars
hosts.ini defines the servers.
group_vars/webservers.yml sets variables for all servers at once.
Change the Docker image in one place — updates all servers.

### become: yes
Ansible connects as a user but needs root for most tasks.
become: yes is equivalent to sudo.

## Commands reference
ansible all -m ping
ansible-playbook site.yml
ansible-playbook site.yml --check
ansible webservers -m shell -a "docker ps"
