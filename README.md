<!--
SPDX-FileCopyrightText: 2024, 2026 sudo-Tiz
SPDX-FileCopyrightText: 2025, 2026 Suguru Hirahara

SPDX-License-Identifier: AGPL-3.0-or-later
-->

# Alloy Ansible role

This is an [Ansible](https://www.ansible.com/) role which installs [Grafana Alloy](https://grafana.com/docs/alloy/latest/) to run as a [Docker](https://www.docker.com/) container wrapped in a systemd service.

Alloy is Grafana's OpenTelemetry Collector distribution and the successor to [Promtail](https://grafana.com/docs/loki/latest/send-data/promtail/) (which reached end-of-life status on March 2, 2026). This role currently configures Alloy to collect logs (from the systemd journal and/or plain-text log files under `/var/log`) and forward them to one or more [Grafana Loki](https://grafana.com/oss/loki/)-compatible endpoints, mirroring what the Promtail role used to do. Alloy is capable of a lot more than this (metrics, traces, and many other data sources), and you can extend the generated configuration with your own additional components via `alloy_configuration_extension_alloy`.

See [`docs/configuring-alloy.md`](docs/configuring-alloy.md) for configuration examples and source-filtering guidance.

This role *implicitly* depends on:

- [`com.devture.ansible.role.playbook_help`](https://github.com/devture/com.devture.ansible.role.playbook_help)
- [`com.devture.ansible.role.systemd_docker_base`](https://github.com/devture/com.devture.ansible.role.systemd_docker_base)

Check [`defaults/main.yml`](defaults/main.yml) for the full list of supported options.

💡 For an Ansible playbook which integrates this role and makes it easier to use, see the [Mother-of-All-Self-Hosting Ansible playbook](https://github.com/mother-of-all-self-hosting/mash-playbook).

## Development

You can optionally install a Git pre-commit hook (via [mise](https://mise.jdx.dev/) + [prek](https://prek.j178.dev/)) that runs formatting and linting checks before each commit. See [`.pre-commit-config.yaml`](./.pre-commit-config.yaml) for which hooks are to be executed.

To install the hook, run the [`just`](https://github.com/casey/just) command below:

```sh
just prek-install-git-pre-commit-hook
```

See [`molecule/README.md`](molecule/README.md) for the end-to-end test that verifies a seeded log reaches Loki.
