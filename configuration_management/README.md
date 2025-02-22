# DevOps-Case-Study

## Tasks

### 1. Retrieve Ansible Configuration
To list all `ansible_` configurations for a specific host:
```bash
ansible-config dump

```

### 2. Configure Logrotate via Cron
The `logrotate.yml` playbook ensures log rotation runs every 10 minutes between 2 AM - 4 AM. Run:
```bash
ansible-playbook -i inventory.ini logrotate.yml
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


