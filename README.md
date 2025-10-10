The Red Hat Demo Platform
=========

This repo is designed to work with the Ansible Product Demos catalog item available in the Red Hat Demo Platform.  This repo will be used to load up setup templates to pull in other demos.

Looking for other Daily Demos?
=========

- [AAP Daily Demo Windows](https://github.com/ericcames/aap.dailydemo.windows "AAP Daily Demo Windows") - Ready
- [AAP Daily Demo Linux](https://github.com/ericcames/aap.dailydemo.linux "AAP Daily Demo Linux")
- [AAP Daily Demo F5](https://github.com/ericcames/aap.dailydemo.F5 "AAP Daily Demo F5")
- [AAP Daily Demo Panos](https://github.com/ericcames/aap.dailydemo.Panos "AAP Daily Demo Panos")
- [AAP Daily Demo Satellite](https://github.com/ericcames/aap.dailydemo.satellite "AAP Daily Demo Satellite")

# Ansible Product Demos

![alt text](https://github.com/ericcames/aap.as.code/blob/main/images/redhatdemo.png "Catalog Item")

**Moving in**

1. Ensure that we have the following credentials
    - Automation Hub - certified
    - Galaxy Server URL - https://console.redhat.com/api/automation-hub/content/published/
    - Auth Server URL - https://sso.redhat.com/auth/realms/redhat-external/protocol/openid-connect/token
    - Credential type - Ansible Galaxy/Automation Hub API Token
![alt text](https://github.com/ericcames/aap.as.code/blob/main/images/AHcertified.png "certified")

    - Automation Hub - validated
    - Galaxy Server URL - https://console.redhat.com/api/automation-hub/content/validated/
    - Auth Server URL - https://sso.redhat.com/auth/realms/redhat-external/protocol/openid-connect/token
    - Credential type - Ansible Galaxy/Automation Hub API Token

2. Ensure the Galaxy credentials are related to the Default Organization
![alt text](https://github.com/ericcames/aap.as.code/blob/main/images/orgswithcreds.png "Default Organization")

3. Create your vault credential
![alt text](https://github.com/ericcames/aap.as.code/blob/main/images/myvault.png "Vault")

4. Create your project
![alt text](https://github.com/ericcames/aap.as.code/blob/main/images/project.png "aap.as.code")

5. Create a remote vault with your secrets

[Remote Vault](https://raw.githubusercontent.com/ericcames/sourcefiles/refs/heads/main/vault_ames.yml "vault_ames.yml")<br>
[Example Vault](https://github.com/ericcames/sourcefiles/blob/main/vault_example.yml "vault_example.yml")<br>

6. Create your job template
![alt text](https://github.com/ericcames/aap.as.code/blob/main/images/template.png "Setup - AAP - CAC")

Extra variables
```
aap_configuration_async_retries: 60
controller_configuration_settings_secure_logging: true
my_aap_url: https://aap-aap.apps.cluster-wzl4x-1.dynamic.redhatworkshops.io
my_organization: AmesCO
my_aap_credential: Controller Credential
my_vault: Eric Ames
my_remote_vault: >-
  https://raw.githubusercontent.com/ericcames/sourcefiles/refs/heads/main/vault_ames.yml
```
**ServiceNow EventDriven Ansible**

Check out the playbooks/files/config_as_code/eda* files to see how much of the SNOW EDA integration is done.  To finish off the integration check out the doc directory for the AAP_Servicenow.pdf.

[AAP ServiceNow Setup](https://github.com/ericcames/aap.as.code/blob/main/doc/AAP_Servicenow.pdf "AAP ServiceNow setup")