```bash
cd Downloads
wget http://prdownloads.sourceforge.net/webadmin/webmin-1.870.tar.gz
tar -xvf webmin-1.870.tar.gz
cd webmin-1.870/
sudo ./setup.sh /usr/local/webmin

[sudo] password for wisoft: wisoft123

Config file directory [/etc/webmin]: /etc/webmin
Log file directory [/var/webmin]: /var/webmin
***********************************************************************
Webmin is written entirely in Perl. Please enter the full path to the
Perl 5 interpreter on your system.

Full path to perl (default /usr/bin/perl): /usr/bin/perl

***********************************************************************
Operating system name:    Ubuntu Linux
Operating system version: 16.04.3

***********************************************************************
Webmin uses its own password protected web server to provide access
to the administration programs. The setup script needs to know :
 - What port to run the web server on. There must not be another
   web server already using this port.
 - The login name required to access the web server.
 - The password required to access the web server.
 - If the webserver should use SSL (if your system supports it).
 - Whether to start webmin at boot time.

Web server port (default 10000): 10000
Login name (default admin): admin
Login password:
Password again:
Use SSL (y/n): n
Start Webmin at boot time (y/n): y

```

```bash
sudo apt install git -y 
sudo git clone https://github.com/openstack-dev/devstack /opt/devstack
cd /opt/devstack
sudo chmod u+x tools/create-stack-user.sh
sudo tools/create-stack-user.sh
	Creating a group called stack
	Creating a user called stack
	Giving stack user passwordless sudo privileges
sudo chown -R stack:stack /opt/devstack
sudo -i -u stack
cd /opt/devstack
``` 


```bash
stack@wisoft-VirtualBox:/opt/devstack$ vim local.conf

[[local|localrc]]

# Credentials
ADMIN_PASSWORD=devstack
MYSQL_PASSWORD=devstack
RABBIT_PASSWORD=devstack
SERVICE_PASSWORD=devstack
SERVICE_TOKEN=token

#Enable/Disable Service
disable_service n-net
enable_service q-svc
enable_service q-agt
enable_service q-dhcp
enable_service q-l3
enable_service q-meta
enable_service neutron
enable_service tempest
HOST_IP= <your virtual ip>

#NEUTRON CONFIG
#Q_USE_DEBUG_COMMAND=True

#CINDER CONFIG
VOLUME_BACKING_FILE_SIZE=102400M

#GENERAL CONFIG
API_RATE_LIMIT=False

# Output
LOGFILE=/opt/stack/logs/stack.sh.log
VERBOSE=True
LOG_COLOR=False
SCREEN_LOGDIR=/opt/stack/logs
```

```bash
./stack.sh 
```

```bash 
=========================
DevStack Component Timing
 (times are in seconds)
=========================
run_process           23
test_with_retry        3
apt-get-update        10
pip_install          375
osc                  153
wait_for_service      20
git_timed            432
dbsync                30
apt-get              562
-------------------------
Unaccounted time     424
=========================
Total runtime        2032



This is your host IP address: 192.168.1.75
This is your host IPv6 address: ::1
Horizon is now available at http://192.168.1.75/dashboard
Keystone is serving at http://192.168.1.75/identity/
The default users are: admin and demo
The password: devstack

WARNING:
Using lib/neutron-legacy is deprecated, and it will be removed in the future


Services are running under systemd unit files.
For more information see:
https://docs.openstack.org/devstack/latest/systemd.html

DevStack Version: queens
Change: 5fb35b4f2bb072bd629e18fbc99522cd1ea73718 Merge
"Added the ability to specify checksum for etcd" 2018-01-26 12:47:48 +0000
OS Version: Ubuntu 16.04 xenial
```

```bash 
stack@wisoft-VirtualBox:/opt/devstack$ ./exercise.sh
=====================================================================
SKIP swift
FAILED aggregates
FAILED boot_from_volume
FAILED client-args
FAILED client-env
FAILED floating_ips
FAILED neutron-adv-test
FAILED sec_groups
FAILED volumes
```