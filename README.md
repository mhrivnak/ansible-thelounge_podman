The Lounge Podman
=================

This role uses podman and systemd to run [The Lounge](https://thelounge.chat/),
a popular IRC client web app, as a rootless container.

You can separately deploy a reverse proxy to expose The Lounge as an external
service. This role only makes it available on localhost. See
https://github.com/mhrivnak/ansible-thelounge_nginx as an option.

Add Users
---------

Login to the host system and run the following command, which will exec into the
running container to add the user.

```
sudo -u thelounge -- bash -c 'cd && podman exec -it --user node systemd-thelounge thelounge add alice'
```

Requirements
------------

- developed on CentOS Stream 9

Role Variables
--------------

`thelounge_port`: the port on the host system's localhost interface to listen on

Dependencies
------------

see requirements.yml for ansible requirements

Example Playbook
----------------

The following playbook will deploy The Lounge:
- as a rootless container using podman
- with nginx as a reverse proxy
- with a TLS certificate from [letsencrypt](https://letsencrypt.org/)

```
- hosts: irc_client_host
  become: true

  roles:
    - mhrivnak.nginx
    - mhrivnak.thelounge_podman
    - geerlingguy.certbot
    - mhrivnak.thelounge_nginx

  vars:
    certbot_admin_email: certs@example.com
    certbot_create_if_missing: true
    certbot_certs:
      - domains:
          - irc.example.com
    thelounge_fqdn: irc.example.com
    thelounge_port: 9000
```

License
-------

GPLv3
