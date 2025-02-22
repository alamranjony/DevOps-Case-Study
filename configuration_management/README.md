# Configuration Management Assignment

## Environment Details
- **OS:** Ubuntu 20.04 LTS
- **Ansible Version:** 2.9.16
- **Puppet Version:** 4.5+

## Tasks Implemented

### 1. Retrieve Ansible Configuration
To list all `ansible_` configurations for a specific host:
```bash
ansible-config dump

```

### 2. Configure Logrotate via Cron
The `cron_logrotate.yml` playbook ensures log rotation runs every 10 minutes between 2 AM - 4 AM. Run:
```bash
ansible-playbook -i inventory.ini cron_logrotate.yml
```

### 3. Deploy NTPD to Servers
The `deploy_ntpd.yml` playbook installs and configures NTP on:
- `app-vm1.fra1.internal` (192.168.0.2)
- `db-vm1.fra1.db` (192.168.0.3)
- `web-vm1.fra1.web` (192.168.0.4)

To deploy:
```bash
ansible-playbook -i inventory.ini deploy_ntpd.yml
```

### 4. Deploy Nagios to a server for deploying monitoring template
The `nagios_install.yml` playbook installs and configures Nagios on:
- `monitoring.fra1.internal` (192.168.0.5)

To deploy:
```bash
ansible-playbook -i inventory.ini nagios_install.yml
```

### 3. Deploy Nagios monitoring template
The `nagios_config.yml` playbook configure Nagios monitoring template on:
- `app-vm1.fra1.internal` (192.168.0.2)
- `db-vm1.fra1.db` (192.168.0.3)
- `web-vm1.fra1.web` (192.168.0.4)

To deploy:
```bash
ansible-playbook -i inventory.ini nagios_config.yml
```

