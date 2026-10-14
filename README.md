# Linux Fleet Audit with Ansible

An advanced, read-only Linux fleet audit built with Ansible. It gathers a concise operations and security snapshot from authorized Ubuntu/Debian hosts, flags a few review items, and summarizes the results in Ansible output.

## What it audits

- Distribution, version, kernel, CPU count, memory, and root filesystem capacity.
- Failed systemd units and listening local TCP/UDP sockets.
- Effective OpenSSH settings for root login and password authentication when `sshd` is available.
- IPv4 forwarding state for infrastructure review.

The playbook does not use privilege escalation, install packages, change configuration, restart services, scan remote ports, or collect passwords/private keys. SSH inventory, network access, and host authorization remain your responsibility. Use a dedicated lab or hosts you are authorized to administer.

## Requirements

- Ansible Core 2.16 or newer on the control node; Python 3.10+ recommended.
- SSH connectivity to the managed Linux hosts and a valid SSH key/agent setup.
- Managed hosts using systemd and Python 3; some checks report unavailable when their command is missing.

Install locally:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

On Windows, use WSL2 or a Linux control node for Ansible.

## Configure an inventory

Copy the example inventory and replace the documentation-only address with a host you own:

```bash
cp inventories/hosts.example.ini inventories/hosts.ini
```

The real inventory is ignored by Git. Keep private addresses, usernames, and connection details out of public commits. Prefer SSH agent/key-based authentication; never put passwords or private keys in the inventory.

Review the parsed inventory and connectivity:

```bash
ansible-inventory -i inventories/hosts.ini --graph
ansible -i inventories/hosts.ini linux -m ping
```

Run the audit:

```bash
ansible-playbook -i inventories/hosts.ini playbooks/audit.yml
```

Limit the audit to a known host or group:

```bash
ansible-playbook -i inventories/hosts.ini playbooks/audit.yml --limit lab-ubuntu-01
```

## Understand the output

The `linux_audit_summary` fact is printed once per host. It is an operational snapshot, not a certified compliance assessment. A failed unit or SSH setting should be reviewed against the host's intended role and your organization's baseline before any remediation.

## Project layout

```text
ansible.cfg
inventories/hosts.example.ini
playbooks/audit.yml
roles/linux_audit/tasks/main.yml
docs/                         setup, safety, and extension guidance
examples/sample-summary.yml   redacted example output
```

## Safety and privacy

- Use only systems you own or are authorized to administer.
- This role is intentionally read-only and `become: false`.
- Reports can expose hostnames, software versions, open local ports, and security posture. Review before sharing output.
- Do not commit real inventories, vault passwords, keys, or unredacted audit output.
- The result is informational; it does not prove a host is secure or compliant.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Improvements to cross-distribution handling, signal quality, and documentation are welcome.

## License

MIT. See [LICENSE](LICENSE).
