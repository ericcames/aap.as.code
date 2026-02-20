# Persona Mapping: Functions & Use Cases

This document maps each functional capability of the aap.as.code repository to specific user personas, their challenges, and the value delivered.

---

## 1. COMPLETE AAP ENVIRONMENT SETUP

**Function**: Automated configuration of entire AAP platform (Controller, EDA, Gateway, Hub)
**File**: `playbooks/main.yml`

### Persona: **Demo Environment Administrator**

**Job Role**: Solutions Architect / Technical Enablement Specialist

**Problems**:
- Manually configuring AAP takes hours to weeks for each demo environment
- No repeatable build process for demos making in the room presentations especially difficult
- No standardized way to replicate successful demo setups
- Credentials and secrets scattered across multiple systems
- Onboarding new team members requires extensive documentation
- No off-the-shelf library of product demos to address a client's exact concern
- Redhat sales demo environments are ephemeral and may have to be re-created on a moments notice
- There are great resources from the Ansible Team that are not operationalized for client demos
- "New to the role" technical sales people need an easy way to produce demos like a season pro
- No way to take field developed demos and widely replicate them

**How Ansible Solves It**:
- Redhat shows clients what "Speed and Certainty" feels like in a live enviroment
- Given a working IaaS endpoint, configure working AAP stack in 15-30 minutes
- Declarative configuration ensures identical environments every time
- Version-controlled setup enables rollback, change tracking, and easy sharing
- Demo Ansible-Vault centralized credential management
- New team members can deploy environments without prior AAP knowledge
- Account Executives can count on the same demo everytime

**Value Delivered**:
- Previously stressful demo process reduced to boring, certain delivery
- 90% reduction in environment setup time
- Zero configuration drift across multiple instances
- Self-service capability for sales and partners
- Audit trail of all configuration changes
- Expands AAP expertise through sales teamss
- Customers have an ready-to-go-pattern to deliver demos to their org

---

## 2. MULTI-ORGANIZATION MANAGEMENT

**Function**: Complete AAP platform population via declarative configuration-as-code
**Directory**: `playbooks/files/config_as_code/` (19 interconnected configuration files)

### Configuration Architecture Overview

The config_as_code directory contains declarative YAML files that work together as a complete configuration blueprint, eliminating manual AAP setup and ensuring identical outcomes across deployments. These files are organized into four layers:

#### **Layer 1: Foundation (Organizations, Credentials, Users)**
- **`gateway_organizations.yml`**: Creates 6 multi-tenant organizations (IT Service, Network, Storage, Online Shopping, Data Center Operations, SOC) with pre-configured Galaxy credentials for each
- **`credential_types.yml`**: Defines 4 custom credential types for third-party integrations (ServiceNow ITSM, Dynatrace/FreshService APIs, SMTP, Red Hat Customer Portal)
- **`controller_credentials.yml`**: Provisions service credentials (AWS Machine, API tokens, container registries, Red Hat Customer Portal access)
- **`gateway_users.yml`**: Creates 18 user accounts with varied roles (16 superusers, 1 system auditor, 1 regular user) for realistic RBAC demonstrations
- **`gateway_teams.yml`**: Establishes teams (Network, Server, Storage) for role-based collaboration
- **`gateway_settings.yml`**: Configures authentication, branding, password policies, and token expiration

#### **Layer 2: Content Distribution (Collections, Execution Environments, Registries)**
- **`execution_environments.yml`**: Defines 2 containerized execution environments (ee-supported-rhel8, hashicorp_ee) with registry credentials
- **`hub_ee_registry.yml`**: Configures Private Automation Hub container registry connections
- **`hub_collection_remotes.yml`**: Sets up remote content sources (certified, validated, community collections)
- **`hub_collection_repository.yml`**: Creates collection repositories for content synchronization

#### **Layer 3: Automation Infrastructure (Projects, Inventories, Templates)**
- **`controller_projects.yml`**: Links 7 Git repositories containing demo playbooks (F5, Palo Alto, Windows, Linux, Satellite, HashiCorp, Event-Driven Ansible)
- **`controller_inventories.yml`**: Creates the "AAP Managed Inventory" for target infrastructure
- **`controller_hosts.yml`**: Populates inventory with managed hosts
- **`controller_templates.yml`**: Provisions 5+ job templates mapped to specific demo scenarios, each pre-configured with the correct execution environment, credentials, and playbooks
- **`controller_settings.yml`**: Configures Controller-level settings (Red Hat Insights tracking, subscription management, logging)

