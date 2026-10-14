# Extending the audit role

Keep the default play read-only and avoid `become`. For each additional check:

1. Prefer Ansible facts or an argument-vector `ansible.builtin.command` task over shell pipelines.
2. Register output, set `changed_when: false`, and handle unavailable commands without hiding unexpected findings.
3. Add the field to `linux_audit_summary` and document whether it can disclose sensitive host details.
4. Explain platform requirements and avoid treating a single configuration value as universally compliant.
5. Keep remediation in a separate, explicitly invoked playbook with clear variables, backups, rollback guidance, and a lab-first workflow.

Potential future checks include package update age, mount options, time synchronization, and host firewall status. Distribution-specific checks should be gated by gathered OS facts and return `unavailable` when the relevant tool is absent.
