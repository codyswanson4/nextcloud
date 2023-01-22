Role Name
=========

Ansible deployment role for contianerized Nextcloud deployment in Podman Pod 

Role Variables
--------------

nextcloud_version: Nextcloud version portion of tag.  Note this variable is appended with "-fpm".  Examples: "24", "25.0.3", "production", "stable"
nextcloud_admin_username: Username for the default created administrator account.  Note this string must not be a directory existent in the data_dir specified below.
nextcloud_admin_password: Password for the default created administrator account.
nextcloud_trusted_domains: Space separated list of domains that should be in the Nextcloud Trusted Domains list.

home_dir: Directory used as nextcloud user home on host.
data_dir: Directory for Nextcloud data on host.

listen_http_port: HTTP port on the host that will be mapped to NGINX http port.
listen_https_port: HTTPS port on the host that will be mapped to NGINX https port.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

See molecule/default/converge.yml for example usage

License
-------

GPLv3 see LICENSE
