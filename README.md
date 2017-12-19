# Mattermost

This role sets up and configures a mattermost instance.


## Requirements

It needs an apt based system like ubuntu. Also the stuvusIT.nginx and stuvusIT.postgresql roles are required.

## Role Variables

| Name                              |         Required         | Default      | Description                                                                                                                                                                                            |    |    |
|:----------------------------------|:------------------------:|:-------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---|:---|
| `mattermost_version`              | :heavy_multiplication_x: | `4.5.0`      | Install version of mattermost                                                                                                                                                                          |    |    |
| `mattermost_db_user`              | :heavy_multiplication_x: | `mattermost` | Name of database user                                                                                                                                                                                  |    |    |
| `mattermost_db_password`          |    :heavy_check_mark:    |              | Password for database user                                                                                                                                                                             |    |    |
| `mattermost_db_name`              | :heavy_multiplication_x: | `mattermost` | Name of database mattermost should use                                                                                                                                                                 |    |    |
| `mattermost_user`                 | :heavy_multiplication_x: | `mattermost` | User under which the mattermost process runs                                                                                                                                                           |    |    |
| `mattermost_enterprise`           | :heavy_multiplication_x: | `false`      | Select if the role should setup a enterprise version or the team version. If you set this variable to `true` you have to place a file named mattermost_licence under `files/{{ inventory_hostname }}/` |    |    |
| `mattermost_teams`                | :heavy_multiplication_x: |              | Mattermost teams that should be created                                                                                                                                                                |    |    |
| `mattermost_channels`             | :heavy_multiplication_x: |              | Mattermost Channels to be created                                                                                                                                                                      |    |    |
| `mattermost_admins`               | :heavy_multiplication_x: |              | Mattermost admin accounts to be created                                                                                                                                                                |    |    |
| `mattermost_Sql_AtRestEncryptKey` |    :heavy_check_mark:    |              | Encrypt key used for sql. Please generate.                                                                                                                                                             |    |    |
| `mattermost_File_PublicLinkSalt`  |    :heavy_check_mark:    |              | Public Link Salt, generate and keep secret                                                                                                                                                             |    |    |
| `mattermost_Email_InviteSalt`     |    :heavy_check_mark:    |              | Email Invite Salt, generate and keep secret                                                                                                                                                            |    |    |


### Mattermost Teams
| Name           |         Required         | Default | Description                  |    |    |
|:---------------|:------------------------:|:--------|:-----------------------------|:---|:---|
| `name`         |    :heavy_check_mark:    |         | Mattermost team name         |    |    |
| `display_name` |    :heavy_check_mark:    |         | Mattermost display team name |    |    |
| `email`        |    :heavy_check_mark:    |         | Mattermost team email        |    |    |
| `private`      | :heavy_multiplication_x: | `false` | Is the team private          |    |    |


### Mattermost Channels
| Name           |         Required         | Default | Description                              |    |    |
|:---------------|:------------------------:|:--------|:-----------------------------------------|:---|:---|
| `name`         |    :heavy_check_mark:    |         | Mattermost channel name                  |    |    |
| `display_name` |    :heavy_check_mark:    |         | Mattermost display name                  |    |    |
| `team`         |    :heavy_check_mark:    |         | Mattermost team where to add the channel |    |    |
| `private`      | :heavy_multiplication_x: | `false` | Is the channel private                   |    |    |

### Mattermost Admins
| Name        |         Required         | Default | Description           |    |    |
|:------------|:------------------------:|:--------|:----------------------|:---|:---|
| `username`  |    :heavy_check_mark:    |         | Username to be used   |    |    |
| `email`     |    :heavy_check_mark:    |         | user account mail     |    |    |
| `password`  |    :heavy_check_mark:    |         | user account password |    |    |
| `firstname` | :heavy_multiplication_x: |         | First name            |    |    |
| `lastname`  | :heavy_multiplication_x: |         | First name            |    |    |
| `locale`    | :heavy_multiplication_x: |         | Language to use       |    |    |

