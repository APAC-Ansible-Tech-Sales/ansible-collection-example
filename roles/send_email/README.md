<!-- DOCSIBLE START -->

# 📃 Role overview

## send_email

```
Role belongs to adoption_architects/microsoft_graph
Namespace - adoption_architects
Collection - microsoft_graph
Version - 1.0.8
Repository - https://github.com/APAC-Ansible-Tech-Sales/ansible-collection-sdlc
```

Description: Send an email using the Microsoft Graph API.






<details>
<summary><b>🧩 Argument Specifications in meta/argument_specs</b></summary>

#### Key: main

**Description**: Send an email with the Microsoft Graph API.

**Options**:


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










### Tasks


#### File: tasks/main.yml

| Name | Module | Has Conditions |
| ---- | ------ | -------------- |
| Assert Required Variables | block | False |
| Assert Required Facts | ansible.builtin.assert | True |
| Assert Required Facts | ansible.builtin.assert | True |
| Get Graph API token if not provided by variable | block | True |
| Get Graph API token | ansible.builtin.import_role | False |
| Set fact for graph access token | ansible.builtin.set_fact | False |
| Send Email with Graph API | ansible.builtin.uri | False |




## Playbook

```yml
---
# Make sure to export the roles location with the following command: export ANSIBLE_ROLES_PATH=../../../roles

- name: Send Email with Graph API
  hosts: localhost

  vars:
    ansible_connection: local
    client_id: "<CLIENT_ID>"
    sender_email: "sender@example.com"
    recipient_email: "recipient@example.com"
    email_subject: "Test"
    email_body: "Test Email"

  tasks:
    - import_role:
        name: send_email

```


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
