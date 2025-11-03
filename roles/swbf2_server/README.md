# SWBF2 Server Ansible Role

Ansible role for deploying Star Wars Battlefront 2 game server on Windows VPS.

## Supported Platforms

- Windows Server 2019
- Windows Server 2022
- Windows Server 2025

## Requirements

- Ansible 2.9 or higher
- Windows target hosts with WinRM configured
- Appropriate permissions on target Windows hosts

## Role Variables

See `defaults/main.yml` for available variables.

## Dependencies

None at this time.

## Example Playbook

```yaml
- hosts: windows_game_servers
  roles:
    - swbf2_server
```

## License

MIT

## Author Information

This role is currently under development.

