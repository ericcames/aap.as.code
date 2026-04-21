# Changelog

All notable changes to this project will be documented in this file.

## [Unreleased]

### Added
- `ROADMAP.md` — DC1 strategic roadmap capturing architecture layers, migration sequence, phase plan, known F5 issues, and related repos
- `playbooks/bootstrap_dev.yml` — automates Phase 1 dev environment setup (Hub credentials, Vault credential, project, job template) against a fresh AAP instance
- `docs/dev-environment.md` — local dev credentials file (excluded from git via .gitignore)

### Changed
- `.gitignore` — exclude `docs/dev-environment.md` from version control
