# Repository Overview

## Purpose

This repository is an Ansible-based **Configuration as Code (CaC)** system designed to automate the setup and management of demo environments on the Red Hat Demo Platform. It serves as the foundation for the Ansible Product Demos catalog item, enabling quick deployment of various automation demonstrations.

## What This Repository Does

The `aap.as.code` repository:

- **Automates AAP Configuration**: Provisions and configures Ansible Automation Platform (AAP) instances with predefined templates, credentials, inventories, and projects
- **Enables Demo Orchestration**: Loads setup templates that pull in other specialized demo repositories
- **Manages Configuration as Code**: Maintains all configuration definitions in version-controlled YAML files
- **Integrates ServiceNow & EDA**: Provides Event-Driven Ansible integration with ServiceNow for automated incident response

## Why This Repository Exists

Organizations and sales engineers need to quickly spin up working Ansible demonstrations on the Red Hat Demo Platform. Rather than manually configuring each AAP instance, this repository provides:

- **Consistency**: Every demo environment is configured identically
- **Speed**: Automated setup reduces deployment time from hours to minutes
- **Repeatability**: Infrastructure as Code ensures reproducible environments
- **Flexibility**: Modular design allows loading different demo scenarios

## Architecture Pattern

The project follows an **Configuration as Code (CaC)** pattern where:

- All infrastructure and configuration is defined in code
- Changes are version-controlled through Git
- Ansible playbooks manage the entire lifecycle
- Configuration is declarative rather than imperative

**Entry Point**: `playbooks/main.yml`

## Key Components

### Configuration as Code Files

Located in `playbooks/files/config_as_code/`, these define:

- **Controller Configuration**:
  - `controller_credentials.yml` - Authentication credentials
  - `controller_hosts.yml` - Managed hosts
  - `controller_inventories.yml` - Host groupings
  - `controller_projects.yml` - Source code repositories
  - `controller_templates.yml` - Job templates
  - `controller_settings.yml` - Platform settings

- **Event-Driven Ansible**:
  - `eda_credentials.yml` - EDA authentication
  - `eda_decision_environments.yml` - Decision environment containers
  - `eda_event_streams.yml` - Event source configurations
  - `eda_projects.yml` - Rulebook repositories
  - `eda_rulebook_activations.yml` - Active event-response rules

- **Gateway & Access**:
  - `gateway_organizations.yml` - Organization structure
  - `gateway_settings.yml` - Gateway configuration
  - `gateway_teams.yml` - Team definitions
  - `gateway_users.yml` - User accounts

### Related Demo Repositories

This repository integrates with specialized daily demos:

- [AAP Daily Demo Windows](https://github.com/ericcames/aap.dailydemo.windows) - Windows automation scenarios
- [AAP Daily Demo Linux](https://github.com/ericcames/aap.dailydemo.linux) - Linux system management
- [AAP Daily Demo F5](https://github.com/ericcames/aap.dailydemo.F5) - F5 load balancer automation
- [AAP Daily Demo Panos](https://github.com/ericcames/aap.dailydemo.Panos) - Palo Alto firewall automation
- [AAP Daily Demo Satellite](https://github.com/ericcames/aap.dailydemo.satellite) - Red Hat Satellite integration
- [AAP Daily Demo Hashicorp](https://github.com/ericcames/aap.dailydemo.hashicorp) - Vault and Terraform integration

## How It Works

### Prerequisites

1. **Automation Hub Access**:
   - Certified content: `https://console.redhat.com/api/automation-hub/content/published/`
   - Validated content: `https://console.redhat.com/api/automation-hub/content/validated/`
   - API token for authentication

2. **Secrets Management**:
   - Vault credential in AAP
   - Remote vault file with encrypted secrets
   - Public SSH key in a publicly accessible repository

3. **Red Hat Demo Platform**:
   - Access to the Ansible Product Demos catalog item
   - AAP instance provisioned and accessible

### Deployment Process

1. **Configure Credentials**: Set up Automation Hub (certified and validated) credentials in AAP
2. **Link to Organization**: Associate Galaxy credentials with the Default Organization
3. **Create Vault Credential**: Configure vault password for decrypting secrets
4. **Create Project**: Point AAP to this Git repository
5. **Prepare Remote Resources**: Host vault file and SSH public key in accessible locations
6. **Create Job Template**: Configure template with required extra variables
7. **Execute**: Launch the job template to provision the demo environment

### Required Extra Variables

```yaml
my_windows_catalog_short_description: "Your Catalog Description"
my_aap_url: "URL from Red Hat Demo Platform"
my_ctrl_admin_password: "Password from Red Hat Demo Platform"
my_vault: "Your Vault Credential Name"
my_remote_vault: "https://url-to-your-vault-file.yml"
my_remote_ssh_pub_key: "https://url-to-your-public-key.pub"
```

## ServiceNow Integration

The repository includes extensive Event-Driven Ansible integration with ServiceNow:

- **Event Streams**: Listen for ServiceNow incidents and changes
- **Rulebook Activations**: Automated response to SNOW events
- **Decision Environments**: Containerized rulebook execution
- **Documentation**: See `doc/AAP_Servicenow.pdf` for detailed setup

Check `playbooks/files/config_as_code/eda_*` files for EDA configuration examples.

## Documentation Structure

- **README.md**: Setup and deployment instructions
- **docs/index.md**: Documentation index and quick reference
- **docs/project-overview.md**: Executive summary
- **docs/architecture.md**: Technical architecture details
- **docs/development-guide.md**: Developer setup process
- **docs/deployment-guide.md**: Deployment instructions
- **docs/**: ServiceNow integration guides (PDFs and external links)

## Technology Stack

- **Orchestration**: Ansible Automation Platform
- **Configuration Management**: Ansible playbooks and roles
- **Event-Driven Automation**: Event-Driven Ansible (EDA)
- **Integration**: ServiceNow, AWS, various network devices
- **Version Control**: Git/GitHub
- **Secrets Management**: Ansible Vault, remote vault files

## Repository Type

**Monolithic** - All configuration and playbooks are maintained in a single repository for simplified management and deployment.

## Getting Started

For detailed setup instructions, see:
- [README.md](README.md) - Complete setup walkthrough
- [docs/development-guide.md](docs/development-guide.md) - Development environment setup
- [docs/deployment-guide.md](docs/deployment-guide.md) - Deployment on Red Hat Demo Platform

## Target Audience

- Red Hat sales engineers demonstrating AAP capabilities
- Solution architects building proof-of-concepts
- Partners showcasing Red Hat Ansible automation
- Training and education teams delivering hands-on labs
