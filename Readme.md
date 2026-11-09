Born2beRoot - Project Documentation
1. Introduction

Born2beRoot is an introductory project to system administration and virtualization. The goal of this project is to set up your first secure virtual machine following strict security rules, utilizing LVM partitioning, a firewall, a custom password policy, and a system monitoring script.
2. Virtual Machine & Hypervisor

    Hypervisor: VMware (or VirtualBox, depending on what you used).

    Operating System: Debian 12 (Bookworm) / Debian 11.

    Architecture: 64-bit.

3. Partitioning & LVM (Logical Volume Manager)

The partitioning scheme strictly follows the project requirements to use LVM with at least two primary partitions (/boot and encrypted/unencrypted root LV):

    root (/)

    swap

    home

    var

    srv

    tmp

    var-log

4. Security & User Management
Password Policy

To ensure strong passwords, the system is configured via /etc/login.defs and libpam-pwquality with the following rules:

    Expiration every 30 days.

    Minimum of 7 days before modification.

    Warning given 7 days prior to expiration.

    Minimum length of 10 characters.

    Must contain at least 1 uppercase, 1 lowercase, and 1 numeric character.

    Cannot contain more than 3 consecutive identical characters.

    Cannot contain the user's login name.

    Must differ by at least 3 characters from the old password.

Sudo Rule

    Sudo is strictly configured with limitations:

        Restricting input/output (TTY enforcement).

        Limiting the path to secure binaries.

        Forcing logging of all executed commands to /var/log/sudo/sudo.log.

    Users configured: Root and a standard user belonging to the sudo and user42 groups.

5. Firewall & Network (UFW & SSH)

    UFW (Uncomplicated Firewall): Enabled and configured to deny all incoming traffic by default and allow only specified ports.

    SSH Service: Configured to run strictly on Port 4242, and root login via SSH is disabled.

6. Monitoring Script (monitoring.sh)

Located at /usr/local/bin/monitoring.sh, this script gathers vital system metrics and broadcasts them to all terminals using wall every 10 minutes via cron.
Metrics Tracked:

    Architecture: OS version and kernel release.

    CPU Physical: Number of physical CPU cores.

    vCPU: Number of virtual CPUs.

    Memory Usage: Current RAM usage and utilization percentage.

    Disk Usage: Total and used disk space with percentage.

    CPU Load: Current CPU utilization rate.

    Last Boot: Date and time of the last system reboot.

    LVM Use: Status (yes or no) indicating if LVM is active.

    Connections TCP: Number of active established TCP connections.

    User Log: Number of currently logged-in users.

    Network: IP address and MAC address.

    Sudo: Number of commands executed via sudo.

7. Signature (Hash)

    Signature File: signature.txt

    Command used to generate:
    Bash

    shasum /path/to/virtual/disk.vdi > signature.txt
