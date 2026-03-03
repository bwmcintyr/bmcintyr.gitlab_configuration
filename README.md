# bmcintyr GitLab Configuration Collection

This Ansible collection allows for easy interaction with GitLab via Ansible roles using the modules from the community collections.

## Requirements

The collection that contains the modules required for this collection to work. You can copy this requirements.yml file example.

```yaml
---
collections:
  - name: community.general
...
```

## Links to Ansible Automation Platform Collections

|                                      Collection Name                                |            Purpose            |
|:-----------------------------------------------------------------------------------:|:-----------------------------:|
| [community.general repo](https://github.com/ansible-collections/community.general)  | gitlab modules                |


## Installing this collection

You can install the bmcintyr.gitlab_configuration collection with the Ansible Galaxy CLI:

```console
ansible-galaxy collection install bmcintyr.gitlab_configuration
```

You can also include it in a `requirements.yml` file and install it with `ansible-galaxy collection install -r requirements.yml`, using the format:

```yaml
---
collections:
  - name: bmcintyr.gitlab_configuration
    # If you need a specific version of the collection, you can specify like this:
    # version: ...
```

## Using This Collection

**Install This Collection:**
Ensure this collection (`bmcintyr.gitlab_configuration`) is installed:

```bash
ansible-galaxy collection install bmcintyr.gitlab_configuration
```

**Run Playbooks from This Collection:**
To execute a playbook packaged within this collection (e.g., `configure_gitlab.yml`):

```console
ansible-playbook bmcintyr.gitlab_configuration.configure_gitlab.yml
```

**Troubleshooting "couldn't resolve module/action":**
This error usually means the required Ansible collection (e.g., `bmcintyr.gitlab_configuration` or a dependency like community.general) is:

* Not installed.
* Incorrectly named in the playbook.
* Not found in Ansible's configured collection paths.

Verify installation with `ansible-galaxy collection list` and that you have all the stated dependencies listed above in the requirements section.

Define following vars here, or in a credential in Ansible Automation Platform
`gitlab_api_url: gitlab.mydomain.com`

You can also specify authentication by a combination of either:

* `gitlab_api_url`, `gitlab_api_username`, `gitlab_api_password`
* `gitlab_api_url`, `gitlab_api_token`

The access token is the preferred method. You can obtain the token through the GitLab UI. https://docs.gitlab.com/user/profile/personal_access_tokens/

## Licensing

GNU General Public License v3.0 or later.

