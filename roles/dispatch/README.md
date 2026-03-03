# truist.gitlab_configuration.dispatch

## Description

An Ansible Role to run all roles in the truist.gitlab_configuration collection.

## Variables

This is a meta role, its purpose is to run the other roles in the collection, it does not run all of them, and can be used to call roles in a custom order.

The control variable is `gitlab_configuration_dispatcher_roles` which will set the roles to run for different components.

If you wish to run just a subset of roles either use `gitlab_configuration_dispatcher_exclude_roles` to exclude entries from the default list or define `gitlab_configuration_dispatcher_roles` with wanted roles locally.


```yaml
gitlab_configuration_dispatcher_roles: "{{ gitlab_configuration_roles | rejectattr('role', 'in', gitlab_configuration_dispatcher_exclude_roles) }}"
```

In addition each service has its own subset of roles, and each role has its own tag that can be used as well.

To exclude roles to avoid unwanted updates, use `gitlab_configuration_dispatcher_exclude_roles`:

```yaml
gitlab_configuration_dispatcher_exclude_roles:
  - gitlab_groups
  - gitlab_ldap_group_links
```

### GitLab Configuration Roles

```yaml
gitlab_configuration_roles:
  - role: gitlab_groups
    var: gitlab_groups
    tags: gitlab_groups
  - role: gitlab_projects
    var: gitlab_projects
    tags: gitlab_projects
  - role: gitlab_group_members
    var: gitlab_group_members
    tags: gitlab_group_members
  - role: gitlab_project_members
    var: gitlab_project_members
    tags: gitlab_project_members
  - role: gitlab_ldap_group_links
    var: gitlab_ldap_group_links
    tags: gitlab_ldap_group_link
```

#### Dispatch role var list keys

Each role in each service has its own variables, for information on those please see each role which this role will call. Each role is called and each item has three elements:

- `role` which is the name of the role within truist.gitlab_configuration
- `var` which is the variable which is used in that role. We use this to prevent the role being called if the variable is not set
- `tags` the tags which are applied to the role so it is possible to apply tags to a playbook using the dispatcher with these tags.

For more information about variables, see [top-level README](../../README.md).
For more information about roles, see each roles' README (also linked in the top-level README)

## License

GNU General Public License v3.0 or later.