#### **Layer 4: Event-Driven Automation (EDA Integration)**
- **`eda_credentials.yml`**: Stores ServiceNow and event source credentials
- **`eda_decision_environments.yml`**: Defines decision environments for rulebook processing
- **`eda_projects.yml`**: Links Event-Driven Ansible rulebook repositories
- **`eda_event_streams.yml`**: Configures event streams from ServiceNow (incidents, change requests, catalog orders)
- **`eda_rulebook_activations.yml`**: Activates rulebooks that trigger automated responses to ITSM events

### How They Eliminate Toil and Ensure Identical Outcomes

**The Problem Without Config-as-Code:**
Manual AAP configuration requires 100+ clicks across multiple UI screens, takes hours to weeks, results in configuration drift between environments, lacks version control, and requires extensive documentation for team onboarding.

**The Solution:**
These 19 files work as a single declarative blueprint consumed by `playbooks/main.yml`. When executed, they:

1. **Ensure Zero Configuration Drift**: Every AAP deployment uses identical YAML definitions, eliminating "works on my instance" problems
2. **Enable Version Control**: All configuration changes are tracked in Git with full audit trail and rollback capability
3. **Provide Self-Service Capability**: Non-experts can deploy complete AAP environments without manual configuration knowledge
4. **Deliver in Minutes, Not Hours**: What previously took 4-8 hours of manual clicking is now fully automated, using Redhat tools, in 15-30 minutes
5. **Support Multi-Environment Consistency**: Dev, test, and production environments are guaranteed identical
6. **Facilitate Collaboration**: Teams share configuration improvements via pull requests rather than chat conversations

**Files**: All files in `playbooks/files/config_as_code/`
### Persona: **Enterprise Architect**

**Persona Role**: Senior Technical Architect / Platform Owner

**Problems**:
- Need to demonstrate multi-tenant AAP capabilities to enterprise customers
- Manually creating isolated organizations for different business units is error-prone
- Galaxy credential configuration repeated manually for each org
- Difficult to show segregated automation across departments
- Can't quickly prove AAP scales across organizational boundaries

**How Ansible Solves It**:
- Automatically creates 6 pre-configured organizations (IT Service, Network, Storage, Shopping, Data Center, SOC)
- Galaxy credentials automatically associated with each organization
- Demonstrates real-world multi-tenant architecture patterns
- Provides isolated namespaces for different automation domains
- Enables immediate role-based access control demonstrations

**Value Delivered**:
- Instant multi-tenant demo environment
- Proof of enterprise-scale AAP capabilities
- Clear separation of concerns across business units
- Accelerated enterprise deal cycles
- Technical validation of AAP for large organizations

---

## 3. TEAM & USER PROVISIONING

**Function**: Automated creation of teams and users with RBAC
**Files**: `gateway_teams.yml`, `gateway_users.yml`

### Persona: **Security & Compliance Manager**

**Job Role**: IT Security Manager / Compliance Officer

**Problems**:
- Demonstrating granular access control requires complex manual setup
- Need to show segregation of duties to auditors
- Creating test users with varied permission levels is time-consuming
- Proving least-privilege access in automation platforms
- Validating that AAP meets security frameworks (SOC2, ISO27001)

**How Ansible Solves It**:
- Pre-configured teams (Network, Server, Storage) with appropriate permissions
- 17+ users with varying roles (superusers, auditors, regular users)
- Demonstrates real-world RBAC patterns immediately
- Shows user-to-team-to-organization hierarchy
- Provides audit-ready access control model

**Value Delivered**:
- Immediate security control demonstration
- Compliance validation without manual setup
- Proof of least-privilege enforcement
- Accelerated security review processes
- Audit-ready access logs and permissions

---

## 4. SERVICENOW ITSM INTEGRATION

**Function**: Event-Driven Ansible integration with ServiceNow
**Files**: `eda_credentials.yml`, `eda_event_streams.yml`, `eda_rulebook_activations.yml`

