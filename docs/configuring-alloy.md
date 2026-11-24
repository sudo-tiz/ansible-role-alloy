<!--
SPDX-FileCopyrightText: 2026 sudo-Tiz

SPDX-License-Identifier: AGPL-3.0-or-later
-->

# Configuring Alloy

The role runs Alloy in a Docker container managed by systemd. It can read the systemd journal and files under `/var/log`; no sources are enabled by default.

## Send logs to Loki

When using the role directly, define at least one Loki-compatible client:

```yaml
alloy_config_clients_custom:
  - url: https://loki.example.com/loki/api/v1/push
    tenant_id: example
```

For the MASH playbook, enabling both Alloy and its local Loki component wires the endpoint automatically. Use `alloy_config_clients_custom` for additional endpoints or a remote Loki instance.

## Choose log sources

Enable the systemd journal or `/var/log` file scraper explicitly:

```yaml
alloy_journald_scraper_enabled: true
alloy_varlog_scraper_enabled: true
```

The journal scraper keeps all systemd units by default, except Alloy itself. Set `alloy_journald_scraper_config_relabel_configs_keep_regex` to an allowlist when only selected units should be collected. The file scraper matches files under `/var/log` ending in `log` by default, excluding `lastlog` and `faillog`; adjust `alloy_varlog_scraper_config_labels_path` for a narrower or broader glob. Custom scraper names must be Alloy identifiers: begin with a letter or underscore, then use only letters, digits, or underscores.

Custom container mounts use Docker `--mount` definition objects:

```yaml
alloy_container_additional_volumes_custom:
  - type: bind
    src: /host/path
    dst: /container/path
    options: readonly
```

Pass extra `alloy run` arguments through `alloy_process_extra_arguments_custom`.

Collected logs can contain credentials or personal data. Limit sources and labels, protect the Loki endpoint, and choose a retention period appropriate for the data.

The role does not discover Docker container logs by default. Alloy can be extended with additional components through `alloy_configuration_extension_alloy`; any Docker socket mount should be treated as a highly privileged access path.

## Extend Alloy configuration

`alloy_configuration_extension_alloy` is appended to the role-generated configuration and can define additional Alloy components. For the full set of role options, see [`defaults/main.yml`](../defaults/main.yml) and the [Alloy documentation](https://grafana.com/docs/alloy/latest/).

Do not expose Alloy's API or UI publicly unless access is appropriately restricted. The role's Traefik integration can expose its metrics endpoint separately.
