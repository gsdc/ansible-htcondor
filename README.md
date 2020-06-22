ansible-htcondor
=========

This role is designed to help you install and configure the HTCondor Job Manager.

Requirements
------------

More tests will be needed, but until now there seems to be no special requirement. However, this role has the requirement that the EL system must be because the package was written to use the yum command. Other OS support will not be available for the time being. However, if anyone gives a Pull Request for this, we will add it.

Role Variables
--------------

This role still has many areas to improve. There are many features that are introduced as variables but not yet supported. Here is the contents of the default/main.yml file for this role.

``` bash
# defaults file for roles/ansible-htcondor
condor_version: "8.8"
condor_admin: ""         #condor_admin@email.com
condor_daemon_list: ""   # "MASTER,SCHEDD,STARTD,GANGLIAD"
condor_domain: "{{ ansible_domain }}"
condor_host: ""
condor_name: ""
condor_pool_username: ""
enable_dynamicslot: False
enable_singularity: False
use_singularity_image_on_cvmfs: False
singularity_image_path: ""
firewall_lowport: 9000
firewall_highport: 9999
firewall_whitelist: []
extra: ""
```

In this role, save the configuration file into the *01-cluster.conf* and *02-local.conf files* in the /etc/condor/config.d/ directory. The cluster.conf file contains configuration files that are common to HTCondor Pool, and the local.conf file contains settings for each machine, such as condor_daemon_list.


Dependencies
------------

For now, to support singularity configuration does not be included. If it will be updated, "cvmfs" role will be required.

Example Playbook
----------------

Below is a small modification to the settings we actually use. Please make the configuration based on the following information:

    - hosts: servers
      roles:
      - role: geonmo.ansible_htcondor 
        condor_admin: "admin@localhost.lo"
        condor_daemon_list: "MASTER, STARTD"
        condor_domain: "localhost.lo"
        condor_host: "condor.local.lo"
        condor_name: "HTCondor Cluster"
        enable_dynamicslot: true

License
-------

BSD

Author Information
------------------

Email : geonmo@kisti.re.kr