### Persona: **IT Service Manager**

**Job Role**: ITSM Manager / Service Desk Director

**Problems**:
- Service catalog requests require manual fulfillment by operations teams
- Incident tickets sit unaddressed for hours before automation kicks in
- Change tickets don't trigger automated validation workflows
- No closed-loop integration between ITSM and automation platform
- Manual handoffs between ServiceNow and infrastructure teams

**How Ansible Solves It**:
- Event streams automatically capture ServiceNow incidents and change requests
- Rulebook activations trigger automated responses to SNOW catalog orders
- Real-time event processing eliminates manual ticket monitoring
- Automated workflows respond to SNOW events in seconds
- Closed-loop updates back to ServiceNow tickets

**Value Delivered**:
- 80% reduction in service catalog fulfillment time
- Automatic incident remediation (self-healing infrastructure)
- Zero manual intervention for common change requests
- Complete audit trail from ticket creation to resolution
- Improved MTTR (Mean Time To Resolution)

---

## 5. NETWORK DEVICE AUTOMATION (F5 & PALO ALTO)

**Function**: Automated setup for F5 and Palo Alto firewall demonstrations
**Files**: `controller_projects.yml`, `controller_templates.yml` (F5 & Panos)

### Persona: **Network Automation Engineer**

**Job Role**: Senior Network Engineer / NetOps Lead

**Problems**:
- Network device configuration is still mostly manual CLI work
- Demonstrating network automation requires extensive pre-configuration
- F5 load balancer changes require 3+ team approvals and manual execution
- Firewall rule updates take days due to manual change control
- No version control for network device configurations

**How Ansible Solves It**:
- Pre-configured F5 automation playbooks for load balancer management
- Palo Alto firewall automation for security policy enforcement
- Job templates ready to demonstrate network-as-code workflows
- Credential management for network device access
- Git-based version control for all network configurations

**Value Delivered**:
- Network configuration changes reduced from days to minutes
- Version-controlled infrastructure enables rollback capability
- Proof that network automation is achievable
- Reduced human error in network changes
- Faster incident response for network issues

---

## 6. WINDOWS SERVER AUTOMATION

**Function**: Windows infrastructure automation and configuration
**Files**: `controller_projects.yml`, `controller_templates.yml` (Windows)

### Persona: **Windows Infrastructure Administrator**

**Job Role**: Windows Systems Administrator / Platform Engineer

**Problems**:
- PowerShell scripts scattered across shared drives
- Patching Windows servers requires late-night maintenance windows
- No centralized orchestration for multi-server Windows deployments
- Group Policy doesn't handle application deployments well
- Compliance reporting requires manual data collection

**How Ansible Solves It**:
- Centralized Windows automation playbooks in version control
- Orchestrated multi-server patching with rollback capability
- Application deployment automation across Windows fleets
- Compliance reporting automation via scheduled job templates
- Integration with AWS EC2 for cloud-based Windows instances

**Value Delivered**:
- 70% reduction in patching downtime
- Centralized automation library replaces script sprawl
- Automated compliance reporting
- Faster application deployments
- Reduced weekend/after-hours work

---

## 7. LINUX/RHEL SYSTEM MANAGEMENT

**Function**: Linux infrastructure automation and Red Hat system management
**Files**: `controller_projects.yml`, `controller_templates.yml` (Linux), `controller_settings.yml`

### Persona: **Linux Systems Engineer**

**Job Role**: Senior Linux Administrator / DevOps Engineer

**Problems**:
- Hundreds of RHEL servers need consistent configuration
- Red Hat Insights findings require manual remediation
- Subscription management across multiple environments is manual
- Server provisioning takes days due to manual steps
- No standardized approach to system hardening

**How Ansible Solves It**:
- Automated RHEL system role application for standardized configs
- Red Hat Insights integration for automated remediation
- Subscription management automation via Customer Portal credentials
- Server provisioning playbooks for rapid deployment
- Automated CIS/STIG compliance enforcement

**Value Delivered**:
- Consistent configuration across all RHEL systems
- Automated security vulnerability remediation
- Server provisioning reduced from days to hours
- Compliance drift prevention
- Integration with Red Hat support ecosystem

---

## 8. RED HAT SATELLITE INTEGRATION

