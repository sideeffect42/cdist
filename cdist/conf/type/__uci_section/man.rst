cdist-type__uci_section(7)
==========================

NAME
----
cdist-type__uci_section - Manage configuration sections in UCI


DESCRIPTION
-----------
This cdist type can be used to replace whole configuration sections in OpenWrt's
Unified Configuration Interface (UCI) system.
It can be thought of as syntactic sugar for :strong:`cdist-type__uci`\ (7),
as this type will generate the required `__uci` objects to make the section
contain exactly the options specified using ``--option``.

Since many default UCI sections are unnamed, this type allows to find the
matching section by one of its options using the ``--match`` parameter.

**NOTE:** Options already present on the target and not listed in ``--option``
or ``--list`` will be deleted.


REQUIRED PARAMETERS
-------------------
None.


OPTIONAL PARAMETERS
-------------------
list
    An option that is part of a list and should be present in the section (as
    part of a list).  Lists with multiple options can be expressed by using the
    same ``<option>`` repeatedly.

    The value to this parameter is a ``<option>=<value>`` string.
match
    Allows to find a section to "replace" through one of its parameters.

    The value to this parameter is a ``<option>=<value>`` string.
option
    An option that should be present in the section.
    This parameter can be used multiple times to specify multiple options.

    The value to this parameter is a ``<option>=<value>`` string.
state
    ``present`` or ``absent``, defaults to ``present``.
transaction
    The name of the transaction this option belongs to.
    The value will be forwarded to :strong:`cdist-type__uci`\ (7).
type
    The type of the section in the format: ``<config>.<section-type>``


BOOLEAN PARAMETERS
------------------
None.


EXAMPLES
--------

.. code-block:: sh

    # Configure the dropbear daemon
    __uci_section dropbear --type dropbear.dropbear --transaction sshd \
        --match Port=22 --option Port=22 \
        --option PasswordAuth=off \
        --option RootPasswordAuth=off

    # Block SSH access from the guest network
    __uci_section firewall.block_ssh_from_guest --type firewall.rule \
        --transaction fwrules \
        --option name='Block-SSH-Access-from-Guest' \
        --option src='guest' \
        --option proto='tcp' \
        --option dest_port='22' \
        --option target='REJECT'

    # Configure a Wi-Fi access point
    __uci_section wireless.default_radio0 --type wireless.wifi-iface \
        --transaction wifi \
        --option device='radio0' \
        --option mode='ap' \
        --option network='wlan' \
        --option ssid='mywifi' \
        --option encryption="psk2' \
        --option key='hunter2'


SEE ALSO
--------
- https://openwrt.org/docs/guide-user/base-system/uci
- :strong:`cdist-type__uci`\ (7)


AUTHORS
-------
Dennis Camera <dennis.camera@ssrq-sds-fds.ch>


COPYING
-------
Copyright \(C) 2020 Dennis Camera. You can redistribute it
and/or modify it under the terms of the GNU General Public License as
published by the Free Software Foundation, either version 3 of the
License, or (at your option) any later version.
