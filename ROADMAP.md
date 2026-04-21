# Datacenter 1 — Demo Platform Roadmap

## Vision

Datacenter 1 (`demo.datacenter`) is the next generation of the Red Hat AAP sales demo
platform. Rather than a collection of isolated per-technology demos, it is a unified
virtual datacenter running on AWS that serves as a shared infrastructure foundation for
all demo scenarios. Individual technology demos (F5, Palo Alto, Windows, Linux, Satellite)
become workloads and Day 2 operating scenarios running on top of a common, realistic
datacenter environment.

This approach tells a stronger customer story:

> *"We built your datacenter with code. Now watch us operate it."*

The platform is managed entirely through Configuration as Code (CaC) using the
`infra.aap_configuration` collection and bootstrapped via this repo.

---

## Guiding Principles

- **Additive migration only** — old demos remain runnable until the DC1 version is
  proven. No demo capability is removed until its replacement passes a full smoke test.
- **Layer order is enforced** — each layer must be stable before building on top of it.
  Skipping layers creates the same intermittent bugs we are migrating away from.
- **aap.as.code is the safety net** — both old demos and DC1 are available from the
  bootstrap repo throughout the migration.
- **Validation is not optional** — every setup playbook ends with an explicit post-setup
  verification task. Green means actually green.
- **CaC for everything** — every AAP object (credential, project, template, workflow,
  inventory) is defined in code and reproducible from scratch.

---

## Architecture

DC1 is a layered stack. Each layer is a prerequisite for the layers above it.

```
┌─────────────────────────────────────────────────────────────────┐
│  Layer 4 — Demo Stories                                         │
│  Patching workflow · Windows mgmt · Network automation · ITSM   │
├─────────────────────────────────────────────────────────────────┤
│  Layer 3 — Workloads                                            │
│  RHEL nodes (Satellite-managed) · Windows nodes (AD-joined)     │
│  Applications (LAMP, websites)                                  │
├─────────────────────────────────────────────────────────────────┤
│  Layer 2 — Network & Security Appliances                        │
│  F5 BIG-IP · Palo Alto NGFW                                     │
├─────────────────────────────────────────────────────────────────┤
│  Layer 1 — Core Services                                        │
│  Red Hat Satellite · Active Directory · Infoblox (DNS/IPAM)     │
├─────────────────────────────────────────────────────────────────┤
│  Layer 0 — AWS Infrastructure                                   │
│  Terraform: VPC · Subnets · Security Groups · EC2 nodes         │
└─────────────────────────────────────────────────────────────────┘
```

---

## Migration Sequence

| Step | Component                      | Layer | Source Repo             | Status         |
|------|--------------------------------|-------|-------------------------|----------------|
| 1    | AWS Infrastructure (Terraform) | 0     | demo.datacenter         | ✅ Complete     |
| 2    | Red Hat Satellite              | 1     | aap.dailydemo.satellite | ✅ Migrated     |
| 3    | Active Directory               | 1     | aap.dailydemo.windows   | 🔄 In Progress |
| 4    | Infoblox (DNS/IPAM)            | 1     | new                     | ⬜ Not Started |
| 5    | F5 BIG-IP                      | 2     | aap.dailydemo.F5        | ⬜ Not Started |
| 6    | Palo Alto NGFW                 | 2     | aap.dailydemo.Panos     | ⬜ Not Started |
| 7    | RHEL workload nodes            | 3     | aap.dailydemo.linux     | ⬜ Not Started |
| 8    | Windows workload nodes         | 3     | aap.dailydemo.windows   | ⬜ Not Started |
| 9    | Linux patching demo story      | 4     | aap.dailydemo.F5 / linux| ⬜ Not Started |
| 10   | Windows management demo story  | 4     | aap.dailydemo.windows   | ⬜ Not Started |
| 11   | Network automation demo story  | 4     | aap.dailydemo.F5 / Panos| ⬜ Not Started |
| 12   | ITSM / EDA integration         | 4     | aap.dailydemo.linux     | ⬜ Not Started |

---

## Phase 1 — Fix the F5 Demo (Immediate)

These fixes are independent of the DC1 migration and should be completed immediately
so demos are not running with known broken components.

- [ ] **Fix credential bug** — Change `Network F5` to `Daily Demo F5 Network` on
  `Day 2 - lb pool check` and `Day 2 - Linux Patching` job templates in
  `playbooks/files/config_as_code/40_job_templates.yml`
- [ ] **Fix workflow creation bug** — Consolidate all `controller_workflows:` definitions
  into a single file (`60_workflows.yml`) to eliminate the `include_vars: dir:` key
  overwrite problem
