# bmcintyr.gitlab_configuration.gitlab_groups

## Description

An Ansible Role to create/update/remove GitLab groups

## Requirements

ansible-galaxy collection install -r tests/collections/requirements.yml to be installed

## Variables

|Variable Name|Default Value|Required|Type|Description|Example|
|:---|:---:|:---:|:---|:---|:---|
|`gitlab_configuration_state`|"present"|no|str|The state all objects will take unless overridden by object default|'present'|
|`gitlab_api_url`|""|yes|str|URL to the GitLab Server.|127.0.0.1|
|`gitlab_validate_certs`|`true`|no|str|Whether or not to validate the GitLab Server's SSL certificate.||
|`gitlab_api_token`|""|no|str|Admin User's token on the GitLab Server. This should be stored in an AAP Credential or elsewhere.||
|`gitlab_groups`|`see below`|yes|str|Data structure describing your groups. Described below.||


### Secure Logging Variables

The following Variables complement each other.
If both variables are not set, secure logging defaults to false.
The role defaults to false as normally the add groups task does not include sensitive information.
gitlab_groups_secure_logging defaults to the value of gitlab_configuration_secure_logging if it is not explicitly called. This allows for secure logging to be toggled for the entire suite of configuration roles with a single variable, or for the user to selectively use it.

|Variable Name|Default Value|Required|Type|Description|
|:---:|:---:|:---:|:---:|:---|
|`gitlab_groups_secure_logging`|`false`|no|str|Whether or not to include the sensitive group_members role tasks in the log.|
|`gitlab_configuration_secure_logging`|`false`|no|str|This variable enables secure logging as well, but is shared across multiple roles, see above.|

### Asynchronous Retry Variables

The following Variables set asynchronous retries for the role.
If neither of the retries or delay or retries are set, they will default to their respective defaults.
This allows for all items to be created, then checked that the task finishes successfully.
This also speeds up the overall role.

|Variable Name|Default Value|Required|Type|Description|
|:---:|:---:|:---:|:---:|:---|
|`gitlab_configuration_async_retries`|50|no|str|This variable sets the number of retries to attempt for the role globally.|
|`gitlab_groups_async_retries`|`{{ aap_configuration_async_retries }}`|no|str|This variable sets the number of retries to attempt for the role.|
|`gitlab_groups_async_delay`|1|no|str|This sets the delay between retries for the role globally.|
|`gitlab_groups_loop_delay`|0|no|int|This sets the pause between each item in the loop for the roles globally. To help when API is getting overloaded.|
|`gitlab_configuration_async_dir`|`null`|no|bool|Sets the directory to write the results file for async tasks. The default value is set to `null` which uses the Ansible Default of `/root/.ansible_async/`.|

## Data Structure

### Group Member Variables

|Variable Name|Default Value|Required|Type|Description|
|:---:|:---:|:---:|:---:|:---:|
|`name`|""|yes|str|Name of the group you want to create..|
|`description`|""|no|str|A description for the group.|
|`parent`|""|no|str|Allow to create subgroups. ID or Full path of parent group in the form of group/name.|
|`state`|`present`|no|str|Create or delete group. Possible values are present and absent.|

#### Yaml Example

```yaml
---
gitlab_groups:
  - name: GroupName
    description: New gitlab group
    parent: parentgroup
    state: present

```

## Playbook Examples

### Standard Role Usage

```yaml
---
- name: Playbook to configure GitLab post installation
  hosts: localhost
  connection: local
  roles:
    - {role: bmcintyr.gitlab_configuration.gitlab_groups, when: gitlab_groups is defined}
```

## License

GNU General Public License v3.0 or later.