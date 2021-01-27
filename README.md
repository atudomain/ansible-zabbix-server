# Dockerized Zabbix Server with Ansible Bootstrap

Sets up Zabbix Server with Zabbix Frontend and Zabbix Agent.
Out of box features:
- https for frontend
- simple local/public switching for frontend
- passive agent model enabled
- volumes for persistant storage of database and psk data

# Usage

```
cp var/zabbix-server-conf.yml.template var/zabbix-server-conf.yml
```

var/zabbix-server-conf.yml is main configuration file.

```
---
zabbix_db_name:
zabbix_db_user:
zabbix_db_password:
zabbix_public_port_https: False
zabbix_public_port_http: False
```

You put there your db credentials. It is recommended to start with False for public access,
so frontend password can be changed safely.

You can create nginx directory and put there your dhparam.pem, ssl.crt, ssl.key.
They must be named accordingly.
If you not, playbook generates default ones (dhparam takes some time).
Remember to make ssl.key readable for zabbix.
You can put yours later and re-run playbook.

Run playbook:
```
ansible-playbook -i hosts zabbix-server.yml
```

After it finishes, you should be able to access Zabbix Frontend using zabbix-frontend container ip and ports.
You log in with Admin:zabbix credentials. Change password.
Edit Zabbix Server agent, so it uses DNS name zabbix-agent. After a moment you should be able to monitor
your zabbix-server.

Now you can edit var/zabbix-server-conf.yml and enable https publicly (8443 port on host).
Re-run playbook after that.

You can adjust configuration in docker-compose.yml and handle the deployment with docker-compose, but it is going to be
overwritten by ansible if you happen to re-run the playbook.

Your Zabbix Server monitoring is ready.
