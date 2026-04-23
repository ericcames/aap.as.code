The Red Hat Demo Platform
=========

This repo is designed to work with the Ansible Product Demos catalog item available in the Red Hat Demo Platform.  This repo will be used to load up setup templates to pull in other demos.

Looking for other Daily Demos?
=========

- [AAP Daily Demo Windows](https://github.com/ericcames/aap.dailydemo.windows "AAP Daily Demo Windows") - ready
- [AAP Daily Demo Linux](https://github.com/ericcames/aap.dailydemo.linux "AAP Daily Demo Linux")
- [AAP Daily Demo F5](https://github.com/ericcames/aap.dailydemo.F5 "AAP Daily Demo F5") - ready
- [AAP Daily Demo Panos](https://github.com/ericcames/aap.dailydemo.Panos "AAP Daily Demo Panos") - ready
- [AAP Daily Demo Satellite](https://github.com/ericcames/aap.dailydemo.satellite "AAP Daily Demo Satellite") - moved to Datacenter 1
- [AAP Daily Demo hashicorp](https://github.com/ericcames/aap.dailydemo.hashicorp "AAP Daily Demo hashicorp") - ready
- [Datacenter 1](https://github.com/ericcames/demo.datacenter "Datacenter 1") - ready<br>

# Automated Bootstrap (Recommended)

Use the Claude Code skills to bootstrap a fresh RHDP AAP instance automatically.

**First time on a new machine? Run this first:**
```
/aap-first-time
```
It walks you through every prerequisite interactively. Already set up? Skip straight to `/aap-bootstrap`.

**Install the skills once:**
```bash
claude plugins marketplace add ericcames/aap-skills
claude plugins install aap-skills
```

| Skill | When to use |
|-------|-------------|
| `/aap-first-time` | First time on a new machine — sets up ansible.cfg, secrets2, collections, SSH key, and vault file |
| `/aap-bootstrap` | Quietly bootstrap AAP before running a separate demo |
| `/aap-setup-demo` | Bootstrap AAP and run the setup as a live demo story |

Skills source: [github.com/ericcames/aap-skills](https://github.com/ericcames/aap-skills)

---

# Ansible Product Demos

![alt text](https://github.com/ericcames/aap.as.code/blob/main/images/redhatdemo.png "Catalog Item")

**Getting started**
1. [Get Automation Hub Token](https://console.redhat.com/ansible/automation-hub/token/ "Load Token")<br>
2. [Create Service Account](https://github.com/ericcames/aap.as.code.starter.kit/blob/main/docs/Ansible%20Automation%20Platform%20Service%20Account.pdf "Service Account")<br>
3. Ensure that we have a certified credential
    - Name
    ```
    Automation Hub - certified
    ```
    - Credential type
    ```
    Ansible Galaxy/Automation Hub API Token
    ```
    - Galaxy Server URL
    ```
    https://console.redhat.com/api/automation-hub/content/published/
    ```
    - Auth Server URL
    ```
    https://sso.redhat.com/auth/realms/redhat-external/protocol/openid-connect/token
    ```
    - API Token

![alt text](https://github.com/ericcames/aap.as.code/blob/main/images/AHcertified.png "certified")

4. Ensure that we have a validated credential
    - Name
    ```
    Automation Hub - validated
    ```
    - Credential type
    ```
    Ansible Galaxy/Automation Hub API Token
    ```
    - Galaxy Server URL
    ```
    https://console.redhat.com/api/automation-hub/content/validated/
    ```
    - Auth Server URL
    ```
    https://sso.redhat.com/auth/realms/redhat-external/protocol/openid-connect/token
    ```
    - API Token

5. Ensure the Galaxy credentials are related to the Default Organization
![alt text](https://github.com/ericcames/aap.as.code/blob/main/images/orgswithcreds.png "Default Organization")

6. Create your vault credential

**There is a dependency to use the team default_password that allows the F5 demo to work**

![alt text](https://github.com/ericcames/aap.as.code/blob/main/images/myvault.png "Vault")

7. Create your project
![alt text](https://github.com/ericcames/aap.as.code/blob/main/images/project.png "aap.as.code")

    - Name
    ```
    aap.as.code
    ```
    - Source control type
    ```
    Git
    ```
    - Source control URL
    ```
    https://github.com/ericcames/aap.as.code.git
    ```

8. Create a remote vault with your secrets

[Remote Vault](https://raw.githubusercontent.com/ericcames/sourcefiles/refs/heads/main/vault_ames.yml "vault_ames.yml")<br>
[Example Vault](https://github.com/ericcames/sourcefiles/blob/main/vault_example.yml "vault_example.yml")<br>

Variables for your online vault

| Key | Description | Purpose |
|---|---|---|
| default_passwd | Our team password; Also used in the F5 | Default Password |
| redhat_username | RH Service Account Username | Used to register the system |
| redhat_passwd | RH Service Account Password | Used to register the system |
| ddw_password | Windows server password | Used to grant access to windows servers |
| ddw_username | Windows server username | Used to grant access to windows servers |
| snow_url | The URL for your ServiceNow instance | Used for SNOW automation |
| snow_username | The username for your ServiceNow instance | Used for SNOW automation |
| servicenow_passwd | The password for your ServiceNow instance | Used for SNOW automation |
| ssh_priv_key | Your laptop private key | Used to give you access to what you build. |
| dyna_key | Not sure if this is used or not | Need to determine if this is used |
| vault_passwd | Not sure if this is used or not | Need to determin if this is used |
| red_hat_auto_hub_token | API Token for Red Hat Automation Hub | Used to pull collections |
| my_ctrl_username | Not sure if this is used | Need to determine if this is used |
| my_ctrl_admin_password | Not sure if this is used | Need to determine if this is used |
| admin_password | Admin password for the network gear | Used for the admin password F5, Palo, Infoblox |
| reg_rh_io_passwd | registry.redhat.io | Used to get access to images |
| reg_rh_io_username | registry.redhat.io | Used to get access to images |
| quay_passwd | quay.io | Used to get access to images |
| quay_username | quay.io | Used to get access to images |
| customer_portal_username | Red Hat Customer Portal | Get access to software |
| customer_portal_password | Red Hat Customer Portal | Get access to software |
| rh_activation_key | Red Hat activation key | Used by the rhsm role |
| rh_org_id | Red Hat Org ID | Used by the rhsm role |

9. Add your public ssh key to a public repo.  Amazon works with RSA keys.

[Public SSH Key](https://raw.githubusercontent.com/ericcames/sourcefiles/refs/heads/main/id_rsa.pub "Public SSH Key")

10. Update my_windows_catalog_short_description: Ames AAP Windows AWS Daily Demo with the short description of your catalog item.<br>
![alt text](https://github.com/ericcames/aap.as.code/blob/main/images/snowshort.png "ServiceNow Catalog Item")

11. Create your job template

Template name
```
Setup - AAP - CAC
```

![alt text](https://github.com/ericcames/aap.as.code/blob/main/images/setup_template.png "Setup - AAP - CAC")

Extra variables
```
my_windows_catalog_short_description: Ames AAP Windows AWS Daily Demo
my_vault: Eric Ames
my_remote_vault: >-
  https://raw.githubusercontent.com/ericcames/sourcefiles/refs/heads/main/vault_ames.yml
my_remote_ssh_pub_key: >-
  https://raw.githubusercontent.com/ericcames/sourcefiles/refs/heads/main/id_rsa.pub
```
**ServiceNow EventDriven Ansible**

Check out the playbooks/files/config_as_code/eda* files to see how much of the SNOW EDA integration is done.  To finish off the integration check out the docs directory for the AAP_Servicenow.pdf.

[AAP ServiceNow Setup](https://github.com/ericcames/aap.as.code/blob/main/docs/AAP_Servicenow.pdf "AAP ServiceNow setup")