# Star Wars Battlefront 2 Game Server - Ansible Deployment

Ansible role and playbooks for deploying Star Wars Battlefront 2 game server on Windows VPS.

## Project Structure

```
.
├── roles/
│   └── swbf2_server/
│       ├── defaults/
│       │   └── main.yml                    # Default variables
│       ├── vars/
│       │   └── main.yml                    # Role variables
│       ├── tasks/
│       │   ├── main.yml                    # Main task orchestration
│       │   ├── configure_system_settings.yml
│       │   ├── configure_autologon.yml
│       │   ├── install_virtio.yml
│       │   ├── install_vcredist.yml
│       │   ├── install_dependencies.yml
│       │   ├── download_swbf2_server.yml
│       │   ├── configure_swbf2_server.yml
│       │   ├── configure_firewall.yml
│       │   └── manual_tasks_reminder.yml
│       ├── handlers/
│       │   └── main.yml                    # Handlers
│       ├── meta/
│       │   └── main.yml                    # Role metadata
│       └── README.md                       # Role documentation
├── playbook.yml                            # Main playbook
├── ansible.cfg                             # Ansible configuration
├── hosts.example                           # Example inventory file
└── README.md                               # This file
```

## Supported Windows Versions

- Windows Server 2019
- Windows Server 2022
- Windows Server 2025

## Prerequisites

1. Ansible installed on your control machine (version 2.9+)
2. Windows hosts with WinRM configured
3. Appropriate network access and credentials
4. `community.windows` collection installed: `ansible-galaxy collection install community.windows`

## Features

This playbook automates the following tasks:

### System Configuration
- Changes ansible user password
- Updates computer hostname to match inventory
- Enables Remote Desktop (RDP)
- Configures performance settings for best performance
- Configures processor scheduling

### Software Installation
- Installs VirtIO drivers (for virtualization)
- Installs Visual C++ Redistributables (2015-2022)
- Installs dependencies via Chocolatey:
  - Google Chrome
  - Chrome Remote Desktop
  - 7-Zip
  - Notepad++
  - .NET 8.0 Runtime
  - GOG Galaxy

### SWBF2 Server Setup
- Downloads and extracts SWBF2 server zip to `Desktop\Servers\<folder_name>`
- Configures `core.xml` with:
  - WebAdminPrefix (using server IP address)
  - MySQL database configuration
- Creates startup shortcut for SWBF2Admin.exe

### Firewall Configuration
- Allows all traffic for BattlefrontII.exe
- Allows all traffic for GalaxyCommunication.exe
- Allows ICMP ping requests
- Allows TCP ports 8080-8081 for WebAdmin

### AutoLogon
- Configures automatic login for the ansible user using SysInternals AutoLogon

## Setup

1. Copy `hosts.example` to `hosts` and configure your Windows hosts:
   ```ini
   [windows_game_servers]
   server-name ansible_host=192.168.1.100

   [windows_game_servers:vars]
   ansible_user=Administrator
   ansible_password=your_password_here
   ```

2. Update variables in `roles/swbf2_server/defaults/main.yml`:
   - `swbf2_server_zip_url`: URL to download the SWBF2 server zip
   - `swbf2_mysql_database_name`: MySQL database name
   - `swbf2_mysql_username`: MySQL username
   - `swbf2_mysql_password`: MySQL password

3. Run the playbook:
   ```bash
   ansible-playbook playbook.yml
   ```

4. The playbook will prompt for:
   - New password for the ansible user
   - SWBF2 server folder name (extracted to Desktop\Servers\<folder_name>)

## Configuration

### Default Variables

Edit `roles/swbf2_server/defaults/main.yml`:

```yaml
# SWBF2 server zip URL
swbf2_server_zip_url: ""

# SWBF2 MySQL configuration
swbf2_mysql_database_name: ""
swbf2_mysql_username: ""
swbf2_mysql_password: ""
```

### WinRM Configuration

The `ansible.cfg` file includes:
- WinRM connection timeout: 300 seconds
- WinRM transport: NTLM
- Certificate validation: Ignored

## Manual Tasks

After running the playbook, the following tasks still need to be completed manually:

1. Activate Windows (Massgrave)
2. Configure Google Chrome Remote Desktop
3. Verify Performance Settings
4. Login to GOG Galaxy
5. Install SWBF2 in GOG Galaxy

These will be displayed at the end of the playbook run.

## Notes

- The playbook checks if the SWBF2 server path already exists and skips download/extraction if found
- Firewall rules are idempotent and can be run multiple times
- XML configuration updates are idempotent
- The server will be extracted to `Desktop\Servers\<folder_name>` (as specified during prompt)

## License

MIT

