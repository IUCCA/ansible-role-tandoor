<!--
SPDX-FileCopyrightText: 2024 Slavi Pantaleev
SPDX-FileCopyrightText: 2024 IUCCA
SPDX-FileCopyrightText: 2026 Suguru Hirahara

SPDX-License-Identifier: AGPL-3.0-or-later
-->

# Tandoor Ansible role

This is an [Ansible](https://www.ansible.com/) role which installs [Tandoor](https://docs.tandoor.dev/) to run as a [Docker](https://www.docker.com/) container wrapped in a systemd service.

This role *implicitly* depends on:

- [`com.devture.ansible.role.playbook_help`](https://github.com/devture/com.devture.ansible.role.playbook_help)
- [`com.devture.ansible.role.systemd_docker_base`](https://github.com/devture/com.devture.ansible.role.systemd_docker_base)

Check [`defaults/main.yml`](defaults/main.yml) for the full list of supported options.

💡 For an Ansible playbook which integrates this role and makes it easier to use, see the [Mother-of-All-Self-Hosting Ansible playbook](https://github.com/mother-of-all-self-hosting/mash-playbook).

>[!NOTE]
> While the [LICENSE](https://github.com/TandoorRecipes/recipes/blob/b8b8a773e7548962723f7a76dcca731ca1dad96d/LICENSE.md) file of the software contains “Commons Clause” License Condition v1.0 as of writing, that additional term constitutes one of the "further restrictions" and may be removed. Refer to [this article](https://www.fsf.org/blogs/licensing/protecting-free-software-against-confusing-additional-restrictions) by FSF for relevant information.

## Development

You can optionally install a Git pre-commit hook (via [mise](https://mise.jdx.dev/) + [prek](https://prek.j178.dev/)) that runs formatting and linting checks before each commit. See [`.pre-commit-config.yaml`](./.pre-commit-config.yaml) for which hooks are to be executed.

To install the hook, run the [`just`](https://github.com/casey/just) command below:

```sh
just prek-install-git-pre-commit-hook
```
