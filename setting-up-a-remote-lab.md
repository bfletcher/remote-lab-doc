# Setting Up A Remote Lab

## What are remote labs?

Remote labs are a LAVA development allowing CI with a single centralised dashboard server and multiple distributed board farms some of which can be in hardware vendor or other partner premises. 
Remote labs provide the following benefits:
 * DUT management in the hands of originators/owners 
 * No need to educate the central lab personnel on every board from every vendor
 * Minimise hardware shipping/return cycle with problematic board debug
 * Can reach “consortium scale” with no single bottleneck/resource sink

## About this guide

This document outlines the requirements and process needed to deploy a remote lab that will connect to Linaro's master LAVA instance. 

## Reasons to set up a remote lab

Remote labs are intended to support partners who are working within an established project but looking to extend coverage e.g. in terms of the number of a particular supported device. Established projects can be one of LKFT, Trusted Firmware, ... . For example, various Linaro Members are working toward setting up a remote lab that will connect to Linaro's LKFT master LAVA server.

## Out of scope for remote labs
Remote labs are an efficient way to scale board farms. They are not intended as a way to develop device or new feature support. The following are contra-indications for working as a remote lab:
* Using a custom version of LAVA
* Working with devices-under-test (DUTs) that are not supported upstream in LAVA
* Using an installation as a testbed to upstream integration support for new devices

## Assumptions

* Native install of LAVA components on local hardware running Debian Stretch
* There is a supported communication path between the remote lab and the central server (see below for test)
* 

## Infrastructure requirements

*(needs more of the wisdom of infrastructure from Lab folks or LAVA docs)*
* There are minimum requirements (CPU, memory, I/O robustness) for the hardware that hosts the Worker instance
* DUT interface hardware (USB, power control) needs to be robust
* All infrastructure control should be via scripts invoked in device dictionary entries rather than any temptation towards dispatcher hard coding 
* For a many-Worker Remote Lab, scalable administration, deployment & configuration tools should be used

## Set Up Tasks

### Other Documentation

The LAVA documentation [1] also provides documentation which covers installing and configuring LAVA components. 

### Proving the Connection

Whilst the LAVA dispatcher's communication with the central server is based on standard protocols, and all communication originates at the dispatcher, It's not possible to guarantee that a remote LAVA dispatcher will be able to communicate with the central server in all possible situations. This can be because of corporate or other firewalls and other infrastructure constraints.
In order to test the connection, Linaro will supply a containerised test dispatcher which should be run at the remote site to prove the connection capability.
If the connection test is not successful, it will be necessary either to debug the corporate firewall infrastructure, or, more likely, install a dedicated external connection for the remote lab.

### Installing lava-dispatcher

This installation assumes a native install on Debian Stretch

```
# Activate the backports
apt-get update
apt-get install --no-install-recommends --yes wget gnupg ca-certificates apt-transport-https
echo "deb https://deb.debian.org/debian stretch-backports main" > /etc/apt/sources.list.d/backports.list

# Add the lava debian repository
wget https://apt.lavasoftware.org/lavasoftware.key.asc
apt-key add lavasoftware.key.asc
echo "deb http://apt.lavasoftware.org/release stretch-backports main" > /etc/apt/sources.list.d/lava.list

# Install lava-dispatcher
apt-get update
apt-get install -t stretch-backports lava-dispatcher

```

### Connecting the dispatcher

Connecting the remote dispatcher to the central server is the final step and key piece of setting up a remote lab. The dispatcher which is to be connected can be installed stand-alone as in the previous section, or it can be part of a full LAVA install. 
It is likely that some of the steps below will be scripted or supported in terms of Linaro supplying a complete lava-slave configuration file. However, because Linaro does not have admin access to the remote lab dispatcher, the actual configuration will need to be actioned by remote lab staff. 

#### Connection steps

1. *(Do we need to stop the lava-slave service first?)
1. Create an encryption certificate:
/usr/share/lava-dispatcher/create_certificate.py <dispatcher-name>
1. Send the public key (to LAB admin
/etc/lava-dispatcher/certificates.d/<dispatcher-name>.key
1. Install the master certificate:
cp master.key /etc/lava-dispatcher/certificates.d/master.key
1. Configure the slave by editing /etc/lava-dispatcher/lava-slave and uncommenting the variables you want to define:
MASTER_URL=<master_url>
LOGGER_URL=<logger_url>
ENCRYPT="--encrypt"
MASTER_CERT="--master-cert /etc/lava-dispatcher/certificates.d/master.key"
SLAVE_CERT=--slave-cert /etc/lava-dispatcher/certificates.d/<dispatcher-name>.key_secret
HOSTNAME="--hostname <dispatcher-name>"
1. Start lava-slave service
service lava-slave start
1. Watch the logs in /var/log/lava-dispatcher/lava-slave.log
1. Check that the worker is online in the lava web interface.

## Maintenance Tasks

## Upgrades

An important remote lab task is the management of LAVA upgrades.  

### SLA/Point-of-Contact/Support

A mailing list has been created specifically for remote lab users to distribute information and for posting support questions. 
*(needs details on how to join the mailing list)*

### Upgrades

## Troubleshooting

### Downgrades

## References

[1] LAVA documentation *what's the official site?*
