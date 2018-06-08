Role Name : Gluster X509 Ansible role
=========

Prepares gluster clients and servers to run using SSL/TLS with self-signed certificates

 - creates keys, certificates (and requests)in the default (expected) locations
 - creates a "bundle CA" file and installs it in the default (expected) location
 - can create and install Diffie-Hellman parameters
 - touches the secure-access config file 
 - it will not overwrite existing certificates, keys or dhparam
 - it will not remove the secure-access config file
 - can be used with all clients and servers to update the bundle CA on all hosts
 - deamons will need to be restarted manually if the are running with an old bundle CA

Requirements
------------

- openssl installed in path or possibility to install the package.
- yum package manager 

This role is intended as a precursor to running one of the following roles:
 - zaxos.glusterfs-ansible-role  ( https://galaxy.ansible.com/zaxos/glusterfs-ansible-role/ ) 
 - zaxos.glusterfs-client-ansible-role ( https://galaxy.ansible.com/zaxos/glusterfs-client-ansible-role/ )

This role functions fine without the above roles and would most likely be applicable to other gluster roles.

Role Variables
--------------

    gluster_x509_signing:  self_signed # type of certificate 
    gluster_x509_local_ca_bundle_dir: /tmp/test_gluster_x509  
    # ca path
    gluster_x509_ca_bundle_path:   /etc/ssl/glusterfs.ca
    #private key
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
    # Set to true to testing as local nonroot user
    gluster_assume_openssl_package_installed: False
    #false for testing
    gluster_var_lib_glusterd_secure_access: True

Dependencies
------------

Example Playbook
----------------

For now, check the tests/test.yml

Some examples after more testing.

License
-------

BSD

Author Information
------------------

https://github.com/mkgin
