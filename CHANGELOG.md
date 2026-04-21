# Changelog

All notable changes to this project will be documented in this file.

## [Unreleased]

### Added
- `CLAUDE.md` — Claude-specific guidelines covering new AAP environment setup, bootstrap playbook usage, collection install steps, key files, and project conventions

### Added
- `ROADMAP.md` — DC1 strategic roadmap capturing architecture layers, migration sequence, phase plan, known F5 issues, and related repos
- `playbooks/bootstrap_dev.yml` — automates Phase 1 dev environment setup (Hub credentials, Vault credential, project, job template) against a fresh AAP instance
- Inline comments in `bootstrap_dev.yml` guide new users on where to store their Automation Hub token (`~/.ansible/ansible.cfg`), vault password (`~/.ansible/secrets2`), and how to host their remote vault and SSH key files

### Changed
- `playbooks/bootstrap_dev.yml` — migrated from `ansible.controller` to `ansible.platform` modules; bootstrap token is now created at start and deleted in an `always:` block to prevent stale token accumulation
- `docs/dev-environment.md` — local dev credentials file (excluded from git via .gitignore)

### Changed
- `.gitignore` — exclude `docs/dev-environment.md` from version control
