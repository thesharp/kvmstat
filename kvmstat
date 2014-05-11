#!/usr/bin/python

import libvirt
import sys
import os
from optparse import OptionParser


def main():
    if not os.geteuid() == 0:
        sys.exit("This program can only be run under root")

    parser = OptionParser()
    parser.add_option("-a", "--all", action="store_true", dest="show_inactive", default=False,
                      help="show all guests, including inactive")
    (options, args) = parser.parse_args()

    conn = libvirt.openReadOnly(None)
    if conn is None:
        sys.exit("Failed to connect to the hypervisor")

    node_info = conn.getInfo()
    node_ram = node_info[1]
    node_cpu = node_info[2]

    used_cpu = 0
    used_ram = 0

    # To show both active and inactive guests, we need to get the list of names.
    active_domains_id = conn.listDomainsID()
    inactive_domains_names = conn.listDefinedDomains()
    active_domains_names = []

    for guest in active_domains_id:
        domain = conn.lookupByID(guest)
        name = domain.name()
        active_domains_names.append(name)

    all_domains = active_domains_names + inactive_domains_names

    print "%-5s %-12s %8s %2s %6s %15s" % ("ID", "NAME", "RUNNING", "VCPU", "RAM", "AUTOSTART")

    for guest in all_domains:
        domain = conn.lookupByName(guest)
        autostart = bool(domain.autostart())
        _id = domain.ID()
        active = bool(domain.isActive())
        info = domain.info()
        cpu = info[3]
        if not active and not options.show_inactive:
            pass
        else:
            used_cpu += cpu
        ram = int(info[2]) / 1024
        if not active and not options.show_inactive:
            pass
        else:
            used_ram += ram

        if not active and not options.show_inactive:
            pass
        else:
            print "%-5d %-12s %8s %2d %10d MB %8s" % (_id, guest, active, cpu, ram, autostart)

    if used_cpu > node_cpu:
        overcommitment = used_cpu - node_cpu
        node_cpu = "%d (overcommitment: %d)" % (node_cpu, overcommitment)
    if used_ram > node_ram:
        overcommitment = used_ram - node_ram
        node_ram = "%d (overcommitment: %d MB)" % (node_ram, overcommitment)

    print "\nAllocated VCPUs: %d / %s" % (used_cpu, node_cpu)
    print "Allocated RAM: %d / %s" % (used_ram, node_ram)


if __name__ == '__main__':
    main()
