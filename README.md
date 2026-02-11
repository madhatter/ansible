# Ansible Infrastructure 

This repository contains Ansible playbooks and roles for managing my own, minimal infrastructure.
Coming from Chef Solo with Kitchen, I finally wanted to try Ansible for a change.
This is a work in progress, and I will update it as I go along.

## Layout
I will try to follow the Ansible best practices for directory layout, which will be something like this:

```.
├── ansible.cfg
├── inventory
│   └── production.ini
├── playbooks
│   ├── deploy_webserver.yml
│   ├── deploy_blog.yml
├── roles
│   ├── common
│   │   ├── tasks
│   │   │   └── main.yml
│   │   ├── handlers
│   │   │   └── main.yml
│   │   ├── templates
│   │   │   └── ntp.conf.j2
│   │   └── files
│       └── motd
├── group_vars
│   └── all.yml
└── host_vars
    └── web1.yml
```

