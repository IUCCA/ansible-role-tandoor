<!--
SPDX-FileCopyrightText: 2018-2025 Slavi Pantaleev
SPDX-FileCopyrightText: 2019-2022 Aaron Raimist
SPDX-FileCopyrightText: 2019-2023 MDAD project contributors
SPDX-FileCopyrightText: 2023 QEDeD
SPDX-FileCopyrightText: 2024 Fabio Bonelli
SPDX-FileCopyrightText: 2024 Nikita Chernyi
SPDX-FileCopyrightText: 2024-2026 Suguru Hirahara
SPDX-FileCopyrightText: 2026 spatterlight

SPDX-License-Identifier: AGPL-3.0-or-later
-->

# Molecule Testing

This role supports [Molecule](https://docs.ansible.com/projects/molecule/), an Ansible testing framework designed for developing and testing Ansible collections, playbooks, and roles.

## Prerequisites

To utilize Molecule you need to prepare several requirements:

- **x86** computer running one of these operating systems that make use of [systemd](https://systemd.io/):
  - **Archlinux**
  - **CentOS**, **Rocky Linux**, **AlmaLinux**, or possibly other RHEL alternatives (although your mileage may vary)
  - **Debian** (10/Buster or newer)
  - **Ubuntu** (18.04 or newer, although [20.04 may be problematic](https://github.com/mother-of-all-self-hosting/mash-playbook/blob/main/docs/ansible.md#supported-ansible-versions) if you run the Ansible playbook on it)
- `root` access on the computer which Molecule runs against
- [Ansible](http://ansible.com/) program
- [Python](https://www.python.org/)
  - Most distributions install Python by default, but some don't (e.g. Ubuntu 18.04) and require manual installation (something like `apt-get install python3`)
- [Docker](https://www.docker.com)
  - Access to Docker UNIX socket (`/var/run/docker.sock`) is required by default

## Installation

To set up the environment for using Molecule, run the command below on the terminal:

```bash
python3 -m venv ./molecule/venv
source ./molecule/venv/bin/activate
pip3 install -r ./molecule/requirements.txt
```

## Scenarios

Currently these testing scenarios are available:

### `default`

Tests a Tandoor installation backed by SQLite, with Traefik labels turned on.

Besides the checks shared by both scenarios, it counts the migrations Django recorded in the SQLite file inside the container (so that "SQLite is configured" means "SQLite is what holds the data"), checks that no Postgres settings were rendered into the environment file, and reads the container label file to confirm that Traefik is told to route to the port Tandoor was told to listen on.

### `postgres`

Tests a Tandoor installation backed by Postgres, with Traefik labels turned off and Tandoor's own webserver moved off its default port.

Besides the checks shared by both scenarios, it queries the Postgres database as the role's own database user for the migrations Django recorded there, confirms that Tandoor wrote no SQLite file to fall back on, and confirms that no Traefik label was written. Reaching Tandoor at all in this scenario proves that `tandoor_container_http_port` reached the webserver inside the container rather than only the published port and the Traefik labels.

### Shared checks

[`verify_tasks.yml`](verify_tasks.yml) holds what both scenarios check:

- the systemd service is active *and* has not been restarting (the unit carries `Restart=always`, so "active" alone says nothing about a container that dies on every boot)
- Tandoor answers `/` with a redirect to `/setup/`, which it can only decide after asking its database whether an account exists
- Tandoor answers a request for a hostname the role did not configure with a 400, which is what makes the check above evidence that the role's environment file reached Django
- the version Tandoor reports over its OpenAPI schema is the one `tandoor_version` pins in `defaults/main.yml`

## Running

By default it is configured to run the scenarios on Ubuntu 26.04.

```bash
molecule test --scenario-name default
```

You can utilize other distributions by setting one to the `MOLECULE_DISTRO` environment variable:

```bash
# Ubuntu 24.04
MOLECULE_DISTRO=ubuntu2404 molecule test --scenario-name default

# Debian 13
MOLECULE_DISTRO=debian13 molecule test --scenario-name default

# Debian 12
MOLECULE_DISTRO=debian12 molecule test --scenario-name default
```
