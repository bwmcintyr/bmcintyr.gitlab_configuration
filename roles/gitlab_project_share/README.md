# bmcintyr.gitlab_configuration.gitlab_project_share

## Description

An Ansible Role to share a GitLab project with another group and its members

## Requirements

ansible-galaxy collection install -r tests/collections/requirements.yml to be installed

## Variables

|Variable Name|Default Value|Required|Type|Description|Example|
|:---|:---:|:---:|:---|:---|:---|
|`gitlab_configuration_state`|"present"|no|str|The state all objects will take unless overridden by object default|'present'|
|`gitlab_api_url`|""|yes|str|URL to the GitLab Server.|127.0.0.1|
|`gitlab_validate_certs`|`true`|no|str|Whether or not to validate the GitLab Server's SSL certificate.||
|`gitlab_api_token`|""|no|str|Admin User's token on the GitLab Server. This should be stored in an AAP Credential or elsewhere.||
|`gitlab_project_share_loop_delay`|0|no|int|This sets the pause between each item in the loop for the roles globally. To help when API is getting overloaded.|
|`gitlab_project_shares`|`see below`|yes|str|Data structure describing your project to share. Described below.||


### Secure Logging Variables

The role defaults to `true` as the tasks make use of of the ansible.builtin.uri module which can include sensitive information.

|Variable Name|Default Value|Required|Type|Description|
|:---:|:---:|:---:|:---:|:---|
|`gitlab_project_share_secure_logging`|`false`|no|str|Whether or not to include the sensitive group_members role tasks in the log.|


## Data Structure

### Project Share Variables

|Variable Name|Default Value|Required|Type|Description|
|:---:|:---:|:---:|:---:|:---:|
|`name`|""|yes|str|Name of the project you want to share.  Must include the full path, i.e. 'parent_group/project_name'.|
|`group_full_path`|""|yes|str|Full path to the group to be invited|
|`group_access`|guest|yes|str|Access level for the group (guest, reporter, developer, maintainer, owner)|
|`expires_at`|""|no|str|Share expiration date in format YYYY-MM-DD|
|`state`|`present`|no|str|Create or delete group. Possible values are present and absent.|

#### Yaml Example

```yaml
---
gitlab_project_shares:
  - name: path_to_project_to_share
    group_full_path: parent/group_to_share_with
    group_access: developer
    expires_at: YYYY-MM-DD
    state: present
```

## Playbook Examples

### Standard Role Usage

```yaml
---
- name: Playbook to mange GitLab resources
  hosts: localhost
  connection: local
  roles:
    - {role: bmcintyr.gitlab_configuration.gitlab_project_share, when: gitlab_project_shares is defined}
```

## License

GNU General Public License v3.0 or later.