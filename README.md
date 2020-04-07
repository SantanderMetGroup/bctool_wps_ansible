# BCtool as a WPS service

## Introduction

PyWPS Ansible Playbook can completely provision a remote server to run the full stack of PyWPS, including:

- Conda to manage application dependencies.
- `Nginx <http://wiki.nginx.org/Main>` as Web-Server.
- `Supervisor <http://supervisord.org/>` to start/stop and monitor services.
- PostgreSQL optional database used for job logging.

It will install a PyWPS application on a single host.
Nginx, Supervisor and miniconda are installed on the system.
The PyWPS application is fetched from GitHub and dependencies are installed into a Conda environment.

See the ``docs`` subdirectory or `readthedocs <http://ansible-wps-playbook.readthedocs.io/en/latest/>` for complete documentation.

- Birdhouse: http://bird-house.github.io/
- PyWPS: http://pywps.org/
- Emu: http://emu.readthedocs.io/en/latest/
- Ansible: https://www.ansible.com/
- Vagrant: https://www.vagrantup.com/
- Conda: https://conda.io/miniconda.html
- PostgreSQL: https://www.postgresql.org/

## Vagrant testing

```
ansible-galaxy install -p roles -r requirements.yml
cp custom.yml.vagrant custom.yml
vagrant up
```

To test the WPS, install pyramid-phoenix WPS client:

```
ansible-playbook -i vagrant --key-file ~/.vagrant.d/insecure_private_key pyramid-phoenix.yml
vagrant ssh
source anaconda/bin/activate pyramid-phoenix
cd pyramid-phoenix/
cp --force custom.cfg.example custom.cfg
echo hostname=192.168.128.100 >> custom.cfg
make install
make start
```

Go to your browser to `192.168.128.100:8081` (vagrant machine IP) and you should see the pyramid-phoenix web interface. Admin password is `qwerty`. More info in `https://pyramid-phoenix.readthedocs.io/en/latest/index.html`.

## Production deployment

Configure ansible to connect to your hosts, configure your `custom.yml` and `group_vars/all` and then:

```
ansible-galaxy install -p roles -r requirements.yml
ansible-playbook playbook.yml
```
