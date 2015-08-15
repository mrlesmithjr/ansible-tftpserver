Role Name
=========

Builds out a fully functional PXE/TFTP Server for OS deployments.. (Ubuntu included using Netboot) Pre-Seed file template included

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------
group_vars/all
````
# To generate passwords use (replace P@55w0rd with new password).... echo "P@55w0rd" | mkpasswd -s -m sha-512
root_password:  #define root password for hosts....define here or in group_vars/all
````

group_vars/tftpserver
````
---
config_dnsmasq: true
config_interfaces: false
enable_apt_caching: false
enable_dhcp: false
enable_tftp: true
````

defaults/main.yml
````
---
# defaults file for ansible-tftpserver
apache_root: /var/www/html
apache_tftp_links:
  - images
config_tftp: true  #defines if tftp services should be configured
domain_name: '{{ pri_domain_name }}'
enable_apt_caching: false  #defines if apt-cacher-ng is setup and added to preseed.cfg
netboot_url: http://archive.ubuntu.com/ubuntu/dists/trusty-updates/main/installer-amd64/current/images/netboot/
netboot_file: netboot.tar.gz
# To generate passwords use (replace P@55w0rd with new password).... echo "P@55w0rd" | mkpasswd -s -m sha-512
pri_domain_name: example.org  #define here or globally in group_vars/all
sync_tftp: false  #defines if setting up multiple servers are to be configured for GlusterFS
root_password: [] #define root password for hosts....define here or in group_vars/all
tftp_bind_address: '{{ ansible_default_ipv4.address }}'
tftp_boot_menu:
#  - label: install
#    menu_label: Install
#    menu_default: false
#    kernel: ubuntu-installer/amd64/linux
#    append: 'vga=788 initrd=ubuntu-installer/amd64/initrd.gz -- quiet'
#  - label: cli
#    menu_label: 'Command-line install'
#    menu_default: false
#    kernel: ubuntu-installer/amd64/linux
#    append: 'tasks=standard pkgsel/language-pack-patterns= pkgsel/install-language-support=false vga=788 initrd=ubuntu-installer/amd64/initrd.gz -- quiet'
  - label: 'auto-install Ubuntu Netboot (Latest)'
    menu_label: 'Automated install Ubuntu (Latest)'
    menu_default: true
    kernel: ubuntu-installer/amd64/linux
    append: 'auto=true priority=critical vga=788 initrd=tftp://{{ tftp_bind_address }}/ubuntu-installer/amd64/initrd.gz locale=en_US.UTF-8 kbd-chooser/method=us netcfg/choose_interface=auto url=tftp://{{ tftp_bind_address }}/preseed.cfg'
#  - label: 'CentOS 7 (Manual)'
#    menu_label: 'CentOS 7 (Manual)'
#    menu_default: false
#    kernel: images/CentOS/7/images/pxeboot/vmlinuz
#    append: 'auto=true priority=critical vga=normal initrd=tftp://{{ tftp_bind_address }}/images/CentOS/7/images/pxeboot/initrd.img ip=dhcp inst.repo=http://{{ tftp_bind_address }}/images/CentOS/7'
#  - label: 'Ubuntu 12.04.5 (Manual)'
#    menu_label: 'Install Ubuntu 12.04.5 (Manual)'
#    menu_default: false
#    kernel: images/Ubuntu/12.04/install/netboot/ubuntu-installer/amd64/linux
#    append: 'auto=true priority=critical vga=788 initrd=tftp://{{ tftp_bind_address }}/images/Ubuntu/12.04/install/netboot/ubuntu-installer/amd64/initrd.gz locale=en_US.UTF-8 kbd-chooser/method=us netcfg/choose_interface=auto'
#  - label: 'Ubuntu 12.04.5 (Pre-Seed)'
#    menu_label: 'Install Ubuntu 12.04.5 (Pre-Seed)'
#    menu_default: false
#    kernel: images/Ubuntu/12.04/install/netboot/ubuntu-installer/amd64/linux
#    append: 'auto=true priority=critical vga=788 initrd=tftp://{{ tftp_bind_address }}/images/Ubuntu/12.04/install/netboot/ubuntu-installer/amd64/initrd.gz locale=en_US.UTF-8 kbd-chooser/method=us netcfg/choose_interface=auto url=tftp://{{ tftp_bind_address }}/preseed.cfg'
#  - label: 'Ubuntu 14.04.2 (Manual)'
#    menu_label: 'Install Ubuntu 14.04.2 (Manual)'
#    menu_default: false
#    kernel: images/Ubuntu/14.04/install/netboot/ubuntu-installer/amd64/linux
#    append: 'auto=true priority=critical vga=788 initrd=tftp://{{ tftp_bind_address }}/images/Ubuntu/14.04/install/netboot/ubuntu-installer/amd64/initrd.gz locale=en_US.UTF-8 kbd-chooser/method=us netcfg/choose_interface=auto'
#  - label: 'Ubuntu 14.04.2 (Pre-Seed)'
#    menu_label: 'Install Ubuntu 14.04.2 (Pre-Seed)'
#    menu_default: false
#    kernel: images/Ubuntu/14.04/install/netboot/ubuntu-installer/amd64/linux
#    append: 'auto=true priority=critical vga=788 initrd=tftp://{{ tftp_bind_address }}/images/Ubuntu/14.04/install/netboot/ubuntu-installer/amd64/initrd.gz locale=en_US.UTF-8 kbd-chooser/method=us netcfg/choose_interface=auto url=tftp://{{ tftp_bind_address }}/preseed.cfg'
tftp_build_images: false  #defines if images folder(s) and isos should be added
tftp_images_folders:
  - CentOS/7
  - ESXi/5.1
  - ESXi/5.1U1
  - ESXi/5.1U2
  - ESXi/5.5
  - ESXi/5.5U1
  - ESXi/5.5U2
  - ESXi/5.5U3
  - ESXi/6.0
  - Ubuntu/12.04
  - Ubuntu/14.04
tftpboot_dir: /var/lib/tftpboot
tftp_iso_images: []
#  - url: http://www.gtlib.gatech.edu/pub/centos/7/isos/x86_64
#    file: CentOS-7-x86_64-Minimal-1503-01.iso
#    folder: CentOS/7
#  - url: http://mirror.pnl.gov/releases/12.04
#    file: ubuntu-12.04.5-server-amd64.iso
#    folder: Ubuntu/12.04
#  - url: http://mirror.pnl.gov/releases/14.04
#    file: ubuntu-14.04.2-server-amd64.iso
#    folder: Ubuntu/14.04
````

Dependencies
------------

````
- { role: mrlesmithjr.apache2 }
- { role: mrlesmithjr.apt-cacher-ng, when: enable_apt_caching is defined and enable_apt_caching }
- { role: mrlesmithjr.config-interfaces, when: config_interfaces is defined and config_interfaces }
- { role: mrlesmithjr.dnsmasq }
- { role: mrlesmithjr.isc-dhcp, when: enable_dhcp is defined and enable_dhcp }
````

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: tftpserver
      roles:
         - { role: mrlesmithjr.tftpserver }

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com
