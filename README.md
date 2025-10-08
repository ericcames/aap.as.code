The Red Hat Demo Platform
=========

This repo is designed to work with the Ansible Product Demos catalog item available in the Red Hat Demo Platform.  This repo will be used to load up setup templates to pull in other demos.

# Ansible Product Demos

![alt text](https://github.com/ericcames/aap.as.code/blob/main/images/redhatdemo.png "Catalog Item")

**Moving in**

1. Ensure that we have the following credentials
    - Automation Hub - certified
    - Galaxy Server URL - https://console.redhat.com/api/automation-hub/content/published/
    - Auth Server URL - https://sso.redhat.com/auth/realms/redhat-external/protocol/openid-connect/token
    - Credential type - Ansible Galaxy/Automation Hub API Token

    - Automation Hub - validated
    - Galaxy Server URL - https://console.redhat.com/api/automation-hub/content/validated/
    - Auth Server URL - https://sso.redhat.com/auth/realms/redhat-external/protocol/openid-connect/token
    - Credential type - Ansible Galaxy/Automation Hub API Token

2. Ensure the Galaxy credentials are related to the Default Organization

![alt text](https://github.com/ericcames/aap.as.code/blob/main/images/orgswithcreds.png "Default Organization")
