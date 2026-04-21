# aap.as.code — Claude Guidelines

## Working with a New AAP Environment

When a user provides a new AAP URL and password, update `docs/dev-environment.md`
with the new values before running anything. This file is excluded from git — never
commit credentials and never share them in chat.

```
docs/dev-environment.md
├── URL      → new AAP instance URL
├── Username → admin (default)
└── Password → provided password
```

## Bootstrapping a Fresh AAP Instance

A fresh AAP instance has none of the required credentials, projects, or job templates.
Run `playbooks/bootstrap_dev.yml` first — it creates the prerequisites automatically.

### Prerequisites (one-time local setup)

Install the required collections locally before running the playbook:

```bash
ANSIBLE_CONFIG=~/.ansible/ansible.cfg \
  ansible-galaxy collection install ansible.platform ansible.controller \
  -p ./collections
```

Your `~/.ansible/ansible.cfg` must have a valid Automation Hub token under
`[galaxy_server.rh_certified]`. Obtain it from:
`console.redhat.com → Automation Hub → Connect to Hub → API token`

### Running the Bootstrap

```bash
export CONTROLLER_HOST=<new AAP URL>
export CONTROLLER_USERNAME=admin
export CONTROLLER_PASSWORD=<new password>
ansible-playbook playbooks/bootstrap_dev.yml
```

The playbook creates:
- Automation Hub certified and validated credentials
- Galaxy credentials associated with the Default Organization
- Eric Ames vault credential (reads password from `~/.ansible/secrets2`)
- `aap.as.code` project
- `Setup - AAP - CAC` job template

The bootstrap token is automatically deleted when the playbook completes
(even on failure) to prevent stale token accumulation.

### After Bootstrap — Run Setup

Once bootstrap completes, launch `Setup - AAP - CAC` from AAP to load all
demo configurations via CaC.

## Key Files

| File | Purpose |
|------|---------|
| `playbooks/bootstrap_dev.yml` | Automates Phase 1 AAP setup from localhost |
| `playbooks/main.yml` | Main CaC setup playbook (runs inside AAP) |
| `docs/dev-environment.md` | Local dev credentials — gitignored, never commit |
| `ROADMAP.md` | DC1 strategic roadmap and migration status |
| `CHANGELOG.md` | Record of all changes — always update before committing |

## Conventions

- Always update `CHANGELOG.md` before committing changes
- Document issues in GitHub before making fixes
- One fix per branch and PR
- Use `ansible.platform` modules where available; fall back to `ansible.controller`
  only when no platform equivalent exists
- Any playbook that creates a token must delete it in an `always:` block
