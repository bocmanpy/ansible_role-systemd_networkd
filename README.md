Ansible role: systemd\_networkd
===============================

Ansible role to configure [systemd-networkd](https://www.freedesktop.org/software/systemd/man/latest/systemd.directives.html#Network%20directives).

Variables
----
```yaml
# Cleanup all unmanaged interface files
# default is set to false
systemd_networkd_cleanup: true
# Restart systemd on change
# default is set to false
systemd_networkd_restart: false
# User/Group for systemd files and directory
systemd_networkd_user: "systemd"
# Path to networkd configuration files
# default is set to /etc/systemd/network
systemd_networkd_directory: "/etc/systemd/network"
systemd_networkd:
  interfaces:
# Interface describe variable
    eth0:
# File type: *.link
      link:
# Section: [Link]
        Link:
# Config inside section
          MACAddressPolicy: 'persistent'
        network:
         Network:
        Bridge: 'br0'
    br0:
      netdev:
        NetDev:
          Name: 'br0'
          Kind: 'bridge'
          MACAddress: 'none'
        Bond:
          FailOverMACPolicy: 'active'
      link:
        Link:
          MACAddressPolicy: 'none'
      network:
        Link:
          RequiredForOnline: 'routable'
        Network:
          DHCP: 'yes'
```
----
## Default Match patten
If `[Match]` section doesn't describe inside interface variables,
by default it will fill it with next pattern:
```ini
[Match]
Name=$name_of_interface
```
Exeption for `systemd.netdev`, because `Name` option [not supported by this section](https://www.freedesktop.org/software/systemd/man/latest/systemd.netdev.html).