# truist.gitlab_configuration.gitlab_projects

## Description

An Ansible Role to create/update/remove GitLab projects

## Requirements

ansible-galaxy collection install -r tests/collections/requirements.yml to be installed

## Variables

|Variable Name|Default Value|Required|Type|Description|Example|
|:---|:---:|:---:|:---|:---|:---|
|`gitlab_configuration_state`|"present"|no|str|The state all objects will take unless overridden by object default|'present'|
|`gitlab_api_url`|""|yes|str|URL to the GitLab Server.|127.0.0.1|
|`gitlab_validate_certs`|`true`|no|str|Whether or not to validate the GitLab Server's SSL certificate.||
|`gitlab_api_token`|""|no|str|Admin User's token on the GitLab Server. This should be stored in an AAP Credential or elsewhere.||
|`gitlab_projects`|`see below`|yes|str|Data structure describing your project members. Described below.||


### Secure Logging Variables

The following Variables complement each other.
If Both variables are not set, secure logging defaults to false.
The role defaults to false as normally the add project members task does not include sensitive information.
gitlab_projects_secure_logging defaults to the value of gitlab_configuration_secure_logging if it is not explicitly called. This allows for secure logging to be toggled for the entire suite of configuration roles with a single variable, or for the user to selectively use it.

|Variable Name|Default Value|Required|Type|Description|
|:---:|:---:|:---:|:---:|:---|
|`gitlab_projects_secure_logging`|`false`|no|str|Whether or not to include the sensitive group_members role tasks in the log.|
|`gitlab_configuration_secure_logging`|`false`|no|str|This variable enables secure logging as well, but is shared across multiple roles, see above.|

### Asynchronous Retry Variables

The following Variables set asynchronous retries for the role.
If neither of the retries or delay or retries are set, they will default to their respective defaults.
This allows for all items to be created, then checked that the task finishes successfully.
This also speeds up the overall role.

|Variable Name|Default Value|Required|Type|Description|
|:---:|:---:|:---:|:---:|:---|
|`gitlab_configuration_async_retries`|50|no|str|This variable sets the number of retries to attempt for the role globally.|
|`gitlab_projects_async_retries`|`{{ aap_configuration_async_retries }}`|no|str|This variable sets the number of retries to attempt for the role.|
|`gitlab_projects_async_delay`|1|no|str|This sets the delay between retries for the role globally.|
|`gitlab_projects_loop_delay`|0|no|int|This sets the pause between each item in the loop for the roles globally. To help when API is getting overloaded.|
|`gitlab_configuration_async_dir`|`null`|no|bool|Sets the directory to write the results file for async tasks. The default value is set to `null` which uses the Ansible Default of `/root/.ansible_async/`.|

## Data Structure

### Project Variables

|Variable Name|Default Value|Required|Type|Description|
|:---:|:---:|:---:|:---:|:---:|
|`name`|""|yes|str|The name of the project.|
|`description`|""|no|str|A description for the project.|
|`group`|""|no|str|ID or the full path of the group of which this projects belongs to.|
|`state`|`present`|no|str|State of the project.|

#### Yaml Example

```yaml
---
gitlab_projects:
  - name: my_project
    description: A GitLab Project
    group: parentgroup/subgroup
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
    - {role: truist.gitlab_configuration.gitlab_projects, when: gitlab_projects is defined}
```

## License

GNU General Public License v3.0 or later.