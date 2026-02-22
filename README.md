# Ansible Infrastructure 

This repository contains Ansible playbooks and roles for managing my own, minimal infrastructure.
Coming from Chef Solo with Kitchen, I finally wanted to try Ansible for a change.
This is a work in progress, and I will update it as I go along.

## Layout
I will try to follow the Ansible best practices for directory layout, which will be something like this:

```
.
├── ansible.cfg
├── inventory/
│   ├── group_vars/
│   │   └── webserver.yml
│   └── production.ini
├── playbooks/
│   ├── deploy-blog.yml
│   └── deploy-webserver.yml
└── roles/
    └── webserver/
        └── templates/
            └── nginx-site.conf.j2
```

