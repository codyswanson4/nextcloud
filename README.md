Nextcloud Podman
=========

Ansible deployment role for contianerized Nextcloud deployment in Podman Pod.

Role Variables
--------------

| Variable Name | Default | Description |
| ------------------------- | ------------------------- | |
| nextcloud_version | stable | Nextcloud version portion of tag.  Note this variable is appended with "-fpm".  Examples: "24", "25.0.3", "production", "stable" |
| nextcloud_admin_username | admin | Username for the default created administrator account.  Note this string must not be a directory existent in the data_dir specified below. |
| nextcloud_admin_password | Randomly generated | Password for the default created administrator account. |
| nextcloud_trusted_domains | | Space separated list of domains that should be in the Nextcloud Trusted Domains list. |
| home_dir | /opt/nextcloud | Directory used as nextcloud user home on host. This directory is used for placing the unit files and configuration files. |
| data_dir | /srv/nextcloud | Directory for Nextcloud data on host. |
| listen_http_port | 1080 | HTTP port on the host that will be mapped to NGINX http port. |
| listen_https_port | 1443 | HTTPS port on the host that will be mapped to NGINX https port. |

Deployment Recommendations
-----------

This role will deploy a working Nextcloud server using a rootless Podman Pod with separated redis cache and mysql database. It also makes use of self signed certificates to be used behind a WAF with trusted certificates.
Recommended configuration is a Web Application Firewall in a DMZ fronting the NGINX service's HTTPS server.

Dependencies
------------

Depends upon the installation of the following Galaxy packages:
- community.crypto
- containers.podman

Software dependencies are installed on playbook run.

Example Playbook
----------------

See molecule/default/converge.yml for example usage

License
-------

GPLv3 see LICENSE
