# Ansible Role: exporter exporter

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)
[![GitHub tag](https://img.shields.io/github/tag/cloudalchemy/ansible-node-exporter.svg)](https://github.com/umanit/ansible-prometheus_exporter_exporter/tags)

## Description

Deploy prometheus [Exporter Exporter](https://github.com/QubitProducts/exporter_exporter) using ansible.

Exporter exporter is a Prometheus exporter providing a reverse proxy for others Prometheus exporters, allowing to open only one port, it also provide TLS communication and Bearer Authentication.

It's a lightweight and simple alternative to NGINX/Apache reverse proxy, especially when the monitored server is not a web server...

More informations on [Exporter Exporter Readme page](https://github.com/QubitProducts/exporter_exporter#configuration)

## Notes

Role forked and largely inspired by [Cloudalchemy Node Exporter Ansible role](https://github.com/cloudalchemy/ansible-node-exporter)

Role is supposed to work with Debian, Suse, RedHat, Fedora, (See [Ansible Galaxy meta](/meta/main.yml)), but it was only tested on Ubuntu Bionic (18.04).

*TODO:*

(Probably never done, PR accepted)
* More control in [preflight file](/tasks/preflight.yml)
* Support of config.dirs
* Managment of Bearer token file
* Unit test
* Publish on Ansible Galaxy

## Requirements

- Ansible >= 2.7 (It might work on previous versions, but we cannot guarantee it)
- gnu-tar on Mac deployer host (`brew install gnu-tar`)

## Role Variables

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file as well as in table below.

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `exporter_exporter_version` | 0.3.0 | Used to install Exporter exporter package. Also accepts latest as parameter. |
| `exporter_exporter_system_group` | "exp-exp" | System group used to run exporter_exporter <br>(used to launch exporter_exporter binary in systemd service unit file)|
| `exporter_exporter_system_user` | "exp-exp" | System user used to run exporter_exporter <br>(used to launch exporter_exporter binary in systemd service unit file)|
| `exporter_exporter_system_additional_groups` | "" | Additional system group of user used to run exporter_exporter (example: ssl-cert to allow reading certificate, used to  launch exporter_exporter binary in systemd service unit file) |
| `exporter_exporter_web_listen_address` | "0.0.0.0:9999" | Address on which node exporter will listen (HTTP), leave empty and provide `exporter_exporter_web_tls_listen_address` for HTTPS connection only <br>(used to launch exporter_exporter binary in systemd service unit file) |
| `exporter_exporter_config_file` | "/etc/expexp.yaml" | File containing exporter_exporter configuration, managed with this role throught `exporter_exporter_configuration` variable <br>(used to launch exporter_exporter binary in systemd service unit file) |
| `exporter_exporter_web_bearer_token` | "" | Token to provide to Bearer authentication <br>(mutually exclusive with `exporter_exporter_web_bearer_token_file` variable, used to launch exporter_exporter binary in systemd service unit file)|
| `exporter_exporter_web_bearer_token_file` | "" | File containing Bearer token for authentication <br>(mutually exclusive with `exporter_exporter_web_bearer_token` variable, managment of the file not provided by this role, used to  launch exporter_exporter binary in systemd service unit file) |
| `exporter_exporter_web_proxy_path` | "" | URL to listen on for proxy HTTP requests. <br>(default "/proxy" in exporter_exporter if not provided, used to  launch exporter_exporter binary in systemd service unit file)|
| `exporter_exporter_web_telemetry_path` | "" | URL to listen on for metrics of exporter_exporter itself. <br>(default "/metrics" in exporter_exporter if not provided, used to  launch exporter_exporter binary in systemd service unit file) |
| `exporter_exporter_web_tls_ca` | "" | Full path of file containing CA certificate <br>(ie: Prometheur server cert if self-signed certificate are used, used to  launch exporter_exporter binary in systemd service unit file) |
| `exporter_exporter_web_tls_cert` | "" | Full path of file containing certificate used by exporter_exporter <br>(ie: Node certificate, used to  launch exporter_exporter binary in systemd service unit file) |
| `exporter_exporter_web_tls_key` | "" | Full path of file containing key used by exporter_exporter <br>(ie: Node key, used to  launch exporter_exporter binary in systemd service unit file) |
| `exporter_exporter_web_tls_listen_address` | "" | Address on which node exporter will listen for TLS connections (HTTPS), optionnal, 0.0.0.0:9998 is usualy used <br>(used to launch exporter_exporter binary in systemd service unit file) |
| `exporter_exporter_web_tls_verify` | "" | Disable client verification <br>(?, used to launch exporter_exporter binary in systemd service unit file) |
| `exporter_exporter_configuration` | "" | YAML configuration set in `exporter_exporter_config_file` [example provided in Exporter Exporter Readme](https://github.com/QubitProducts/exporter_exporter#configuration) |

Exporter_exporter configuration can be breaked in multiple files in a directory provided to exporter_exporter throught config.dirs and config.skip-dirs, but this role does not support (yet) this.

Creation and deployment of certificates and key used througth exporter_exporter_web_tls_xxx variables is not part of this role.


## Example

### Playbook

Use it in a playbook as follows:
```yaml
- hosts: all
  roles:
    - umanit.prometheus_exporter_exporter
  vars:
    exporter_exporter_configuration:
      modules:
        node:
          method: http
          http:
             port: 9100
```
## License

This project is licensed under MIT License. See [LICENSE](/LICENSE) for more details.
