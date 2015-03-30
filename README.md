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

To use bdr, you need to add these variables::

  - role: jpic.pg
    pg_track_commit_timestamp: 'on'
    pg_shared_preload_libraries: 'bdr'
    pg_bdr_default_apply_delay: 2000
    pg_bdr_log_conflicts_to_table: 'on'

Also important, set a hash of hostname/ip for postgresql servers::

  - role: jpic.pg
    pg_ips:
      bdr1: 172.16.17.101
      bdr2: 172.16.17.102

Finnaly, the easiest way to just create a replicated database and user::

  - role: jpic.pg
    pg_dbclients:
    - user: john
      pass: foo
      replicator_pass: barfoo

The above example is equivalent to::

  - role: jpic.pg
    pg_dbclients:
    - user: john
      db: john
      pass: foo
      replicator: john_replication
      replicator_pass: barfoo

Which, in turn, is the equivalent of::

  - role: jpic.pg
    pg_users:
    - name: john
      pass: foo
    - name: john_replicator
      pass: barfoo
      role_attr_flags: REPLICATION,LOGIN,SUPERUSER
    pg_databases:
    - name: john
    pg_privileges:
    - user: john
      db: john
      priv: all
    pg_replication:
    - name: bdr1john
      auth_method: md5
      dsn:
        dbname: john
        user: john_replication
        host: bdr1
      master_for:
      - host: bdr2
        local_dsn:
          dbname: john
          user: john_replication
    - name: bdr2john
      auth_method: md5
      dsn:
        dbname: john
        user: john
        host: bdr2

While the above example demonstrates the precision with
which this role is able to fine-tune, it's a bit boring - use pg_dbclients for
simple needs.

License
-------

BSD
