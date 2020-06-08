# Ansible Role: Postfix

This is a basic Ansible role to manage Postfix. It is limited by design.

## Role Variables

### Required Variables

- ``postfix_configure_firewalld`` (default: `true`): Set to `true` to configure firewalld to allow incoming smtp connections. Set to `false` to skip configuring firewalld.
- ``postfix_enable_tls`` (default: `false`): Set to `true` to enable TLS. Setting it to `true`, requires to set the variables `postfix_ssl_directory`, `postfix_ssl_certificate`, `postfix_ssl_key` as well. Set to `false` to skip configuring TLS.
- ``postfix_ssl_directory`` (default: `/etc/postfix/ssl`): Only required when `postfix_enable_tls` is set to `true`. Path to directory where the SSL files will be placed.
- ``postfix_ssl_certificate``: Only required when `postfix_enable_tls` is set to `true`. Filename of the certificate file. File needs to be present in directory `files/`.
- ``postfix_ssl_key``: Only required when `postfix_enable_tls` is set to `true`. Filename of the certificate key file. File needs to be present in directory `files/`.
- ``mydomain``: Domain name of mail server. Example: `sensson.net`.
- ``mynetworks``: List of subnets / IP addresses that are allowed to send e-mail through the mail server.

### Optional Variables

- ``myhostname`` (default: `{{ ansible_nodename }}`): Hostname of the mailserver. When unset it will default to `{{ ansible_nodename }}`.
- ``myorigin`` (default: `{{ ansible_nodename }}`): The domain name that locally-posted mail appears to come from, and that locally posted mail is delivered to. When unset it will default to `{{ ansible_nodename }}`.
- ``inet_interfaces`` (default: `all`): The network interface addresses that this mail system receives mail on. When unset it will default to `all`.
- ``postfix_additional_config``: Postfix has an enormous amount of configurable variables. You may add undefined variables to this variable.

## Example variables file

```yaml
---
# Setting to false will skip firewalld configuration
# Default: true
postfix_configure_firewalld: false

# Setting to true will enable TLS
# Default: false
postfix_enable_tls: true

# Directory to place SSL certificate files, will be ignored when postfix_enable_tls is false
# Default: /etc/postfix/ssl
postfix_ssl_directory: /etc/postfix/ssl

# SSL Certificate files, needs to be placed in directory files/
postfix_ssl_certificate: postfix_ssl_certificate.crt
postfix_ssl_key: postfix_ssl_key.pem

# CONFIGURATION

# REQUIRED
# Postfix configuration
mydomain: example.com
mynetworks:
  - 192.168.1.0/24
  - 10.0.0.1/32

# OPTIONAL
# Postfix configuration
# myhostname: kvm-smtp01-srv.example.com # When not set, will default to {{ ansible_nodename }}
# myorigin: kvm-smtp01-srv.example.com # When not set, will default to {{ ansible_nodename }}
# inet_interfaces: eth0 # When not set, will default to 'all'

# OPTIONAL
# Place to set undefined values
postfix_additional_config:
  smtp_destination_concurrency_limit: 10
  smtp_destination_rate_delay: 1s
  smtp_extra_recipient_limit: 10
```

## Limitations

This module has been tested on:

- CentOS 7, 8

## Development

This module was made by Polat Consultancy for Sensson.

We strongly believe in the power of open source. This module is our way of
saying thanks.
