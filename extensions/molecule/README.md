# Molecule tests

Integration tests for this collection using [Molecule](https://docs.ansible.com/projects/molecule/) with [Docker containers](https://docs.ansible.com/projects/molecule/examples/docker/).

## Why `extensions/molecule/`?

This repository is an Ansible **collection** (`galaxy.yml` with `namespace` / `name`), not a standalone role.

Molecule’s collection mode expects scenarios under `extensions/molecule/`. When it detects `galaxy.yml`, it discovers scenarios here and treats the repo as the collection under test. That is the layout described in the [Molecule collection testing docs](https://docs.ansible.com/projects/molecule/getting-started-collections/).

The older per-role path (`roles/<role>/molecule/`) is for testing a single role in isolation. It still works, but for a multi-role collection it means repeating Docker create/destroy under each role and losing shared collection-level config.

## Layout

```text
extensions/molecule/
├── README.md                      # this file
├── config.yml                     # shared Molecule defaults (prerun off, no shared_state)
├── requirements.txt               # Python packages for the controller
├── default/
│   └── molecule.yml               # stub scenario (keeps collection mode quiet)
├── shared/
│   ├── requirements.yml           # Galaxy collections (community.docker)
│   ├── roles-requirements.yml     # empty roles file (avoids dependency "missing")
│   ├── create.yml                 # start Docker hosts + dynamic inventory
│   ├── destroy.yml                # remove containers + inventory
│   └── tasks/
│       └── create-fail.yml        # dump docker logs if create fails
├── auth_get_access_token/         # scenario: token from role can call Microsoft Graph
│   ├── molecule.yml               # Docker platform, shared create/destroy, test sequence
│   ├── converge.yml               # assert env creds; run auth_get_access_token on the container
│   └── verify.yml                 # re-run role; assert Bearer token; GET graph.microsoft.com/v1.0/ → 200
└── send_email/                    # scenario: send_email works to a null/discard recipient
    ├── molecule.yml               # Docker platform, shared create/destroy, test sequence
    ├── converge.yml               # assert env creds + mailboxes; run send_email on the container
    └── verify.yml                 # re-run send_email; assert Graph sendMail returned HTTP 202
```

## What each file does

### Shared / top-level

| File | Role |
| --- | --- |
| `config.yml` | Defaults for every scenario: skip prerun (so ansible-compat does not install `collections/requirements.yml` from Automation Hub), and disable shared state so each scenario owns its own container. |
| `requirements.txt` | Python packages on the **controller** (`molecule`, `ansible-core`, `docker`). Not consumed by Molecule itself — install with `pip`. |
| `default/molecule.yml` | Stub so Molecule collection mode stays quiet. Empty test sequence; no real test. |
| `shared/requirements.yml` | Ansible Galaxy **collections** deps for the scenarios — mainly `community.docker`. |
| `shared/roles-requirements.yml` | Empty Galaxy **roles** requirements file so Molecule’s Roles invoker does not report missing. |
| `shared/create.yml` | Starts the Docker container(s) from `platforms:` in `molecule.yml`, builds a dynamic inventory with `ansible_connection: community.docker.docker`, and writes it into Molecule’s ephemeral directory. |
| `shared/tasks/create-fail.yml` | If a container fails to start, dumps `docker logs` and fails the create step. |
| `shared/destroy.yml` | Stops/removes the containers and deletes the dynamic inventory file. |

### Per-scenario (`auth_get_access_token/` and `send_email/`)

| File | Role |
| --- | --- |
| `molecule.yml` | Scenario config: Docker image/name, where create/destroy live, role path, and the test sequence (dependency → destroy → syntax → create → converge → verify → destroy). No `cleanup` step — there is no `cleanup.yml`. |
| `converge.yml` | Applies the role under test on the Molecule Docker host (Graph credentials from env vars). |
| `verify.yml` | Assertions after converge — token works against Graph, or `sendMail` returns 202 to the null recipient. |

## Scenarios

| Scenario | What it checks |
| --- | --- |
| `auth_get_access_token` | Role returns a Bearer token, then `GET https://graph.microsoft.com/v1.0/` succeeds with it. |
| `send_email` | Role sends mail via Graph to a null/discard recipient (`MOLECULE_RECIPIENT_EMAIL`) and gets HTTP 202. |

Each scenario spins up its own `python:3.12-bookworm` container via `community.docker`, runs the role over `community.docker.docker`, then tears the container down.

## How a test run works

When you run `molecule test -s auth_get_access_token`:

1. **dependency** — installs Galaxy collections/roles from `shared/`
2. **destroy** — clears any leftover container from a previous run
3. **syntax** — syntax-checks `converge.yml`
4. **create** — `shared/create.yml` starts the container
5. **converge** — `auth_get_access_token/converge.yml` runs the role inside it
6. **verify** — `auth_get_access_token/verify.yml` checks the outcome
7. **destroy** — `shared/destroy.yml` removes the container

The same flow applies to `send_email`, with that scenario’s converge/verify playbooks.

## Prerequisites

- A **Docker engine must be running** on the machine where you invoke Molecule (local Docker Desktop/Engine, or CI with Docker available). Scenarios create and destroy containers via `community.docker`.
- Python 3 with a virtualenv
- Entra ID app registration with Graph permissions appropriate to the scenario (`Mail.Send` for `send_email`)

## Setup

Confirm Docker is up (`docker info` should succeed), then:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r extensions/molecule/requirements.txt
```

## Required environment variables

### Both scenarios

| Variable | Purpose |
| --- | --- |
| `MOLECULE_TENANT_ID` | Entra ID tenant ID |
| `MOLECULE_CLIENT_ID` | App registration client ID |
| `MOLECULE_CLIENT_SECRET` | App registration client secret |

### `send_email` only

| Variable | Purpose |
| --- | --- |
| `MOLECULE_SENDER_EMAIL` | Mailbox used as the Graph `sendMail` sender (needs `Mail.Send`) |
| `MOLECULE_RECIPIENT_EMAIL` | Null/discard address that safely absorbs test mail |

## Run

From the collection root:

```bash
export MOLECULE_TENANT_ID=...
export MOLECULE_CLIENT_ID=...
export MOLECULE_CLIENT_SECRET=...

molecule test -s auth_get_access_token

export MOLECULE_SENDER_EMAIL=...
export MOLECULE_RECIPIENT_EMAIL=...
molecule test -s send_email

# or both component scenarios
molecule test --all --exclude default
```

Useful partial commands:

```bash
molecule list
molecule create -s auth_get_access_token
molecule converge -s auth_get_access_token
molecule verify -s auth_get_access_token
molecule destroy -s auth_get_access_token
```

## Documentation references

- [Molecule documentation](https://docs.ansible.com/projects/molecule/)
- [Collection testing with Molecule](https://docs.ansible.com/projects/molecule/getting-started-collections/) — why scenarios live under `extensions/molecule/`
- [Using Docker containers](https://docs.ansible.com/projects/molecule/examples/docker/) — pattern used by `shared/create.yml` and `shared/destroy.yml`
- [Molecule configuration](https://docs.ansible.com/projects/molecule/configuration/) — `molecule.yml` options, env vars (`MOLECULE_PROJECT_DIRECTORY`, etc.)
- [community.docker collection](https://docs.ansible.com/projects/ansible/latest/collections/community/docker/) — `docker_container` module and `community.docker.docker` connection
- [Microsoft Graph auth (client credentials)](https://learn.microsoft.com/en-us/graph/auth-v2-service)
- [Microsoft Graph sendMail API](https://learn.microsoft.com/en-us/graph/api/user-sendmail)