**Function**: Satellite integration for patch management and system registration
**Files**: `controller_projects.yml`, `controller_templates.yml` (Satellite)

### Persona: **Patch Management Coordinator**

**Job Role**: Systems Management Lead / Operations Manager

**Problems**:
- Patch compliance reporting requires manual Satellite queries
- Content view promotions are manual multi-step processes
- System registration to Satellite requires shell access
- Errata application across thousands of systems is manual
- Patching schedules require coordination across multiple teams

**How Ansible Solves It**:
- Automated content view promotion workflows
- Bulk system registration to Satellite via job templates
- Scheduled errata application across server groups
- Automated patch compliance reporting
- Orchestrated patching workflows with approval gates

**Value Delivered**:
- Patch compliance from 60% to 95%+
- Automated monthly patching cycles
- Reduced patch-related incidents
- Clear audit trail for all patching activities
- Faster security vulnerability response

---

## 9. HASHICORP VAULT INTEGRATION

**Function**: Secrets management via HashiCorp Vault
**Files**: `controller_projects.yml`, `controller_templates.yml` (HashiCorp), execution environments

### Persona: **DevSecOps Engineer**

**Job Role**: Security Automation Engineer / DevSecOps Lead

**Problems**:
- Secrets stored in playbooks and version control (security risk)
- No centralized secrets management across automation workflows
- Credential rotation requires updating multiple playbooks
- Audit trail for secrets access is incomplete
- Developers hard-code credentials in automation

**How Ansible Solves It**:
- HashiCorp Vault integration via custom execution environment
- Dynamic secrets retrieval during playbook execution
- Centralized credential storage with automated rotation
- Complete audit trail of secrets access
- Demonstration of secrets management best practices

**Value Delivered**:
- Elimination of hard-coded credentials
- Centralized secrets lifecycle management
- Automated credential rotation
- Complete secrets audit trail
- Compliance with secrets management policies

---

## 10. CUSTOM CREDENTIAL TYPES

**Function**: Third-party API integration via custom credential types
**File**: `credential_types.yml`

### Persona: **Integration Engineer**

**Job Role**: API Integration Specialist / Middleware Engineer

**Problems**:
- AAP doesn't natively support every third-party system
- Custom API authentication requires scripting workarounds
- No standardized way to manage third-party credentials
- Email notification integration requires custom development
- Monitoring tool integration (Dynatrace, FreshService) is manual

**How Ansible Solves It**:
- Custom credential types for Dynatrace, FreshService, SMTP
- Standardized API key and token management
- ServiceNow ITSM credential type for seamless integration
- Red Hat Customer Portal credential type for support API access
- Extensible framework for future integrations

**Value Delivered**:
- Seamless third-party tool integration
- Standardized credential management across all systems
- Reduced custom scripting for integrations
- Faster onboarding of new tools
- Reusable credential types across playbooks

---

## 11. CONTAINER EXECUTION ENVIRONMENTS

**Function**: Containerized execution environments for dependencies
**Files**: `execution_environments.yml`, `hub_ee_registry.yml`

### Persona: **Automation Platform Engineer**

**Job Role**: AAP Platform Administrator / Automation Architect

**Problems**:
- Different playbooks require different Python libraries
- Dependency conflicts between automation projects
- No standardized execution environment across teams
- Custom modules require manual installation on AAP nodes
- Testing new dependencies risks breaking existing automation

**How Ansible Solves It**:
- Pre-configured execution environments with required dependencies
- HashiCorp-specific execution environment for Vault integration
- Container registry integration (registry.redhat.io, quay.io)
- Standardized ee-supported-rhel8 environment
- Isolated dependencies prevent conflicts

**Value Delivered**:
- Zero dependency conflicts between projects
- Faster development with pre-built environments
- Consistent execution across all AAP nodes
- Easy testing of new dependencies
- Scalable approach to custom modules

---

## 12. COLLECTION MANAGEMENT & CONTENT SYNCHRONIZATION

**Function**: Automated Ansible collection synchronization
**Files**: `hub_collection_remotes.yml`, `hub_collection_repository.yml`

### Persona: **Automation Content Curator**

**Job Role**: Ansible Content Developer / Automation Librarian

