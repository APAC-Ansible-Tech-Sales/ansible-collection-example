<!-- DOCSIBLE START -->

# 📃 Role overview

## auth_get_access_token

```
Role belongs to adoption_architects/microsoft_graph
Namespace - adoption_architects
Collection - microsoft_graph
Version - 1.0.11
Repository - https://github.com/APAC-Ansible-Tech-Sales/ansible-collection-sdlc
```

Description: Get an access token for the EntraID tenant.






<details>
<summary><b>🧩 Argument Specifications in meta/argument_specs</b></summary>

#### Key: main

**Description**: Get an access token for the Azure AD tenant.

**Options**:


  - **tenant_id**
    - **Required**: True
    - **Type**: str
    - **Default**: none
  
    - **Description**: The tenant ID of the Azure AD tenant.
  
  
  

  - **client_id**
    - **Required**: True
    - **Type**: str
    - **Default**: none
  
    - **Description**: The client ID of the Azure AD application.
  
  
  

  - **client_secret**
    - **Required**: True
    - **Type**: str
    - **Default**: none
  
    - **Description**: The client secret of the Azure AD application.
  
  
  



</details>










### Tasks


#### File: tasks/main.yml

| Name | Module | Has Conditions |
| ---- | ------ | -------------- |
| Assert required variables are set | ansible.builtin.assert | False |
| Acquire Microsoft Graph API access token | ansible.builtin.uri | False |







## Author Information
Phil White

#### License

GPL-3.0-or-later

#### Minimum Ansible Version

2.19

#### Platforms

No platforms specified.

#### Dependencies

No dependencies specified.
<!-- DOCSIBLE END -->
