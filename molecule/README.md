<!--
SPDX-FileCopyrightText: 2026 sudo-Tiz

SPDX-License-Identifier: AGPL-3.0-or-later
-->

# Molecule testing

The `default` scenario installs Alloy in a systemd-enabled Docker instance, seeds a log file under `/var/log`, and runs Loki as the sink. Verification queries Loki for the unique line, proving it passed through Alloy rather than merely checking that Alloy started.

Run with:

```sh
molecule test --scenario-name default
```