[For all config variables see the mattermost config](https://docs.mattermost.com/administration/config-settings.html#)

## Example Playbook
This will follow after testing

```yml
served_domains:
      - domains: 
          - mattermost
        default_server: true
        https: false
        enable_http2: true
        locations:
          - condition: /
            content:
            |
              client_max_body_size 50M;
              proxy_set_header Connection "";
              proxy_set_header Host $http_host;
              proxy_set_header X-Real-IP $remote_addr;
              proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
              proxy_set_header X-Forwarded-Proto $scheme;
              proxy_set_header X-Frame-Options SAMEORIGIN;
              proxy_buffers 256 16k;
              proxy_buffer_size 16k;
              proxy_read_timeout 600s;
              proxy_cache mattermost_cache;
              proxy_cache_revalidate on;
              proxy_cache_min_uses 2;
              proxy_cache_use_stale timeout;
              proxy_cache_lock on;
              proxy_pass http://backend;
          - condition: ~ /api/v[0-9]+/(users/)?websocket$
            content:
            |
              proxy_set_header Upgrade $http_upgrade;
              proxy_set_header Connection "upgrade";
              client_max_body_size 50M;
              proxy_set_header Host $http_host;
              proxy_set_header X-Real-IP $remote_addr;
              proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
              proxy_set_header X-Forwarded-Proto $scheme;
              proxy_set_header X-Frame-Options SAMEORIGIN;
              proxy_buffers 256 16k;
              proxy_buffer_size 16k;
              proxy_read_timeout 600s;
              proxy_pass http://backend;
    mattermost_Service_LicenseFileLocation: "/opt/mattermost/config/licence_file"
    mattermost_enterprise: true
    mattermost_db_password: dbpassword
    mattermost_cert_email_address: referent-it@stuvus.uni-stuttgart.de
    mattermost_user: mattermost
    mattermost_Service_URL: "mattermost.stuvus.uni-stuttgart.de"
    mattermost_Team_SiteName: "stuvus"
    mattermost_Service_TLSCertFile: ""
    mattermost_Service_TLSKeyFile: ""
    mattermost_Service_UseLetsEncrypt: "false"
    mattermost_Team_EnableTeamCreation: "true"
    mattermost_Team_EnableUserCreation: "true"
    mattermost_Team_EnableOpenServer: "false"
    mattermost_Team_RestrictCreationToDomains: "stuvus.uni-stuttgart.de"
    mattermost_Team_EnableCustomBrand: "true"
    mattermost_Team_CustomBrandText: "stuvus"
    mattermost_Team_CustomDescriptionText: "Studierendenvertretung Universität Stuttgart"
    mattermost_Sql_AtRestEncryptKey: "put something here"
    mattermost_Ldap_Enable: "false"
    mattermost_Ldap_LdapServer: "ldap01.faveve.uni-stuttgart.de"
    mattermost_Ldap_LdapPort: 636
    mattermost_Ldap_ConnectionSecurity: "TLS"
    mattermost_Ldap_BaseDN: "dc=faveve,dc=uni-stuttgart,dc=de"
    mattermost_Ldap_BindUsername: "binduser"
    mattermost_Ldap_BindPassword: "bindpw"
    mattermost_Ldap_UserFilter: "(|(objectclass=gosaAccount)(objectclass=inetOrgPerson)(objectclass=posixAccount))"
    mattermost_Ldap_FirstNameAttribute: ""
    mattermost_Ldap_LastNameAttribute: ""
    mattermost_Ldap_EmailAttribute: "mail"
    mattermost_Ldap_UsernameAttribute: "cn"
    mattermost_Ldap_NicknameAttribute: ""
    mattermost_Ldap_IdAttribute: "uid"
    mattermost_Ldap_PositionAttribute: ""
    mattermost_Ldap_SyncIntervalMinutes: 60
    mattermost_Ldap_SkipCertificateVerification: "false"
    mattermost_Ldap_QueryTimeout: 60
    mattermost_Ldap_MaxPageSize: 0
    mattermost_Ldap_LoginFieldName: ""
    mattermost_GitLab_Enable: "false"
    mattermost_GitLab_Secret: ""
    mattermost_GitLab_Id: ""
    mattermost_GitLab_Scope: ""
    mattermost_GitLab_AuthEndpoint: ""
    mattermost_GitLab_TokenEndpoint: ""
    mattermost_GitLab_UserApiEndpoint: ""
    mattermost_Email_EnableSignUpWithEmail: "false"
    mattermost_Email_EnableSignInWithEmail: "true"
    mattermost_Email_EnableSignInWithUsername: "true"
    mattermost_Email_SendEmailNotifications: "false"
    mattermost_Email_RequireEmailVerification: "false"
    mattermost_Email_FeedbackName: ""
    mattermost_Email_FeedbackEmail: ""
    mattermost_Email_FeedbackOrganization: ""
    mattermost_Email_EnableSMTPAuth: "false"
    mattermost_Email_SMTPUsername: ""
    mattermost_Email_SMTPPassword: ""
    mattermost_Email_SMTPServer: ""
    mattermost_Email_SMTPPort: ""
    mattermost_Email_ConnectionSecurity: ""
    mattermost_Email_InviteSalt: "du9bga69i1dknm8wnasdf134woe123"
    mattermost_Email_SendPushNotifications: "false"
    mattermost_Email_PushNotificationServer: ""
    mattermost_Email_PushNotificationContents: "generic"
    mattermost_Email_EnableEmailBatching: "false"
    mattermost_Email_EmailBatchingBufferSize: 256
    mattermost_Email_EmailBatchingInterval: 30
    mattermost_Email_SkipServerCertificateVerification: "false"
    mattermost_Email_EmailNotificationContentsType: "full"
    mattermost_teams:
      - name: "vorstand"
        display_name: "Vorstand"
        private: true
        email: "vorstand@stuvus.uni-stuttgart.de"
      - name: "referat-o"
        display_name: "Referat Oeffentlichkeitsarbeit"
        email: "referent-oeffentlichkeit@stuvus.uni-stuttgart.de"
    mattermost_admins:
      - firstname: Fritz
        lastname: Otlinghaus
        email: fritz.otlinghaus@stuvus.uni-stuttgart.de
        locale: de
        username: Scriptkiddi
        password: test1234
```

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-sa/4.0/).


## Author Information

- [Fritz Otlinghaus (Scriptkiddi)](https://github.com/scriptkiddi) _fritz.otlinghaus@stuvus.uni-stuttgart.de_
