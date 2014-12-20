ansible-teamcity
=========

ansible-teamcity is an [Ansible](http://ansible.com) role.
Use this role to install [Teamcity](http://www.jetbrains.com/teamcity/) server and agent on server and run it within [Docker](http://docker.com) containers.

Requirements
------------

1. Ansible 1.4 or higher
2. Ubuntu 14.04 (other Linux distro with docker support)
3. Docker
4. Vagrant (optional)
5. PostgreSQL (other RDBMS which Teamcity support)

Role Variables
--------------

The role has default value that should work without modification unless you want to

Docker container settings for server:

    teamcity_server_image: clayman74/teamcity-server:9.0
    teamcity_server_container: teamcity-server

Docker container settings for agent:

    teamcity_agent_image: clayman74/teamcity-agent
    teamcity_agent_container: teamcity-agent
    teamcity_agent_server_address: 127.0.0.1

External database settings for server:

    teamcity_external_db: false
    teamcity_db_user: teamcity
    teamcity_db_password: secret
    teamcity_db_name: teamcity

Dependencies
------------

Dependecies are not enforced by the role and left for you to manage - Docker - Nginx - PostgreSQL

Usage
----------------

### Get the code

```bash
$ git clone git@github.com:clayman74/ansible-teamcity.git
```

The code should reside in the roles directory of ansible ( See [ansible documentation](http://www.ansibleworks.com/docs/playbooks.html#roles) for more information on roles ), in a folder jenkins.

### Create a host file

Following example make ansible aware of the Vagrant box reachable on localhost port 2222.

```bash
$ vi ansible.host
```

with

```ini
[teamcity]
127.0.0.1 ansible_ssh_port=2222
```

### Create host specific variables

Make the host_vars directory where *ansible.host* file is located.

```bash
$ mkdir host_vars
```

Create a file in the newly created directory matching your host.

```bash
$ cd host_vars
$ vi 127.0.0.1
```

with

```yaml
---
teamcity_external_db: false
teamcity_db_user: teamcity
teamcity_db_password: secret
teamcity_db_name: teamcity
```

### Run the playbook


First create a playbook including the jenkins role, naming it `teamcity.yml`.

```yml
- name: Teamcity
  hosts: teamcity
  roles:
    - ansible-teamcity
```

Use *ansible.host* as inventory. Run the playbook only for the remote host *teamcity*. Use *vagrant* as the SSH user to connect to the remote host. *-k* enables the SSH password prompt.

```bash
$ ansible-playbook -k -i ansible.host teamcity.yml -u vagrant
```

License
-------

MIT
