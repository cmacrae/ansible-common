Common
======
An ansible role to manage miscellaneous types of data/resources/constructs in an ad-hoc fashion.

Current manageable resources
----------------------------
- Users
- Groups
- Cron jobs
- Packages 
- Files
- Remote files (URL fetching)
- Lines in files
- `sudo` configurations
- Package repositories (Yum at the moment, more to come!)

Requirements
------------
Ansible >= 2.0.0

Variables
---------
|     Name     |     Default Value    |    Description     |
|---------------------------|-----------------------|------------------------------------|
| `common_users` | not_defined | A dictionary of misc users to manage (supports all params presented by the `user` module) |
| `common_groups` | not_defined | A dictionary of misc groups to manage (supports all params presented by the `group` module) |
| `common_cron_jobs` | not_defined | A dictionary of misc cron jobs to manage (supports all params presented by the `cron` module) |
| `common__packages` | not defined | A list of misc packages to install |
| `common_files` | not_defined | A dictionary of misc files to manage (supports all params presented by the `file` module) |
| `common_fetch_files` | not_defined | A dictionary of misc remote files to fetch from from urls (supports all params presented by the `get_url` module) |
| `common_file_lines` | not_defined | A dictionary of file lines to manage (supports all params presented by the `lineinfile` module) |
| `common_sudoer_configs` | not_defined | A dictionary of misc sudoers rules to manage |
| `common_yumrepos` | not_defined | A dictionary of Yum repositories to manage |


Example definitions
----------------
You can define any data/resources/constructs at any level you wish (`host_vars`,`group_vars` playbook `vars`, a `vars` file, in a role that depends on this role, etc.)

### Users
``` yaml
common_users:
  some_user:
    group: mygroup
    generate_ssh_key: true
    ssh_key_comment: "This is an example user's keypair"
  some_other_user: {}
```

### Groups
``` yaml
common_groups:
  mygroup: {}
    my_other_group:
     gid: 9001
     system: true
```

### Cron jobs
``` yaml
common_cron_jobs:
  'check dirs':
    minute: 0
    hour: '5,2'
    job: 'ls -alh > /dev/null'
    state: absent
  'some old job':
    state: absent
  'a job to run at reboot':
    special_time: reboot
    job: '/path/to/some_scrupt.sh'
    state: absent
  'a job with a cron file':
    job: 'uptime &> /dev/null'
    cron_file: cron_example
    state: absent
```

### Packages
``` yaml
common_packages:
  - mg
  - golang
```

### Files
``` yaml
common_files:
  /tmp/myfile:
    owner: some_user
    group: some_user
    mode: 0644
    src: /tmp/my_example_file
  /tmp/my_other_file:
    owner: some_user
    group: some_user
    mode: 0644
    content: >-
      These contents will be inserted into
      /tmp/my_other_file.
```

### Remote files (URL fetching)
``` yaml
common_fetch_files:
  git_fat:
    url: 'https://raw.githubusercontent.com/jedbrown/git-fat/master/git-fat'
    dest: '/usr/bin/git-fat'
    mode: 755 # (cannot use explicit mode here)
    owner: root
    group: root
```

### Lines in files
``` yaml
common_file_lines:
  /etc/hosts:
    regexp: '^127\.0\.0\.1'
    line: '127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4'
    owner: root
    group: root
    mode: 0644
  /etc/sudoers:
    state: absent
    regexp: "^%wheel"
```

### `sudo` configurations
``` yaml
common_sudoer_configs:
  hubot_multi_sudo_rules:
    content:
      - 'hubot ALL=(root) NOPASSWD: /usr/bin/systemctl restart hubot-*'
      - 'hubot ALL=(root) NOPASSWD: /usr/bin/systemctl start hubot-*'
      - 'hubot ALL=(root) NOPASSWD: /usr/bin/systemctl status hubot-*'
      - 'hubot ALL=(root) NOPASSWD: /usr/bin/systemctl stop" hubot-*'
  hubot_just_date_cmd:
    content:
      - 'hubot ALL=(root) NOPASSWD: /bin/date'
```

### Package repositories (more to come!)
``` yaml
# Deploy Yum repositories
common_yumrepos:
  RethinkDB:
    descr: RethinkDB
    baseurl: 'http://download.rethinkdb.com/centos/6/x86_64'
    gpgcheck: 0
    enabled: 1
```

License
-------
MIT

Author Information
------------------
------------------
Created by [Calum MacRae](http://cmacr.ae)

Feel free to:  
Contact me - [@calumacrae](https://twitter.com/calumacrae), [mailto:calum0macrae@gmail.com](calum0macrae@gmail.com)  
[Raise an issue](https://github.com/cmacrae/ansible-common/issues)  
[Contribute](https://github.com/cmacrae/ansible-common/pulls)  
