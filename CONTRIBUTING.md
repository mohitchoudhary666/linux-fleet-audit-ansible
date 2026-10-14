# Contributing

Contributions to read-only fleet auditing, cross-distribution support, and clear operations guidance are welcome.

- Do not add destructive or mutating tasks to the default audit play.
- Do not weaken SSH host key checking or commit credentials, private inventories, or real audit output.
- Use `ansible.builtin` modules explicitly where possible.
- Mark read-only commands `changed_when: false`; handle optional tools and document limitations.
- Explain OS support, required privileges, data collected, and operational impact for every new check.
- Keep compliance language precise: report evidence and context rather than claiming certification.
