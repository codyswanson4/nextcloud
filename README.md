# Nextcloud Podman

This role will deploy a running, unconfigured [Nextcloud](https://nextcloud.com/) server using a rootless Podman Pod with container-separated redis cache and mysql database. Self-signed certificates are used by default in order to facilitate the recommended deployment discussed below.

## Justification

[Nextcloud](https://nextcloud.com/) is a very useful self-hosted tool and a great candidate for containerization and pod running due to the additional services it requires to run. Additionally, the nature of Nextcloud itself requires heightened security due to the manner of personal data it may host, so any percaution that is possible without too negatively impacting the user experience should be taken.

With the above in mind, this role addresses usability and security concerns by using the Linux kernel's cgroup manager, rootless containers, [Podman](https://podman.io/), and [Systemd](https://systemd.io). It also separates individual services in individual containers within the shared pod as well as allowing only necessary communication between containers and externally to the pod. Monitoring is also provided by sending access and error logs directly to the system logs for easy ingestion in a log analytics solution or SIEM.  
Management is also simplified using standard Systemd services for each individual service as well as the pod and automatic updates for containers with the [io.containers.autoupdate](https://docs.podman.io/en/latest/markdown/podman-auto-update.1.html) label, though this can be disabled for production environments.  

## Role Variables

| Variable Name | Default | Description |
| ------------------------- | ------------------------- | -------------------------------------------------- |
| nextcloud_version | stable | Nextcloud version portion of tag. Note this variable is appended with "-fpm". Examples: "24", "25.0.3", "production", "stable" |
| nextcloud_admin_username | admin | Username for the default created administrator account. Note this string must not be a directory existent in the data_dir specified below. |
| nextcloud_admin_password | Randomly generated | Password for the default created administrator account. |
| nextcloud_trusted_domains | | Space separated list of domains that should be in the Nextcloud Trusted Domains list. |
| home_dir | /opt/nextcloud | Directory used as nextcloud user home on host. This directory is used for placing the unit files and configuration files. |
| data_dir | /srv/nextcloud | Directory containing Nextcloud data on host. |
| listen_http_port | 1080 | HTTP port on the host that will be mapped to NGINX http port. |
| listen_https_port | 1443 | HTTPS port on the host that will be mapped to NGINX https port. |

## Deployment Methods

This role has a few different possible deployment methods. Please consider using the recommended method to allow additional security features of a Web Application Firewall if possible.  

The recommended architecture consists of a host running the podman pod created using this role as well as a Web Application Firewall to inspect external connections and terminate a trusted TLS connection. This architecture requires additional configuration outside the scope of this role to configure WAF certificates and rules.  
Additionally, this role can be deployed internally for testing purposes without a WAF using the high number ports directly, changing the listen_http\[s\]\*_port directives to default http and https ports and authorizing the user to use those ports (using authbind and SELinux), redirecting ports (using iptables), or using nginx as a reverse proxy. Additional configuration is required to use trusted TLS certificates and requires changing the pki contents at ~/.config/nextcloud.key and ~/.config/nextcloud-ss.crt.

## System Requirements

This role is designed and tested on the systems listed in molecule/default/molecule.yml.

## Dependencies

Depends upon the installation of the following Galaxy packages:
- community.crypto
- containers.podman

System software dependencies are installed on playbook run.

## Contributing

This project welcomes contributions aligned with the stated project purpose. Please open an issue to track the change intended as well as get feedback from the project owner on mergability.

## Example Usage

Use of this role requires including the role via Ansible's include_role statement and specifying required variables in a variable source such as below;
```
- name: "Include codyswanson4.nextcloud"
    include_role:
        name: "codyswanson4.nextcloud"
```
See molecule/default/converge.yml for test usage

## License

GPLv3 see LICENSE
