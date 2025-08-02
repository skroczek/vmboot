# Ansible Bootstrap for Linux Initialization via ansible-pull

This repository provides a modular Ansible-based system initialization framework designed for use with `ansible-pull`. It is ideal for automating the setup of Debian and Ubuntu systems in environments where configuration is bootstrapped using `cloud-init`.

## 💡 Purpose

- Unified base configuration for fresh Linux systems
- Enable automatic security updates
- Easily extendable via Ansible roles (e.g., user setup, SSH hardening, monitoring)
- Suitable for cloud deployments using `cloud-init`

## 🐧 Supported Distributions

| Distribution | Playbook                  | Description                                  |
|--------------|---------------------------|----------------------------------------------|
| Debian       | `playbooks/debian.yml`    | Installs and enables `unattended-upgrades`   |
| Ubuntu       | `playbooks/ubuntu.yml`    | Enables automatic updates and base packages  |

## 📁 Repository Structure

```text
ansible-bootstrap/
├── ansible.cfg
├── inventory/
│   └── localhost
├── playbooks/
│   ├── debian.yml
│   └── ubuntu.yml
├── roles/
│   ├── common/
│   │   └── tasks/main.yml
│   ├── debian/
│   │   └── tasks/main.yml
│   └── ubuntu/
│       └── tasks/main.yml
```

🚀 Usage with cloud-init

Minimal working example:

```yaml
#cloud-config
package_update: true
package_upgrade: true
packages:
  - git
  - python3-pip

runcmd:
  - pip3 install ansible
  - ansible-pull -U https://github.com/<YOUR-USERNAME>/ansible-bootstrap.git -i localhost playbooks/ubuntu.yml
```

Alternatively, using cloud-init’s native Ansible integration:

```yaml
ansible:
  install_method: pip
  pull:
    url: "https://github.com/<YOUR-USERNAME>/ansible-bootstrap.git"
    playbook_names: [playbooks/ubuntu.yml]
```

🧪 Manual Execution (Local Testing)

```bash
sudo apt update
sudo apt install -y git python3-pip
pip3 install ansible
ansible-pull -U https://github.com/<YOUR-USERNAME>/ansible-bootstrap.git -i localhost playbooks/ubuntu.yml
```

## ➕ Ideas for Extensions
	•	User account provisioning (with SSH key setup)
	•	Basic firewall configuration (UFW, nftables, etc.)
	•	Locale and timezone settings
	•	Monitoring agent installation (e.g., Prometheus Node Exporter)
	•	Hardened APT sources

## 📄 License

This repository is licensed under the MIT License. See LICENSE for details.
