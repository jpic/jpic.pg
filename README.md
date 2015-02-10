jpic.pg
=======

This ansible role is able to compile and setup postgresql with bdr patches for
killer bi directionnal multi master replication.

Requirements
------------

Although I use Arch Linux, it should be easy to add support for bloatware
distros because this role does not rely on distro-specific patches and scripts.

Role Variables
--------------

Required variables:

- ``pg_ips``: dict of hostname: ip for each postgresql node.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Example setup with replication::

  - role: jpic.pg
    pg_bdr_path: /tmp/bdr
    pg_track_commit_timestamp: 'on'
    pg_shared_preload_libraries: 'bdr'
    pg_bdr_default_apply_delay: 2000
    pg_bdr_log_conflicts_to_table: 'on'
    pg_ips:
      bdr1: 172.12.17.101
      bdr2: 172.12.17.102
    pg_users:
    - name: testuser
      pass: testpass
    pg_databases:
    - name: testdb
    pg_privileges:
    - user: testuser
      db: testdb
      priv: all
    pg_replication:
    - name: bdr1testdb
      auth_method: trust
      dsn:
        dbname: testdb 
        user: postgres 
        host: bdr1
      master_for:
      - host: bdr2
        local_dsn:
          dbname: testdb
          user: postgres
    - name: bdr2testdb
      auth_method: trust
      dsn:
        dbname: testdb 
        user: postgres 
        host: bdr2

License
-------

BSD
