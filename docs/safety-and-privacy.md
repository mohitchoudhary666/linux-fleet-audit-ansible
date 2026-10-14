# Safety and privacy

## Read-only behavior

The included role uses Ansible facts, `assert`, `command`, `set_fact`, and `debug`. Every command task sets `changed_when: false` and tolerates missing optional commands. The play and Ansible configuration both set `become: false`. No task installs software, edits files, restarts services, changes a firewall, or enables remote access.

Review the playbook before using it in an environment with stricter requirements. Ansible facts and command output still disclose system details to the control node and into its terminal/logging system.

## Protect the inventory

`inventories/hosts.ini` is excluded by `.gitignore`. Do not commit private hostnames, addresses, usernames, proxy commands, credentials, vault passwords, or private keys. Use an SSH agent or your organization's approved secrets manager.

## Limit the target set

Use `--limit` for a small known group or host during initial use. Host key checking remains enabled. Do not broaden the inventory to systems you are not authorized to manage.

## Treat output as sensitive

The summary can reveal host identity, software versions, listening ports, SSH policy, and network forwarding state. Redact it before sharing publicly. Findings need context: routers, bastions, and application servers have different intended settings.
