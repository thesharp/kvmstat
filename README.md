# kvmstat

## Description
**kvmstat** shows you neat usage report of your KVM guests.

## Sample output
    ID    NAME          RUNNING VCPU    RAM       AUTOSTART
    1     xxxxxxxx         True  4       4096 MB     True
    2     xxxxxxxxxx       True  4      20480 MB     True
    3     xxxxx            True  4      17408 MB     True
    4     xxxxxxxxxx01     True  4       6144 MB     True
    5     xxxxxxxxxx02     True  4       6144 MB     True
    6     xxxxxx           True  2       2048 MB     True
    7     xxxxxxxxxx00     True  4       6144 MB     True
    -1    xxx             False  4      10240 MB    False

    Allocated VCPUs: 30 / 24 (overcommitment: 6)
    Allocated RAM: 72704 / 64403 (overcommitment: 8301 MB)

## Installation on RHEL6 boxes
- [Enable Vortex RPM repository](http://vortex-rpm.org) (``yum install -y http://vortex-rpm.org/el6/noarch/vortex-release-6-1.vortex.el6.noarch.rpm``)
- ``yum install kvmstat``

## Dependencies
- Python 2.6
- libvirt-python
- libvirtd

## Usage
Simply run it. If you will pass ``-a`` as an argument, all guests (including stopped ones) will be shown.
