# Ansible Role: rClone

[![ubuntu-18](https://img.shields.io/badge/ubuntu-18.x-orange?style=flat&logo=ubuntu)](https://ubuntu.com/)
[![ubuntu-20](https://img.shields.io/badge/ubuntu-20.x-orange?style=flat&logo=ubuntu)](https://ubuntu.com/)
[![debian-9](https://img.shields.io/badge/debian-9.x-orange?style=flat&logo=debian)](https://www.debian.org/)
[![debian-10](https://img.shields.io/badge/debian-10.x-orange?style=flat&logo=debian)](https://www.debian.org/)
[![centos-7](https://img.shields.io/badge/centos-7.x-orange?style=flat&logo=centos)](https://www.centos.org/)
[![centos-8](https://img.shields.io/badge/centos-8.x-orange?style=flat&logo=centos)](https://www.centos.org/)

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg?style=flat)](https://opensource.org/licenses/MIT)
[![GitHub issues](https://img.shields.io/github/issues/OnkelDom/ansible-role-rclone?style=flat)](https://github.com/OnkelDom/ansible-role-rclone/issues)
[![GitHub tag](https://img.shields.io/github/tag/OnkelDom/ansible-role-rclone.svg?style=flat)](https://github.com/OnkelDom/ansible-role-rclone/tags)
[![GitHub action](https://github.com/OnkelDom/ansible-role-rclone/workflows/ansible-lint/badge.svg)](https://github.com/OnkelDom/ansible-role-rclone)

## Description

Deploy prometheus [rClone](https://github.com/rclone/rclone) using ansible.

## Requirements

- Ansible >= 2.9 (It might work on previous versions, but we cannot guarantee it)

## Role Variables

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file as well as in table below.

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `proxy_env` | {} | Proxy environment variables |
| `rclone_version` | 1.55.1 | Node exporter package version |
| `rclone_system_user` | rclone | default system user |
| `rclone_system_group` | rclone | default system group |
| `rclone_web_listen_port` | 3030 | default listen port |
| `rclone_config_dir` | /etc/rclone |  |
| `rclone_config_file` | config.conf |  |
| `rclone_mountfolder` | /mnt/google |  |
| `rclone_cryptfolder` | /mnt/google/media |  |
| `rclone_binary_install_dir` | /usr/local/bin |  |
| `rclone_http_service_enable` | true |  |
| `rclone_http_user` | "" |  |
| `rclone_http_pass` | "" |  |
| `rclone_http_adress` | 127.0.0.1:{{ rclone_listen_port }} |  |
| `rclone_http_params` | "--read-only --bwlimit 20M --dir-cache-time 60m --vfs-cache-max-age 3h --vfs-cache-mode minimal --exclude backup/** --vfs-cache-poll-interval 30m" |  |
| `rclone_startparams` | "--allow-other --allow-non-empty --no-modtime --dir-cache-time 24h --buffer-size 24M --vfs-cache-mode writes -v" |  |
| `rclone_unit_mount_connection` | gdrive-crypt | mount to define in systemd units |
| `rclone_connections` | {} | Define Connections |

  gdrive:
    type: drive
    client_id: "google_api_id"
    client_secret: "google_secret_pass"
    scope: drive
    use_trash: false
    skip_gdocs: true
    token: "your api token"
  gdrive-crypt:
    type: crypt
    remote: "gdrive:<cryptfolder>"
    filename_encryption: standard
    directory_name_encryption: "true"
    password: "first_encryption_password"
    password2: "second_encryption_password"

## Example

```yml
rclone_unit_mount_connection: gdrive-crypt
rclone_connections:
  gdrive:
    type: drive
    client_id: "google_api_id"
    client_secret: "google_secret_pass"
    scope: drive
    use_trash: false
    skip_gdocs: true
    token: "your api token"
  gdrive-crypt:
    type: crypt
    remote: "gdrive:<cryptfolder>"
    filename_encryption: standard
    directory_name_encryption: "true"
    password: "first_encryption_password"
    password2: "second_encryption_password"
```

### Playbook

Use it in a playbook as follows:
```yaml
- hosts: all
  roles:
    - onkeldom.rclone
```

## Contributing

See [contributor guideline](CONTRIBUTING.md).

## License

This project is licensed under MIT License. See [LICENSE](/LICENSE) for more details.
