Exim4
=====

Installs and configures Exim4 as an internet mailer. Features include:
 * SPF record checking on inbound mail
 * Antivirus support (ClamAV)
 * Anti spam support (Spamassassin)
 * Authentication via SASLauthd
 * DKIM mail signing on outbound mail (work in progress)

DKIM mail verification on inbound mail is still planned.

This has only been tested on Debian Wheezy but in theory will work on any
Debian based system with Exim4 (and possibly other platforms if the package
install routines are changed)

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.


Exim4 configuration template variables and their defaults

exim_primary_hostname: 'example.com'
exim_domainlist_local_domains: '@:localhost'
exim_domainlist_relay_to_domains: ''
exim_hostlist_relay_from_hosts: '127.0.0.1'
exim_av_scanner_enable: true
exim_av_scanner_path: clamd:/var/run/clamav/clamd.ctl
exim_spamd_enable: true
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

FIXME: make clamav/spamassassin/sasl install only on if some variables are set?
FIXME: split out those in to their own roles?

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

GPL2+

Author Information
------------------

Issues or feedback can be reported to the author at karl@kgoetz.id.au; please
prefix the subject with 'ansible' or 'role'.