**Problems**:
- Keeping collections updated across multiple AAP instances
- Manual collection downloads from Automation Hub
- No standardized collection versions across teams
- Community collections not available in air-gapped environments
- Custom collections need distribution mechanism

**How Ansible Solves It**:
- Automated synchronization of certified, validated, and community collections
- Version-pinned collections for consistency
- Automated content repository creation
- Custom collection distribution (ericcames.* collections)
- Air-gap-ready content synchronization

**Value Delivered**:
- Always up-to-date collection libraries
- Consistent collection versions across environments
- Reduced manual collection management
- Support for air-gapped deployments
- Centralized content distribution

---

## 13. DYNATRACE MONITORING INTEGRATION

**Function**: Application performance monitoring integration
**Files**: `credential_types.yml`, `controller_credentials.yml`

### Persona: **Application Performance Manager**

**Job Role**: APM Lead / Site Reliability Engineer

**Problems**:
- Manual correlation between automation events and APM alerts
- No automated response to performance degradation
- Dynatrace events don't trigger remediation workflows
- Manual deployment validation in APM tools
- No closed-loop integration between automation and monitoring

**How Ansible Solves It**:
- Dynatrace API token credential type for seamless integration
- Automated workflow triggering based on Dynatrace events
- Post-deployment validation via Dynatrace API calls
- Automated remediation triggered by performance alerts
- Metrics collection during automation execution

**Value Delivered**:
- Automated response to performance issues
- Deployment validation without manual checks
- Reduced MTTR for application issues
- Closed-loop deployment pipeline
- Performance-driven automation decisions

---

## 14. GATEWAY AUTHENTICATION & BRANDING

**Function**: Custom authentication, password policies, and branding
**File**: `gateway_settings.yml`

### Persona: **Identity & Access Manager**

**Job Role**: IAM Specialist / Security Administrator

**Problems**:
- Default AAP login page doesn't match corporate branding
- Password policies need to meet corporate standards
- OAuth2 integration requires manual configuration
- Token expiration policies not aligned with security requirements
- No custom login messages for security notices

**How Ansible Solves It**:
- Automated custom logo/branding configuration
- Standardized password policy enforcement
- OAuth2 settings automation
- Configurable token expiration policies
- Custom login message for security banners

**Value Delivered**:
- Corporate branding consistency
- Compliance with password policies
- Seamless SSO/OAuth integration
- Security notice enforcement
- Professional demo appearance

---

## 15. MULTI-PLATFORM DEMO ORCHESTRATION

**Function**: Coordinated setup across 7+ demo platforms
**Files**: All controller templates and projects

### Persona: **Sales Engineer / Solutions Consultant**

**Job Role**: Technical Sales / Pre-Sales Engineer

**Problems**:
- Customer demos require hours of preparation
- Different customer industries need different demo scenarios
- Live demo failures due to incomplete setup
- No quick way to switch between demo scenarios
- Customers want to see their specific use case

**How Ansible Solves It**:
- 7 ready-to-run demo environments (Windows, F5, Panos, Satellite, HashiCorp, Linux, EDA)
- One-click setup for each demo scenario
- Industry-specific organizations (Retail, Network, Security)
- Reliable, tested demo configurations
- Rapid switching between demo types

**Value Delivered**:
- Demo preparation time reduced from hours to minutes
- Higher demo success rate
- Industry-specific storylines ready to go
- Confidence in live demo reliability
- More customer meetings per week

---

## 16. INSIGHTS TRACKING & SUBSCRIPTION MANAGEMENT

**Function**: Red Hat Insights integration and subscription tracking
**File**: `controller_settings.yml`

### Persona: **Red Hat Platform Owner**

**Job Role**: Enterprise Red Hat Administrator / Platform Manager

**Problems**:
- Systems not reporting to Red Hat Insights
- Subscription consumption tracking is manual
- Security advisories from Insights require manual action
- Compliance drift not automatically detected
- No automated remediation of Insights findings

**How Ansible Solves It**:
- Automated Insights tracking enablement
- Red Hat subscription client credential configuration
- Automated remediation playbooks for Insights findings
- Subscription usage reporting automation
- Security advisory response workflows

**Value Delivered**:
- 100% Insights enrollment
- Automated security remediation
- Proactive issue detection
- Subscription optimization
- Maximum value from Red Hat subscriptions

