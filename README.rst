===============================
 Ansible Role: kickstart_files
===============================

This ansible role is used to render kickstart files for installing CentOS or RHEL systems. It only renders the files, it does not set up any mechanisms for serving the files.

----------------
 Role Variables
----------------

Key role variables are documented with their default values below. See ``defaults/main.yml`` for a full list.

::

    ksdef_root_pwhash: ""
    ksdef_ansible_pwhash: ""

These two variables *must* be defined as crypted Unix password strings. Use ``python -c "import crypt; print(crypt.crypt('<password>'))"`` to generate crypted Unix password hashes.

::

    kickstart_files_list:
        - path: "test.ks"

The list of dictionaries defining kickstart files to render. The ``path`` key is the only required key for each item. For all ``ksdef_<variable>`` variables defined in the ``defaults``, the ``variable`` key can be set for a given dictionary to override that default value for that specifc kickstart file.

------------------
 Example Playbook
------------------

::

    ---
    - hosts: localhost
      vars:
        ksdef_root_pwhash: {{ lookup('file', 'crypted_root_pw.txt') }}
        ksdef_ansible_pwhash: {{ lookup('file', 'crypted_ansible_pw.txt') }}
        kickstart_files_list:
            - path: "default.ks"
            
            - path: "centos75.ks"
              install_line: "url --url=\"http://example.com/centos/7.5/os/x86_64/\""
      roles:
        - kickstart_files
