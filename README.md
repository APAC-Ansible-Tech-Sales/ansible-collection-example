<!-- DOCSIBLE START -->

# 📃 Collection overview

**Namespace**: adoption_architects

**Name**: microsoft_graph

**Version**: 1.0.2

**Authors**:

- Phil White (github.com/phwhite-redhat)

## Description

Collection for Microsoft Graph



## Roles

### [auth_get_access_token](https://github.com/APAC-Ansible-Tech-Sales/ansible-collection-sdlc/tree/main/roles/auth_get_access_token)

### [send_email](https://github.com/APAC-Ansible-Tech-Sales/ansible-collection-sdlc/tree/main/roles/send_email)

## Roles vars

# [auth_get_access_token](https://github.com/APAC-Ansible-Tech-Sales/ansible-collection-sdlc/tree/main/roles/auth_get_access_token)
## auth_get_access_token Description:
Get an access token for the EntraID tenant.




<details>
<summary><b>🧩 auth_get_access_token Argument Specifications in meta/argument_specs</b></summary>

#### Key: main
**Description**: Get an access token for the Azure AD tenant.


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







# [send_email](https://github.com/APAC-Ansible-Tech-Sales/ansible-collection-sdlc/tree/main/roles/send_email)
## send_email Description:
Send an email using the Microsoft Graph API.




<details>
<summary><b>🧩 send_email Argument Specifications in meta/argument_specs</b></summary>

#### Key: main
**Description**: Send an email with the Microsoft Graph API.


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
  
  
  
  

  - **graph_access_token**
    - **Required**: False
    - **Type**: str
    - **Default**: none
  
    - **Description**: The access token for the Microsoft Graph API.
  
  
  
  

  - **email_subject**
    - **Required**: True
    - **Type**: str
    - **Default**: none
  
    - **Description**: The subject of the email.
  
  
  
  

  - **email_body**
    - **Required**: True
    - **Type**: str
    - **Default**: none
  
    - **Description**: The body of the email.
  
  
  
  

  - **email_content_type**
    - **Required**: False
    - **Type**: str
    - **Default**: Text
  
    - **Description**: The content type of the email.
  
  
      - **Choices**:
    
          - Text
    
          - HTML
    
          - RichText
    
          - PlainText
    
          - RichText
    
  
  
  

  - **sender_email**
    - **Required**: True
    - **Type**: str
    - **Default**: none
  
    - **Description**: The email address of the sender.
  
  
  
  

  - **recipient_email**
    - **Required**: True
    - **Type**: str
    - **Default**: none
  
    - **Description**: The email address of the recipient.
  
  
  
  



</details>







## Metadata

- **Repository**: [Repository](https://github.com/APAC-Ansible-Tech-Sales/ansible-collection-sdlc)

- **Documentation**: [Documentation](https://github.com/APAC-Ansible-Tech-Sales/ansible-collection-sdlc)

- **Homepage**: [Homepage](https://github.com/APAC-Ansible-Tech-Sales/ansible-collection-sdlc)


<!-- DOCSIBLE END -->
