Exim4
=====

Installs and configures Exim4 as an internet mailer. Role features include:

* SPF record checking on inbound mail
* Antivirus support (ClamAV)
* Anti spam support (Spamassassin)
* Authentication via SASLauthd
* DKIM mail signing on outbound mail (work in progress)
* Fail2ban integration for restricting automated attacks on port 25

DKIM mail verification on inbound mail is still planned.

This has only been tested on Debian Wheezy but in theory will work on any
Debian based system with Exim4 (and possibly other platforms if the package
install routines are changed)

Requirements
------------

This role doesn't install an antivirus so if you wish to use that functionality
you will need to install one yourself.

Because this role uses sasl for authentication by default it authenticates
against the system user database. Thats fine where user=account but not if you
intend to do virtual mail hosting (where mail accounts are separate to system
accounts).
In those cases the authentication part of the role may need to be changed -
feel free to let me know if you would like to see options available in the role

Role Variables
--------------

Exim4 configuration template variables and their defaults

```
exim_ssl_country: AU
exim_ssl_state: Tasmania
exim_ssl_locality: Glenorchy
exim_ssl_common_name: ''
exim_ssl_challenge_password: ''
exim_ssl_output_passsword: ''
exim_primary_hostname: 'example.com'
exim_domainlist_local_domains: '@:localhost'
exim_domainlist_relay_to_domains: ''
exim_hostlist_relay_from_hosts: '127.0.0.1'
exim_av_scanner_enable: false
exim_av_scanner_path: clamd:/var/run/clamav/clamd.ctl
exim_spamd_enable: false
exim_spamd_address: '127.0.0.1 783'
exim_tls_enable: true
exim_tls_advertise_hosts: ''
exim_tls_certificate: '/etc/ssl/exim.crt'
exim_tls_privatekey: '/etc/ssl/exim.pem'
exim_daemon_smtp_ports: '25 : 465 : 587'
exim_tls_on_connect_ports: '465'
exim_check_rfc2047_length_enable: false
exim_acl_check_rcpt_dnsbls_enable: true
exim_acl_check_rcpt_host_lookup_enable: true
exim_acl_check_rcpt_spf_enable: true
exim_acl_check_data_malware_enable: true
exim_acl_check_data_spam_enable: true
exim_routers_localuser_use_maildir: true
exim_transport_remote_smtp_dkim_enable: false
exim_transport_remote_smtp_dkim_domain: '${lookup{$sender_address}lsearch*@{/etc/exim4/dkim-senders.list}}'
exim_transport_remote_smtp_dkim_selector: '$dkim_domain'
exim_transport_remote_smtp_dkim_private_key: ''
exim_transport_remote_smtp_dkim_canon: 'relaxed'
exim_transport_remote_smtp_dkim_strict: 'false'
exim_transport_remote_smtp_dkim_sign_headers: ''
```

# Should we install the fail2ban filter for exim+saslauthd
exim_fail2ban_enable: false

Further work
------------
* Finish DKIM support
* Split clamav/spamassassin/sasl install and configuration to their own roles
* Split out OpenSSL component

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

For this example set your variables in group_vars/servers.

    - hosts: servers
      roles:
         - goetzk.exim

License
-------

GPL2+

Author Information
------------------

Issues or feedback can be reported to the author at karl@kgoetz.id.au; please
prefix the subject with 'ansible' or 'role'.

