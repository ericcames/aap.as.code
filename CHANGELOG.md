# Changelog

All notable changes to this project will be documented in this file.

## [Unreleased]

### Changed
- `playbooks/bootstrap_dev.yml` — refactored to be inventory-driven; all environment vars moved to `inventories/rhdp-<customer>-<demo>/group_vars/all.yml` (closes #152)
- `playbooks/bootstrap_dev.yml` — changed `hosts: localhost` to `hosts: all` to support inventory targeting

### Added
- `inventories/rhdp-sample-demo/` — sample inventory template for new RHDP environments; copy and rename for each customer/demo combination

### Fixed
- `playbooks/bootstrap_dev.yml` — changed `scm_branch` from hardcoded `ericames/productdemo` to `main` (fixes #150)

### Changed
- `ROADMAP.md` — marked issues #14, #18, #21, #23 resolved in Phase 1 checklist and Known Issues table; added issue links

### Added
- `CLAUDE.md` — Claude-specific guidelines covering new AAP environment setup, bootstrap playbook usage, collection install steps, key files, and project conventions

### Added
- `ROADMAP.md` — DC1 strategic roadmap capturing architecture layers, migration sequence, phase plan, known F5 issues, and related repos
- `playbooks/bootstrap_dev.yml` — automates Phase 1 dev environment setup (Hub credentials, Vault credential, project, job template) against a fresh AAP instance
- Inline comments in `bootstrap_dev.yml` guide new users on where to store their Automation Hub token (`~/.ansible/ansible.cfg`), vault password (`~/.ansible/secrets2`), and how to host their remote vault and SSH key files

### Changed
- `playbooks/bootstrap_dev.yml` — corrected module usage: `ansible.controller` with `controller_oauthtoken` for credential/organization/project/job_template (no `ansible.platform` equivalents exist); `ansible.platform.token` used for token lifecycle only
- `docs/dev-environment.md` — local dev credentials file (excluded from git via .gitignore)

### Changed
- `.gitignore` — exclude `docs/dev-environment.md` from version control
