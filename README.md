# Ansible Role: Dehydrated

An Ansible Role that installs [dehydrated](https://github.com/dehydrated-io/dehydrated).

## Usage

### Minimal configuration

```yaml
vars:
  dehydrated_contact_email: ""
  dehydrated_domains:
    - name: "example.com"

roles:
  - role: dehydrated
```

### Full example

```yaml
vars:
  dehydrated_version: "v0.7.0"
  dehydrated_contact_email: ""
  dehydrated_location: "/usr/local/share/dehydrated"
  dehydrated_binary: "/usr/local/bin/dehydrated"
  dehydrated_config_dir: "/usr/local/etc/dehydrated"
  dehydrated_certs_dir: "{{ dehydrated_config_dir }}/certs"
  dehydrated_wellknown_dir: "{{ dehydrated_config_dir }}/challenge"
  dehydrated_force_update: True
  dehydrated_domains:
    - name: example.com
      alternate_names:
        - www.example.com
        - web.example.com
      deploy_challenge_hook: printf 'server 127.0.0.1\nupdate add _acme-challenge.%s 300 IN TXT "%s"\nsend\n' "${DOMAIN}" "${TOKEN_VALUE}" | nsupdate -k /var/run/named/session.key
      clean_challenge_hook: printf 'server 127.0.0.1\nupdate delete _acme-challenge.%s TXT "%s"\nsend\n' "${DOMAIN}" "${TOKEN_VALUE}" | nsupdate -k /var/run/named/session.key

roles:
  - role: dehydrated
```

