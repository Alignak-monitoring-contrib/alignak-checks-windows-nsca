Alignak checks package for Windows WMI
======================================

Checks pack for monitoring hosts with Windows Management Instrumentation (WMI)


Installation
------------

From PyPI
~~~~~~~~~
To install the package from PyPI:
::
   pip install alignak-checks-wmi


From source files
~~~~~~~~~~~~~~~~~
To install the package from the source files:
::
   git clone https://github.com/Alignak-monitoring-contrib/alignak-checks-wmi
   cd alignak-checks-wmi
   sudo python setup.py install


Documentation
-------------

Configuration
~~~~~~~~~~~~~
Edit the */usr/local/etc/alignak/arbiter/packs/wmi/resources.cfg* file and configure the domain name, user name and password allowed to access remotely to the monitored hosts WMI.
::
   #-- Active Directory for WMI
   # Replace MYDOMAIN with your domain name or . for local user account
   $DOMAIN$=MYDOMAIN
   # Replace MYUSER with the WMI authorized user (domain or local user account)
   $DOMAINUSERSHORT$=MYUSER
   $DOMAINUSER$=$DOMAIN$\\$DOMAINUSERSHORT$
   # Replace MYPASSWORD with the WMI authorized user password
   $DOMAINPASSWORD$=MYPASSWORD

Prepare Windows host
~~~~~~~~~~~~~~~~~~~~
Some operations are necessary on the Windows monitored hosts if WMI remote access is not yet activated.

Create a user account:

- username/password (example): alignak/alignak
- member of following groups: Administrators, Remote DCOM users
- Deactivate interactive login permissions (more secure)

Check that WMI and RPC services are started

The Windows Firewall must allow inbound trafic for:
   - Windows Firewall Remote Management (RPC)
   - Windows Management Instrumentation (DCOM-In)
   - Windows Management Instrumentation (WMI-In)

This page contains more information about remote WMI configuration: https://kb.op5.com/display/HOWTOs/Agentless+Monitoring+of+Windows+using+WMI

Test remote WMI access with the plugins files:
::
   # Basic wmic command ...
   $ /usr/local/var/libexec/alignak/wmic -U .\\alignak%alignak //192.168.0.20 'Select Caption From Win32_OperatingSystem'

   # Alignak plugin command ...
   $ /usr/local/var/libexe/alignak/check_wmi_plus.pl -H 192.168.0.20 -u ".\\alignak" -p "alignak" -m checkdrivesize -a '.'  -w 90 -c 95 -o 0 -3 1  --inidir=/usr/local/var/libexec/alignak


Alignak configuration
~~~~~~~~~~~~~~~~~~~~~

You simply have to tag the concerned hosts with the template `windows-nsca-host`.
::

    define host{
        use                     windows-nsca-host
        host_name               windows_nsca_host
        address                 0.0.0.0
    }


Bugs, issues and contributing
-----------------------------

Contributions to this project are welcome and encouraged ... issues in the project repository are the common way to raise an information.

License
-------

Alignak Pack Checks WMI is available under the `GPL version 3 license`_.

.. _GPL version 3 license: http://opensource.org/licenses/GPL-3.0