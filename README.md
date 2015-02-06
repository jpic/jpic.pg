Role Name
=========

A brief description of the role goes here.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

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

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).


failed: [bdr1] => {"changed": true, "cmd": "initdb --auth-host=md5 --auth-local=peer -A trust --locale en_US.UTF-8 -D '/var/lib/postgres/data'", "delta": "0:00:00.022247", "end": "2015-02-07 00:03:36.553004", "rc": 1, "start": "2015-02-07 00:03:36.530757", "warnings": []}
stderr: initdb: invalid locale name "en_US.UTF-8"
stdout: The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

This is because the locale_gen module is broken with ArchLinux because of:

- https://github.com/ansible/ansible-modules-extras/pull/221
- https://github.com/ansible/ansible-modules-extras/pull/224

You need to apply those patches in ansible-modules-extras.
