# Star Wars Battlefront 2 Game Server - Ansible Deployment

Ansible role and playbooks for deploying Star Wars Battlefront 2 game server on Windows VPS.

## Project Structure

```
.
├── roles/
│   └── swbf2_server/
│       ├── defaults/
│       │   └── main.yml      # Default variables
│       ├── vars/
│       │   └── main.yml      # Role variables
│       ├── tasks/
│       │   └── main.yml      # Main tasks
│       ├── handlers/
│       │   └── main.yml      # Handlers
│       ├── meta/
│       │   └── main.yml      # Role metadata
│       └── README.md         # Role documentation
├── playbook.yml              # Main playbook
├── ansible.cfg               # Ansible configuration
├── hosts.example             # Example inventory file
└── README.md                 # This file
```

## Supported Windows Versions

- Windows Server 2019
- Windows Server 2022
- Windows Server 2025

## Prerequisites

1. Ansible installed on your control machine (version 2.9+)
2. Windows hosts with WinRM configured
3. Appropriate network access and credentials

## Setup

1. Copy `hosts.example` to `hosts` and configure your Windows hosts
2. Update variables in `roles/swbf2_server/defaults/main.yml` as needed
3. Run the playbook: `ansible-playbook playbook.yml`

## Configuration

Edit `roles/swbf2_server/defaults/main.yml` to customize deployment settings.

## License

MIT

## Status

⚠️ This role is currently in development. Tasks will be added in subsequent iterations.

