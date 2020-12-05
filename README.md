ansible-prometheus
=========

Ansible role for installing Prometheus monitoring system.

Role installs and manages services using systemd. Currently supported Prometheus modules:
  - Prometheus
  - Node Exporter (collects metrics of host machine)
  - Alert manager
  - Push gateway
  - SNMP exporter
  - Blackbox exporter

Playbook includes extensive configuration options check [default/main.yml](https://github.com/llamalump/ansible-prometheus/blob/master/defaults/main.yml)

Contributing
------------

Pull requests are always welcome! If you'd like to contribute, please:

1. Fork this repo 
1. Make your changes in a feature branch
1. [Raise a Pull Request](https://github.com/llamalump/ansible-prometheus/compare)

Installation
------------

To "install" this role, you can either install via Ansible Galaxy:

    ansible-galaxy install llamalump.ansible-prometheus

Or you can simply clone the repository:

    git clone https://github.com/llamalump/ansible-prometheus.git

Requirements
------------

* Systemd
* Firewalld (the role will install this for you, and open the required ports as well)

Role Variables
--------------

```yaml
---
# defaults/main.yml - Defaults for ansible-prometheus

# versions
prometheus_version: "2.22.2"
node_exporter_version: "1.0.1"
alert_manager_version: "0.21.0"
push_gateway_version: "1.3.0"
snmp_exporter_version: "0.19.0"
blackbox_exporter_version: "0.18.0"

# General defaults
prometheus_platform_architecture: 'linux-amd64'

# Default installation flags
prometheus_install: false
prometheus_node_exporter_install: true
prometheus_alert_manager_install: false
prometheus_push_gateway_install: false
prometheus_snmp_exporter_install: false
prometheus_blackbox_exporter_install: false

# Default service user/group configuration
prometheus_service_user: "prometheus"
prometheus_service_group: "prometheus"

# Default directory configurations
prometheus_base_install_dir: "/opt"
prometheus_install_dir: "{{ prometheus_base_install_dir }}/prometheus"
prometheus_lib_dir: "/var/lib/prometheus"
prometheus_config_dir: "/etc/prometheus"
prometheus_rules_dir: "{{ prometheus_config_dir }}/rules"
prometheus_data_base_dir: "/data/prometheus"
prometheus_data_dir: "{{ prometheus_data_base_dir }}/prometheus/data"

# Prometheus server defaults
prometheus_server_conf_template_src: "templates/prometheus.yml.j2"
prometheus_server_conf_dst: "{{ prometheus_config_dir }}/prometheus.yml"

# Node_exporter defaults
node_exporter_install_dir: "{{ prometheus_base_install_dir }}/node_exporter"
node_exporter_service_file_template_src: "templates/node_exporter.service.j2"
node_exporter_service_file_dst: "/etc/systemd/system/node_exporter.service"

# Alertmanager defaults
alert_manager_install_dir: "{{ prometheus_base_install_dir }}/alertmanager"
alert_manager_data_dir: "{{ prometheus_data_base_dir }}/alertmanager/data"
alert_manager_config_dir: "{{ prometheus_config_dir }}/alertmanager"
alert_manager_templates_dir: "{{ alert_manager_config_dir }}/templates"

# SNMP exporter defaults
snmp_exporter_config_dir: "{{ prometheus_config_dir }}/snmpexporter"

# Blackbox exporter defaults
blackbox_exporter_config_dir: "{{ prometheus_config_dir }}/blackboxexporter"
```

[DOCS: Prometheus variables](https://github.com/llamalump/ansible-prometheus/blob/master/docs/prometheus.md)

[DOCS: Node exporter variables](https://github.com/llamalump/ansible-prometheus/blob/master/docs/node_exporter.md)

[DOCS: Alert manager variables](https://github.com/llamalump/ansible-prometheus/blob/master/docs/alert_manager.md)

[DOCS: Pushgateway variables](https://github.com/llamalump/ansible-prometheus/blob/master/docs/push_gateway.md)

[DOCS: SNMP exporter variables](https://github.com/llamalump/ansible-prometheus/blob/master/docs/snmp_exporter.md)

[DOCS: Blackbox exporter variables](https://github.com/llamalump/ansible-prometheus/blob/master/docs/blackbox_exporter.md)

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- name: Instal Prometheus on hosted machine
  hosts: servers
  sudo: yes
  roles:
    - role: ansible-prometheus
      prometheus_config_scrape_configs:
        - job_name: 'prometheus'
          honor_labels: true
          scrape_interval: '15s'
          scrape_timeout: '3s'
```

License
-------

Copyright (c) 2016, Ernestas Poskus
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this
  list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice,
  this list of conditions and the following disclaimer in the documentation
  and/or other materials provided with the distribution.

* Neither the name of ansible-prometheus nor the names of its
  contributors may be used to endorse or promote products derived from
  this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

