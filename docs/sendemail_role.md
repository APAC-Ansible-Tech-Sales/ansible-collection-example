# Overview

This role uses the `auth_get_access_token` role to generate a token for the Microsoft Graph API, then uses that token to send an email. This role supports CC’ing other email addresses using the optional cc_recipients variable.

# Requirements

 - You must have a EntraID service principal with the `Mail.Send` application permission on GraphAPI. A user with elevated permissions must grant the access. To restrict what mailbox can be used as the sender, you can configure the service principal in Exchange Online with the `Send As` permission for the relevant mailbox.

# Role Variables

    | Variable         | Required  | Description                                      |
    |------------------|-----------|--------------------------------------------------|
    | `client_id`      | Yes       | The client ID of the service principal.          |
    | `client_secret`  | Yes       | The client secret of the service principal.      |
    | `recipient_email`| Yes       | The email address of the recipient.              |
    | `email_subject`  | Yes       | The subject of the email.                        |
    | `email_body`     | Yes       | The body content of the email.                   |
    | `sender_email`   | No        | The email address of the sender. If not provided, the default is example@email.com. |
    | `email_content_type` | No        | The content type of the email body. Defaults to "Text". Set to "HTML" if the body contains HTML content. |
    | `cc_recipients`  | No        | A list of email addresses to CC.                 |
