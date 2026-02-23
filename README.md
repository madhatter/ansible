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

## Todos
- [ ] Migrate tasks from deploy-webserver.yml to webserver role tasks folder
    - [ ] tasks/main.yml e.g. for installing nginx and configuring folders and
      config files
    - [ ] handlers/main.yml e.g. for restarting nginx
- [ ] Configure nginx service itself
- [ ] Add letsencrypt role for SSL certificates
- [ ] Add blog role for deploying my blog
    - [ ] deployer user
    - [ ] git hook for automatic deployment

## More environments be like
```
inventory/                                                 
├── production/                                            
│   ├── hosts                 # inventory-Datei für Prod   
│   └── group_vars/                                        
│       └── webserver.yml     # Prod-Variablen             
│                                                          
└── staging/                                               
    ├── hosts                 # inventory-Datei für Staging
        └── group_vars/                                        
                └── webserver.yml     # Staging-Variablen          
```
