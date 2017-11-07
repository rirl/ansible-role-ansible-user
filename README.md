# Ansible user role

An ansible role that creates a user for running playbooks and allows connections via SSH for users from `ansible_users` list (using public keys).

The role includes the following tasks:

1. Create a group `ansible_playbook_group` for an ansible user.
2. Create a user `ansible_playbook_user` for processing ansible tasks. Add the user to the newly created group.
3. Set sudo rights for the ansible user.
4. Authorize users from `ansible_users` to login as an ansible user via SSH.

This role can be run under all versions of Ubuntu and Debian.

## Requirements

None

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
ansible_playbook_user: ansible      # An ansible user and a group to create
ansible_playbook_group: ansible
```

After that you can use the created account to login as an ansible user. Add existed users (see `users_accounts` variable, ansible role `users`) to the `ansible_users` list in the playbook to do that (see `vars/users.yml`).

```yaml
ansible_users: []     # A list of ansible users
```

For each user you need to specify `name` (required) and an SSH public key as `key` (required). You can also add `state` parameter (by default, it's `present`). Set `absent` to revoke the user authorization as an ansible user.

```yaml
ansible_users:
  - name: ""      # A user name
    key: ""       # An SSH user public key as a string or (since 1.9) url
```

## Dependencies

None

## Example Playbook

```yaml
- hosts: all
  roles:
    - role: ansible-user
  vars_files:
    - vars/users.yml
```

Inside `vars/users.yml`:

```yaml
ansible_users:
  - name: alex    # Allow the user `alex` to login as an ansible user via SSH with the key:
    key: "{{ lookup('file', 'files/public_keys/alex.pub') }}"
    state: present
  - name: jack
    key: "{{ lookup('file', 'files/public_keys/jack.pub') }}"
    state: present
```

where `files/public_keys/alex.pub` is a user public key.

## License

Licensed under the [MIT License](https://opensource.org/licenses/MIT).
