---
layout: post
title: Mount Azure Storage in VM
---


With ansible we can mount a azure storage share file in a Virtual Machine (VM).


**Example**

playbook.yml

```
- hosts: dde
  become: True
  vars:
    storage_accounts_credentials_directory: "/etc/smbcredentials"
    shared_home_directory: "/var/azstorage"
    storage_accounts:
      - name: "STORAGE_ACC_NAME"
        sharename: "SHARENAME"
        mount_path: "{{ shared_home_directory }}"
        creates: "{{ shared_home_directory }}/data"
        storage_key: "STORAGE_ACCESS_KEY"
  tasks:
    - name: ping
      ping:
    
    - name: Create Credentials folder
      file:
        path: "{{ storage_accounts_credentials_directory }}"
        state: directory
        mode: 00600
        owner: "root"
        group: "root"

    - name: Create Credentials files
      copy:
        content: "username={{ item.name }}\npassword={{ item.storage_key }}"
        dest: "{{ storage_accounts_credentials_directory }}/{{ item.name }}.cred"
        mode: 00600
      with_items: "{{ storage_accounts }}"

    - name: Mount Storage Account
      mount:
        src: "{{ '//' + item.name + '.file.core.windows.net/' + item.sharename }}"
        path: "{{ item.mount_path }}"
        fstype: cifs
        opts: nofail,vers=3.0,credentials={{ storage_accounts_credentials_directory }}/{{ item.name }}.cred,uid=1000,forceuid,gid=1000,forcegid,dir_mode=0740,file_mode=0740,serverino
        state: mounted
      with_items: "{{ jira_storage_accounts }}"
```

Command to execute the above playbook

```
ansible-playbook -i host.ini playbook.yml --become-user root --ask-become-pass

```
