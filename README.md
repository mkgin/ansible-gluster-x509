Role Name : Gluster X509 Ansible role
=========

Prepares gluster clients and servers to run using SSL/TLS with self-signed certificates or using a local CA

 - creates keys, certificates (and requests)in the default (expected) locations
 - creates a "bundle CA" file and installs it in the default (expected) location
 - can create and install Diffie-Hellman parameters
 - touches the secure-access config file so gluster will use SSL/TLS
 - it will not overwrite existing certificates, keys or dhparam (csr's are fair game)
 - it will not remove the secure-access config file
 - can be used with all clients and servers to update the bundle CA on all hosts
 - client and server deamons will need to be restarted manually when the CA configuration changes

Requirements
------------

- openssl installed in path or possibility to install the package.
- yum package manager  ( this is the only E.L. dependancy... will get around to testing on debian sometime) 

This role is intended as a precursor to running one of the following roles:
 - zaxos.glusterfs-ansible-role  ( https://galaxy.ansible.com/zaxos/glusterfs-ansible-role/ ) 
 - zaxos.glusterfs-client-ansible-role ( https://galaxy.ansible.com/zaxos/glusterfs-client-ansible-role/ )

This role functions fine without the above roles and would most likely be applicable to other gluster roles.

Role Variables
--------------

```
# defaults file for gluster-x509
# type of certificate to make self_signed or local_ca
gluster_x509_signing: self_signed  # or local_ca
# ca bundle creation and maintenance location (on the localhost that runs ansible)
gluster_x509_local_ca_bundle_dir: /tmp/test_gluster_x509_make_ca_bundle
# ca bundle path / if using a local CA where the CA cert is copied to 
gluster_x509_ca_bundle_path:   /etc/ssl/glusterfs.ca
gluster_x509_private_key_path: /etc/ssl/glusterfs.key
gluster_x509_private_key_bits: 2048
# certificate and certificate request
gluster_x509_certificate_subject: "/CN={{ ansible_hostname }}"
gluster_x509_cert_request_path: /etc/ssl/glusterfs.csr
gluster_x509_certificate_path: /etc/ssl/glusterfs.pem
gluster_x509_certificate_days: 400

# Diffie-Hellman
gluster_x509_dh_param_generate: False
gluster_x509_dh_param_path: /etc/ssl/dhparam.pem 
gluster_x509_dh_param_bits: 4096 # 4096 takes a while.

# local CA settings (not used if self signed)
gluster_x509_local_ca_path: /tmp/gluster_x509_test 
# config... use the --config option to if the variable is set empty the no config is used
gluster_x509_local_ca_config_option: "-config {{gluster_x509_local_ca_path}}/openssl.cnf"
gluster_x509_local_ca_passphrase: "secret"
gluster_x509_local_ca_privatekey: "{{gluster_x509_local_ca_path}}/private/cakey.pem"
gluster_x509_local_ca_requests_dir: "{{gluster_x509_local_ca_path}}/requests/"
gluster_x509_local_ca_newcerts_dir: "{{gluster_x509_local_ca_path}}/newcerts/"
gluster_x509_local_ca_certificate: "{{gluster_x509_local_ca_path}}/cacert.pem"

# use nolog in role where the password would be visible on the command line
gluster_x509_no_log_passwords: True

# Set to true to testing as local nonroot user
gluster_assume_openssl_package_installed: False
# Set to False to testing as local nonroot user
gluster_var_lib_glusterd_secure_access: True
```

Dependencies
------------

Example Playbook
----------------

For now, check the tests.

* `test_self_signed.yml`
* `test_local_ca.yml`

Tests should be copied linked to the main ansible playbook directory and run from there, for example:

`ansible-playbook -i roles.git/gluster-x509/tests/inventory test_local_ca.yml`

`test_local_ca.yml` makes a local test CA in the temporary directory. It depends on the inventory being correctly set ( connection=local for localhost)

License
-------

BSD

Author Information
------------------

https://github.com/mkgin
