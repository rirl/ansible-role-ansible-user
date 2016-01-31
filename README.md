Ansible user role
=========

An Ansible Role that creates user for running playbooks and allows connections via ssh for users from `users` list (using public keys).

Requirements
------------

None.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    ansible_playbook_user: "ansible"
    ansible_playbook_group: "admin"

User and group to create. After that you can use it as `ansible_user` in playbook.

    users: []

Add next properties to users: `name` (required), `email` (required) and ssh public key as `key` (required).

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: all
      roles:
        - role: ansible-user
      vars_files:
        - vars/users.yml

Inside `vars/users.yml`:

    users:
      - name: john
        email: john.doe@domain.tld
        key: "{{ lookup('file', 'files/public_keys/john.pub') }}"

where `files/public_keys/john.pub` is a user public key.

License
-------

MIT