- [ ] **Add post-setup validation** — Add a final play to `setup_demo.yml` that queries
  the AAP API and asserts all expected job templates exist with correct credentials and
  all expected workflows are present before exiting green
- [ ] **Pin collection version** — Pin a known-good version of `infra.aap_configuration`
  in `collections/requirements.yml` to prevent upstream async bugs from reintroducing

---

## Phase 2 — Mature Datacenter 1 Core (Weeks)

Complete Layer 1 so the datacenter has a stable foundation before adding appliances.

- [ ] Complete Active Directory provisioning and AAP integration
- [ ] Add Infoblox DNS/IPAM provisioning
- [ ] Validate Layer 1 end-to-end: Satellite + AD + Infoblox stable and interoperable
- [ ] Establish post-setup validation pattern for DC1 `setup_demo.yml`
- [ ] Document Layer 0 + Layer 1 as a standalone demo story ("Here is your datacenter
  foundation, built by code in under an hour")

---

## Phase 3 — Migrate Appliances and Workloads (Weeks to Months)

Migrate Layer 2 and Layer 3 from standalone repos into DC1.

- [ ] Migrate F5 BIG-IP provisioning into DC1 — re-target against DC1 inventory
- [ ] Migrate Palo Alto provisioning into DC1
- [ ] Deploy RHEL workload nodes managed by Satellite
- [ ] Deploy Windows workload nodes joined to Active Directory
- [ ] Retire `aap.dailydemo.F5` standalone infrastructure (keep Day 2 playbooks)
- [ ] Retire `aap.dailydemo.Panos` standalone infrastructure (keep Day 2 playbooks)

---

## Phase 4 — Next Gen Demo Capabilities (Months)

Add capabilities that address current customer conversations and AAP platform evolution.

- [ ] **EDA across all demos** — Convert patching workflow approval node to an
  EDA-triggered automation (CVE notification → automatic patching workflow launch)
- [ ] **Ansible Lightspeed / AI integration** — In-controller playbook generation demo
  targeting the sysadmin-to-automation journey
- [ ] **Terraform + Ansible narrative** — Build explicit demo story around how AAP
  and Terraform coexist (targets customers who already have Terraform investment)
- [ ] **Multi-demo scenario** — A single DC1 environment that can demonstrate F5,
  Satellite, AD, and ITSM integration in one connected story without switching demos

---

## Continuity Rules During Migration

1. `aap.as.code` continues to list and support all standalone demos until their
   DC1 equivalents are proven
1. No standalone demo repo is archived until its DC1 replacement passes a
   full smoke test including post-setup validation
1. Each DC1 layer is validated independently before the next layer begins
1. All credential names are standardized across DC1 CaC files —
   no more `Network F5` vs `Daily Demo F5 Network` class of bugs

---

## Known Issues Being Tracked

| Issue                                                        | Repo             | Priority | Status                |
|--------------------------------------------------------------|------------------|----------|-----------------------|
| `Network F5` credential on Day 2 patching templates         | aap.dailydemo.F5 | High     | Open                  |
| Only one workflow created (include_vars key overwrite)       | aap.dailydemo.F5 | High     | Open                  |
| Intermittent async behavior in infra.aap_configuration 4.6.6 | aap.dailydemo.F5 | Medium   | Upstream (issue #1029)|
| No post-setup validation on any demo                         | All demos        | Medium   | Open                  |

---

## Related Repositories

| Repo | Purpose | Status |
|------|---------|--------|
| [aap.as.code](https://github.com/ericcames/aap.as.code) | Master bootstrap / entry point | Active |
| [demo.datacenter](https://github.com/ericcames/demo.datacenter) | DC1 — next gen platform | In Development |
| [aap.dailydemo.linux](https://github.com/ericcames/aap.dailydemo.linux) | Linux daily demo | Active |
| [aap.dailydemo.F5](https://github.com/ericcames/aap.dailydemo.F5) | F5 daily demo | Active (bugs noted) |
| [aap.dailydemo.windows](https://github.com/ericcames/aap.dailydemo.windows) | Windows daily demo | Active |
| [aap.dailydemo.Panos](https://github.com/ericcames/aap.dailydemo.Panos) | Palo Alto daily demo | Active |
| [aap.dailydemo.hashicorp](https://github.com/ericcames/aap.dailydemo.hashicorp) | Hashicorp daily demo | Active |

---

*This roadmap is maintained alongside the codebase. Update migration table status and
known issues as work progresses. Open GitHub Issues in `demo.datacenter` for individual
work items, using this document as the strategic reference.*
