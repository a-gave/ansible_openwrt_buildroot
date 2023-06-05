Ansible Role: openwrt_buildroot
=========

Ansible role to generate OpenWrt firmware images with OpenWrt Buildroot.

Requirements
------------

Install Ansible requirements:

    pip3 install ansible
    pip3 install jinja2-ansible-filters

To use the ansible commands `ansible-playbook`, `ansible-galaxy`
Add the `~/.local/bin` path to your .bashrc or .bash_profile

    echo "export PATH=$PATH:$HOME/.local/bin" >> ~/.bashrc
    source ~/.bashrc

Install this role
------------

    git clone https://gitlab.com/a-gave/ansible_openwrt_buildroot.git ~/.ansible/roles/libremesh.openwrt_buildroot

Dependencies
------------

    
A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Choose a working directory
------------


Create inventory
------------
Define the host that will run the commands in `hosts` file

Example:

    # hosts
    yucca:
      ansible_host: 10.170.55.44
      ansible_user: antennine
      ansible_become_pass: "ciao"
      ansible_become_user: root
      ansible_become_method: su
      ansible_become_flags:

If you intend to build on `localhost` creating a specific user e.g `openwrt`
Uncomment the configuration to tell ansible to ask for password in `ansible.cfg`

    # ansible.cfg
    [defaults]
    callbacks_enabled = profile_tasks
    ask_pass = True

    [ssh_connection]
    scp_if_ssh=True

Example Playbook
--------------
Create a playbook file `build_openwrt.yml`

    # playbook.yml
    - name: Build OpenWrt
      hosts: localhost
      gather_facts: false
      vars:
        build_targets: 
          - openwrt_target: ath79
            openwrt_subtarget: generic
          - openwrt_target: x86
            openwrt_subtarget: 64

      tasks: 
        - name: Build OpenWrt - Loops targets.
          include_role: 
            name: libremesh.openwrt_buildroot 
          vars:
            openwrt_target: "{{ item.openwrt_target }}"
            openwrt_subtarget: "{{ item.openwrt_subtarget }}"
            openwrt_devices: "{{ item.openwrt_devices }}"
          loop: "{{ build_targets }}"

Build OpenWrt
--------------
Run the playbook passing the inventory file as parameter

    ansible-playbook -i hosts build_openwrt.yml

Role Variables
--------------




A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.
