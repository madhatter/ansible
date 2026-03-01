# Ansible Infrastructure

Ansible playbooks and roles for managing a minimal VPS running Arch Linux (`brutal.nostalgix.org`).

## Layout

```
.
├── ansible.cfg
├── inventory/
│   ├── group_vars/
│   │   └── webserver.yml       # variables for the webserver group
│   └── production.ini          # hosts and connection settings
├── roles/
│   ├── webserver/              # nginx installation and site configuration
│   │   ├── defaults/main.yml
│   │   ├── handlers/main.yml
│   │   ├── tasks/main.yml
│   │   └── templates/
│   │       └── nginx-site.conf.j2
│   └── letsencrypt/            # certificate management via certbot
│       ├── defaults/main.yml
│       ├── handlers/main.yml
│       └── tasks/main.yml
├── deploy-webserver.yml
├── deploy-letsencrypt.yml
└── deploy-blog.yml             # work in progress
```

## Playbooks

### deploy-webserver.yml

Installs and configures nginx. Sites are data-driven via `nginx_sites` in
`inventory/group_vars/webserver.yml` — adding a site only requires a new entry
there, no changes to tasks or templates.

```bash
ansible-playbook deploy-webserver.yml
ansible-playbook deploy-webserver.yml --check   # dry-run
ansible-playbook deploy-webserver.yml -v        # verbose, shows nginx -t output
```

### deploy-letsencrypt.yml

Installs certbot, issues any missing certificates via the webroot method, and
enables the `certbot-renew.timer` for automatic renewal. Run after
`deploy-webserver.yml` so nginx is already serving port 80.

```bash
ansible-playbook deploy-letsencrypt.yml
```

Certificates are only issued if the `fullchain.pem` for a given cert name does
not yet exist — existing certificates are skipped. The webroot
(`/var/www/letsencrypt`) is served by all port 80 nginx server blocks via a
dedicated `location /.well-known/acme-challenge/` block.

**Before running on a fresh server**, set `certbot_email` in
`inventory/group_vars/webserver.yml`.

### deploy-blog.yml

Work in progress. Will create the `deployer` user and set up a git hook for
automatic Jekyll deployment.

## Fresh server setup order

```bash
ansible-playbook deploy-webserver.yml
ansible-playbook deploy-letsencrypt.yml
ansible-playbook deploy-webserver.yml   # reload nginx with SSL now that certs exist
```

## Certificate renewal

Renewal is handled automatically by `certbot-renew.timer`. To verify:

```bash
# Simulate the full renewal process without touching live certs
sudo certbot renew --dry-run

# Show when the timer last ran and when it will run next
systemctl list-timers certbot-renew.timer
```

## Adding a new nginx site

Add an entry to `nginx_sites` in `inventory/group_vars/webserver.yml`:

```yaml
- name: example.nostalgix.org
  server_name: example.nostalgix.org
  listen: 80
  root: /var/www/example
```

Supported per-site keys: `ssl`, `autoindex`, `access_log`, `index`, `redirects`.
Then add the corresponding entry to `certbot_certs` if it needs a certificate.

## Planned

- Blog role: `deployer` user, git hook for automatic Jekyll deployment
- Support multiple environments (production/staging) via separate inventory directories
