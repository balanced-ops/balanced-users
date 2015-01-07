# balanced-users

An ansible role that manages the user files of the balanced team on remote servers or local machines for a consistent shell feel and personalizations.

**NOTE**: This role is purposefully not prefixed with the common `ansible-*` namespace because it's specialized for [@balanced-ops](https://github.com/balanced-ops) use. It is just here for reference if someone finds it useful.

## Compatibility

- Ubuntu 12.04
- Ubuntu 14.04
- CentOS 6.4
- Amazon Linux

## Features

- Can use github ssh keys

### EC2 Specific
- Removes the default sudoer from ec2 (whether its ec2-user or ubuntu)