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
- Removes the default sudoer from ec2 (whether its ec2-user or ubuntu)

## Usage

`balanced_users_<identifier>` is the modifications applied to a user.

The only **required** field is `name`.

List of all recognized values, default value listed first.

Example:

```yaml

balanced_users_global_admins:     # sets a global list of admins referencing users
 - balanced_users_mahmoud
 - balanced_users_${username}

balanced_users_${username}:
 - name: 'username'               # mandatory, default group if not defined
   state: 'present,absent'
   group: 'name'                  # defaults to the name of the user
   groups: []                     # list of groups to set
   append: yes/no                 # add to, or set groups
   shell: '/bin/bash'             # defaults to /bin/bash
   system_user: no/yes            # create system user, defaults to no
   system_group: no/yes           # create system group, defaults to no

   comment: '${username} is a really cool person'

   dotfiles: False/True           # download and configure dotfiles?
   dotfiles_git_repo: 'repository'
   dotfiles_install_command: 'make all'
   dotfiles_creates '~/.zshrc'    # idempotent create

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
```

## LICENSE

MIT


## Inspirations

- [mivok/ansible-users](https://github.com/mivok/ansible-users)
- [debops/ansible-users](https://github.com/debops/ansible-users)