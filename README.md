# balanced-users
[![Build Status](http://img.shields.io/travis/balanced-ops/balanced-users.svg?style=flat-square)](https://travis-ci.org/balanced-ops/balanced-users)

An ansible role that manages the user files of the balanced team on remote servers or local machines for a consistent shell feel and personalization.

**NOTE**: This role is purposefully not prefixed with the common `ansible-*` namespace because it's specialized for [@balanced-ops](https://github.com/balanced-ops) use. It is just here for reference if someone finds it useful.

## Compatibility

- Ubuntu 12.04
- Ubuntu 14.04
- CentOS 6.4
- Amazon Linux

## Features

- Can use github ssh keys via the `authorized_ssh_keys_from_github_username` attribute
- Can remove users and groups
- Allows for customizations per user, see `vars/test_vars.yml` for an example

### EC2 Specific
- Removes the default ec2 login SSH keys (whether its ec2-user or ubuntu)

## Usage

`balanced_users_<identifier>` is the modifications applied to a user.

The only **required** field is `name`.

Example of configuration below. We list of all configurable values, and they're
set to their default value.

```yaml

balanced_users_admins_list:     # sets a global list of admins referencing users
 - user_mahmoud
 - user_${username}

user_${username}:
 - name: 'username'               # mandatory, default group if not defined
   state: 'present,absent'
   shell: '/bin/bash'             # defaults to /bin/bash

   group: 'name'                  # defaults to the name of the user
   # groups here must exist on the system, if they don't, you must
   # create them by setting the `balanced_users_groups_list`
   groups: []                     # list of groups to set
   append: yes/no                 # add to, or set groups

   # http://unix.stackexchange.com/questions/80277/whats-the-difference-between-a-normal-user-and-a-system-user
   system_user: no/yes            # create system user, defaults to no
   # http://askubuntu.com/questions/523949/what-is-a-system-group-as-opposed-to-a-normal-group
   system_group: no/yes           # create system group, defaults to no

   comment: '${username} is a really cool person'

   # dotfiles support!
   dotfiles: False/True           # download and configure dotfiles?
   dotfiles_git_repo: 'repository'
   dotfiles_install_command: 'make all'

   # Add ssh authorized keys
   authorized_ssh_keys:
     - "ssh-rsa ..... lol@lama"
     - "ssh-rsa ..... lol@lama.gandalf"

   # If set, will pull ssh keys from github
   authorized_ssh_keys_from_github_username: mahmoudimus

   # Remove specific ssh key (if user state==absent, removes all keys)
   revoked_ssh_keys:
     - "ssh-rsa ..... lol@lama.revokeme"


   password: 's0m3-s3cr3t-!!'     # mkpasswd --method=SHA-512 or cat /etc/shadow
   update_password: 'on_create'   # always will update passwords if they differ.
                                  # on_create will only set the password for newly created users. (added in Ansible 1.3)


balanced_users_groups_list:     # sets a global list of groups to create
 - group_admin

group_admin:
 - name: 'group-name'               # mandatory
   state: 'present,absent'
   is_sudoer: 'no/yes'              # will add to /etc/sudoers.d/50-{{name}}
```

## TODO

Sudoer work
 - probably need to define system wide admin role

## LICENSE

MIT


## Inspirations

- [mivok/ansible-users](https://github.com/mivok/ansible-users)
- [debops/ansible-users](https://github.com/debops/ansible-users)