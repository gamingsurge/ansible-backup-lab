# Ansible Cisco Backup Lab

This project automates the process of backing up configuration files from Cisco IOS switches using Ansible.

---

## 📁 Project Structure

```
ansible-backup-lab/
├── group_vars/
│   └── cisco_switches.yml         # Connection variables for Cisco switches
├── inventory/
│   └── hosts.yml                  # Inventory file with Cisco switch hostnames/IPs
├── playbooks/
│   └── backup_configs.yml         # Main playbook to back up configs
├── switch_backups/               # Final backup output directory (auto-created)
└── README.md                      # This file
```

---

## 🚀 What It Does

1. **Backs up running configuration** directly from Cisco IOS switches.
2. **Fetches the config file** to the local Ansible control node.
3. **Stores backups** in a `switch_backups/` folder.
4. **Cleans up** temporary files on both the switch and local temp storage.

---

## 🛠️ Requirements

- Ansible 2.10+
- Python 3.x
- Network access to Cisco IOS devices
- `cisco.ios` collection installed:

```bash
ansible-galaxy collection install cisco.ios
```

---

## ⚙️ Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/ansible-backup-lab.git
cd ansible-backup-lab
```

### 2. Define Inventory

Edit `inventory/hosts.yml`:

```yaml
all:
  children:
    cisco_switches:
      hosts:
        sw1:
          ansible_host: 192.168.1.10
        sw2:
          ansible_host: 192.168.1.11
```

### 3. Set Credentials and Connection Method

Edit `group_vars/cisco_switches.yml`:

```yaml
ansible_connection: network_cli
ansible_network_os: cisco.ios.ios
ansible_user: your_username
ansible_password: your_password
ansible_ssh_common_args: >
  -o PreferredAuthentications=password
  -o PubkeyAuthentication=no
  -o KexAlgorithms=+diffie-hellman-group14-sha1
  -o HostKeyAlgorithms=+ssh-rsa
```

> 🔐 **Sensitive Info Warning**: Consider using Ansible Vault to encrypt this file.

---

## ▶️ Running the Backup

```bash
ansible-playbook playbooks/backup_configs.yml
```

After execution, all switch configurations will be saved in the `switch_backups/` folder with filenames like:

```
switch_backups/
├── sw1.cfg
├── sw2.cfg
```

---

## ✅ Example Output

```text
TASK [Ensure final backup directory exists] ***************************
changed: [sw1 -> localhost]

TASK [Backup running config on device] ********************************
ok: [sw1]

TASK [Fetch config from device to temp dir] ***************************
ok: [sw1]

TASK [Move fetched config to final switch_backups folder] *************
changed: [sw1 -> localhost]
```

---

## 🧹 Cleanup

The playbook automatically removes:
- Temporary config files from `/tmp`
- Backup files from the switch
- Leftover folders like `playbooks/backup/` that shouldn't exist

---

## 💡 Future Improvements

- Add timestamps to backup filenames
- Encrypt backups with Vault
- Zip backups into an archive
- Send Discord/Slack notifications

---

## 👤 Author

**Sergio Cuevas**  
🔗 [serginetworks.com](https://serginetworks.com)
