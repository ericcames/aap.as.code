# Architecture

## Executive Summary

This project is an Ansible-based "Infrastructure as Code" (IaC) repository. It is designed to be used with the Ansible Automation Platform to set up and manage demo environments on the Red Hat Demo Platform.

## Technology Stack

| Category                  | Technology | Version         | Justification                                                                |
| ------------------------- | ---------- | --------------- | ---------------------------------------------------------------------------- |
| Orchestration/Automation | Ansible    | Not specified   | ansible.cfg, ansible-navigator.yml, galaxy.yml, playbooks/, collections/ |

## Architecture Pattern

The project follows an **Infrastructure as Code (IaC)** pattern. All infrastructure and configuration is defined in code and managed through Ansible playbooks.

## Source Tree

The source tree is organized as follows:

```
project-root/
├── playbooks/         # Ansible playbooks
│   ├── main.yml       # Main playbook entry point
│   └── files/         # Files used by playbooks
│       └── config_as_code/ # Configuration as code files
│           ├── controller_credentials.yml
│           ├── controller_hosts.yml
│           ├── controller_inventories.yml
│           ├── controller_projects.yml
│           ├── controller_settings.yml
│           ├── controller_templates.yml
│           ├── credential_types.yml
│           ├── eda_credentials.yml
│           ├── eda_decision_environments.yml
│           ├── eda_event_streams.yml
│           ├── eda_projects.yml
│           ├── eda_rulebook_activations.yml
│           ├── execution_environments.yml
│           ├── gateway_organizations.yml
│           ├── gateway_settings.yml
│           ├── gateway_teams.yml
│           └── gateway_users.yml
```

## Development Workflow

See `development-guide.md` for details on the development workflow.

## Deployment Architecture

See `deployment-guide.md` for details on the deployment architecture.
