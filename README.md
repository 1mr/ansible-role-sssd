SSSD
====

Install and configure SSSD for sssd-level LDAP authentication against an LDAP-enabled Active Directory server.

Requirements
-------------

This role requires ansible 2.0 or higher.

Role Variables
--------------

The variables that can be passed to this role and a brief description about them are as follows:

```
ldap_user: ldap_user
ldap_pass: ********
sssd_dep: it-dep
sssd_ldap_domain: example.lcl
sssd_ldap_schema: ad
sssd_ldap_search_base: ou=Deps,dc=example,dc=lcl?subtree?{{ sssd_ldap_user_search_filter }}
sssd_ldap_user_search_base: ou=Deps,dc=example,dc=lcl?subtree?{{ sssd_ldap_user_search_filter }}
sssd_ldap_group_search_base: OU=IT,OU=Deps,DC=example,DC=lcl???OU=Servers,DC=example,DC=lcl??{{ sssd_ldap_group_search_filter }}
sssd_ldap_uris: ["ldaps://dc1.example.lcl/", "ldaps://dc2.example.lcl/"]
sssd_ldap_bind_dn: "{{ ldap_user }}@example.lcl"  
sssd_ldap_bind_password: "{{ ldap_pass }}"
sssd_ldap_user_search_filter: (&(objectClass=person)(!(userAccountControl=514))(|(memberOf=CN={{ inventory_hostname }},OU={{ ldap_ou }},OU=Servers,DC=example,DC=lcl)(memberOf=CN={{ inventory_hostname }}-www,OU={{ ldap_ou }},OU=Servers,DC=example,DC=lcl)(memberOf=CN={{ inventory_hostname }}-root,OU={{ ldap_ou }},OU=Servers,DC=example,DC=lcl)(memberOf={{ sssd_dep }},OU=IT,OU=Deps,DC=example,DC=lcl)))
sssd_ldap_group_search_filter: "(&(objectClass=group)(gidNumber=*)(|(CN={{ sssd_dep }})(CN={{ inventory_hostname }})(CN={{ inventory_hostname }}-www)(CN={{ inventory_hostname }}-root)))"
sssd_ldap_access_filter: (|(memberOf=CN={{ inventory_hostname }},OU={{ ldap_ou }},OU=Servers,DC=example,DC=lcl)(memberOf=CN={{ inventory_hostname }}-www,OU={{ ldap_ou }},OU=Servers,DC=example,DC=lcl)(memberOf=CN={{ inventory_hostname }}-root,OU={{ ldap_ou }},OU=Servers,DC=example,DC=lcl)(memberOf=CN={{ sssd_dep }},OU=IT,OU=Deps,DC=example,DC=lcl))
sssd_ldap_cacert: ca-certificates.crt

```

All variables above are defaults, except 'ldap_user' and 'ldap_pass'.

Dependencies
------------

Example Playbook
----------------

```
---
- hosts: all
  become: True
  roles:
    - { role: sssd, tags: sssd }

```

License
-------

BSD

Author Information
------------------

Created by Stas Stavnichuk 
