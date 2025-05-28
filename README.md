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
  dehydrated_base_dir: "{{ dehydrated_config_dir }}"
  dehydrated_certs_dir: "{{ dehydrated_config_dir }}/certs"
  dehydrated_wellknown_dir: "{{ dehydrated_config_dir }}/challenge"
  dehydrated_force_update: True
  dehydrated_common_hooks:
    deploy_cert_hook: /usr/bin/systemctl reload nginx
  dehydrated_domains:
    - name: example.org
    - name: example.net
    - name: example.com
      alternate_names:
        - www.example.com
        - web.example.com
      deploy_cert_hook:     # empty
      deploy_challenge_hook: printf 'server 127.0.0.1\nupdate add _acme-challenge.%s 300 IN TXT "%s"\nsend\n' "${DOMAIN}" "${TOKEN_VALUE}" | nsupdate -k /var/run/named/session.key
      clean_challenge_hook: printf 'server 127.0.0.1\nupdate delete _acme-challenge.%s TXT "%s"\nsend\n' "${DOMAIN}" "${TOKEN_VALUE}" | nsupdate -k /var/run/named/session.key

roles:
  - role: dehydrated
```

### Migrate from Debian example

If you used the Debian dehydrated package before, you can migrate to
this role while keeping your account and cert data.  To mimic the
directory structure used by Debian, configure the role like this:

```yaml
dehydrated_contact_email: "acme@example.com"
dehydrated_config_dir: "/etc/dehydrated"
dehydrated_base_dir: "/var/lib/dehydrated"
dehydrated_certs_dir: "{{ dehydrated_base_dir }}/certs"
dehydrated_wellknown_dir: "{{ dehydrated_base_dir }}/acme-challenges"
```
