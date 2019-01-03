Role Name
=========

This role will install shibboleth on CentOS 7 and allow you to add Shibboleth key/certs and configurations
This is based on the tutorial https://accc.uic.edu/answer/how-do-i-install-and-configure-shibboleth

Requirements
------------

You will need to generate the private key and cert for shibboleth

````bash
sudo ./keygen.sh -h foo.example.uic.edu -e https://foo.example.uic.edu/shibboleth -f -y 10
````

You will also need to use th private key and certificate generated in the previous step to register your SP (Service Provider) on I-Trust (Identity Provider) 

To register with I-Trust please go to https://accc.uic.edu/answer/how-do-i-install-and-configure-shibboleth and follow the section **Register your service provider with I-Trust federation**


Role Variables
--------------

You will need to provide the key and certs for this module which we generated in the previous steps.
Please read about ansible vault you should place the key in the vault and only link to it from your host vars https://docs.ansible.com/ansible/latest/user_guide/vault.html

I-Trust cert is available here https://discovery.itrust.illinois.edu/itrust-certs/itrust.pem 

**Example:**
vars.yml
````yml
shibbolethsp_key: "{{ vault_shibbolethsp_key }}"
shibbolethsp_cert: "{{ vault_shibbolethsp_cert }}"
shibbolethsp_itrust_cert: "{{ vault_shibboleth_itrust_cert }}"
shibbolethsp_entity_id: "https://foo.example.uic.edu/shibboleth"
````
vault.yml
````yml
vault_shibbolethsp_key: |
  -----BEGIN PRIVATE KEY-----
  Your Private key goes here (KEEP this encrypted, NEVER SHARE IT!)
  -----BEGIN PRIVATE KEY-----
  
vault_shibbolethsp_cert: |
  -----BEGIN CERTIFICATE-----
  your certificate goes here
  -----END CERTIFICATE-----
  
vault_shibboleth_itrust_cert: |
  -----BEGIN CERTIFICATE-----
  your I-Trust certificate goes here
  -----END CERTIFICATE-----

````
Dependencies
------------

If you are using SELinux (Which you should be).
You should go to https://github.com/nevoband/semodule and call the shibboleth selinux module (Please follow README)

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: shibboleth-itrust }

License
-------

BSD
