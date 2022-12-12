ansible-htcondor
=========

This role is designed to help you install and configure the HTCondor Job Manager.
We tried to implement the same environment as the get\_htconor script provided by HTCondor Homepage.
Several settings used by our institution have been added.

Requirements
------------

More tests will be needed, but until now there seems to be no special requirement. However, this role has the requirement that the EL system must be because the package was written to use the yum command. Other OS support will not be available for the time being. However, if anyone gives a Pull Request for this, we will add it.

Role Variables
--------------

This role still has many areas to improve. There are many features that are introduced as variables but not yet supported. Here is the contents of the default/main.yml file for this role.

``` bash
# defaults file for roles/ansible-htcondor
condor_version: "9.0"
condor_admin: ""         #condor_admin@email.com
condor_daemon_list: ""   # "MASTER,SCHEDD,STARTD,GANGLIAD"
condor_domain: "{{ ansible_domain }}"
condor_host: ""
condor_name: ""
condor_pool_password_file_path: "/etc/condor/passwords.d/POOL"
condor_pool_password: "hello world"
enable_dynamicslot: False
enable_singularity: False
firewall_lowport: 9000
firewall_highport: 9999
firewall_whitelist: []
step: "default"
extra: ""
```

In this role, save the configuration file into the *01-cluster.conf* and *02-local.conf files* in the /etc/condor/config.d/ directory. The cluster.conf file contains configuration files that are common to HTCondor Pool, and the local.conf file contains settings for each machine, such as condor_daemon_list.

* ```condor_version``` : Select the HTCondor version to install. This version selection is supported only after version 9.0 (LTS: 9.0, 10.0)
* ```condor_admin``` : Set up the administrator email for the HTCondor cluster.
* ```condor_daemon_list``` : Based on the official installation script, the following settings are recommended.
   * CentralManager : MASTER, NEGOTIATOR, COLLECTOR
   * Submit : MASTER, SCHEDD
   * Execute : MASTER, STARTD
* ```condor_doamin``` : Domain settings to be specified for *FILESYSTEM_DOMAIN*, *UID_DOMAIN* during HTCondor settings.
* ```condor_host``` : CentralManager's hostname(FQDN)
* ```step``` : Choose step to run, (default[install+config], install, config)


Example Playbook
----------------

Below is a small modification to the settings we actually use. Please make the configuration based on the following information:

    - hosts: servers
      roles:
      - role: geonmo.htcondor 
        condor_admin: "admin@localhost.lo"
        condor_daemon_list: "MASTER, STARTD"
        condor_domain: "localhost.lo"
        condor_host: "condor.local.lo"
        condor_name: "HTCondor Cluster"
        enable_dynamicslot: true
        enable_singularity: true

License
-------

BSD

Author Information
------------------

Email : geonmo@kisti.re.kr