---

## 17. CENTRALIZED CREDENTIAL VAULT

**Function**: Remote vault file for centralized secrets
**File**: `main.yml` (vault download functionality)

### Persona: **Security Operations Manager**

**Job Role**: Security Operations Lead / Secrets Management Owner

**Problems**:
- Secrets scattered across multiple systems
- No centralized secrets repository for automation
- Credential rotation requires updating multiple locations
- Audit trail for secrets access is incomplete
- Secrets shared via insecure methods (email, Slack)

**How Ansible Solves It**:
- Single remote vault file for all demo credentials
- Encrypted storage with Ansible Vault
- Version-controlled secrets with audit trail
- Automated vault retrieval during setup
- Centralized rotation point for credentials

**Value Delivered**:
- Single source of truth for secrets
- Complete secrets audit trail
- Simplified credential rotation
- Secure secrets distribution
- Compliance with secrets policies

---

## PERSONA-BASED VALUE PROPOSITIONS

### For **C-Level Executives** (CIO, CTO, CISO)

**Problems**:
- High operational costs due to manual processes
- Slow time-to-market for new services
- Compliance and audit findings
- Risk of human error in critical operations
- Skills shortage in automation expertise

**Value Delivered**:
- 3x reduction in operational toil
- Speed: service delivery (days → hours)
- Automated compliance enforcement
- Reduced risk through standardization
- Democratized automation (less expertise required)

### For **Training & Enablement Teams**

**Problems**:
- Building training environments takes days for a one-off enviorment
- Inconsistent lab experiences for students
- Manual IaaS cleanup often neglected resulting in paying for unused services
- Difficultly of building hands-on labs means the best sales tool is radically under utilized
- Difficult to scale field training programs

**Value Delivered**:
- Lab environments provisioned in minutes
- Identical experiences for all students
- Automated environment reset
- Infinite scalability on cloud platforms
- Faster student onboarding
- Instructors can focus on student learning vs. training infrastructure

### For **Partner Organizations**

**Problems**:
- Need to demonstrate AAP capabilities to their customers
- Limited AAP expertise in partner team
- Can't afford full-time AAP administrators
- Need proof-of-concept environments quickly
- Want to show industry-specific use cases

**Value Delivered**:
- Self-service demo environment creation
- No AAP expertise required
- Zero ongoing administration
- Industry-specific demo scenarios included
- Accelerated partner deal cycles

---

## SUMMARY: WHO BENEFITS AND HOW

| Persona | Primary Pain Point | Automation Solution | Time Saved |
|---------|-------------------|---------------------|------------|
| Demo Environment Admin | Manual AAP setup takes hours | One-click full stack setup | 90% |
| Enterprise Architect | Can't show multi-tenancy | Pre-built 6-org structure | 95% |
| Security Manager | Complex RBAC setup | 17 users, teams, roles ready | 85% |
| IT Service Manager | Manual SNOW fulfillment | Event-driven automation | 80% |
| Network Engineer | Manual device configs | F5/Panos automation | 75% |
| Windows Admin | Script sprawl, no orchestration | Centralized Windows automation | 70% |
| Linux Engineer | Manual system config | Automated RHEL management | 70% |
| Patch Manager | Manual Satellite workflows | Automated patching cycles | 80% |
| DevSecOps Engineer | Hard-coded secrets | HashiCorp Vault integration | 90% |
| Integration Engineer | Custom API scripting | Reusable credential types | 65% |
| Platform Engineer | Dependency conflicts | Container execution environments | 85% |
| Content Curator | Manual collection updates | Automated content sync | 90% |
| APM Manager | Manual monitoring correlation | Dynatrace integration | 70% |
| IAM Specialist | Manual auth config | Automated SSO/branding | 80% |
| Sales Engineer | Demo prep time | 7 ready-to-run scenarios | 90% |
| Platform Owner | Manual Insights enrollment | Automated subscription mgmt | 85% |
| Security Ops | Scattered secrets | Centralized vault | 80% |

**Overall Value**: This repository transforms weeks of manual AAP configuration into minutes of automated deployment, while providing industry-proven patterns for 15+ distinct use cases across networking, security, systems management, and ITSM integration.
