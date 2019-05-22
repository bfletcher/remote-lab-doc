# Setting Up A Remote Lab

## What are remote labs

LAVA evolution allowing CI with a single dashboard server and distributed board farms typically in vendor premises. 
We see the following benefits:
 * DUT management in the hands of originators/owners 
 * Don’t need to educate central lab personnel on every board from every vendor
 * Minimise hardware shipping/return cycle with problematic board debug
 * Can reach “consortium scale” with no single bottleneck/resource sink

## About this guide

A document outlining the requirements, and process needed to deploy a remote lab that will connect to Linaro's master LAVA instance. Ensure the deployment of remote LKFT dispatcher is fast, efficient, and correct to minimize impact on the Linaro's resources. Various Linaro Members are working toward setting up a remote lab that will connect to Linaro's LKFT master LAVA server.

## Reasons to set up a remote lab

Working within an established project but looking to extend coverage in terms of number of a particular supported device. Established projects can be one of LKFT, Trusted Firmware, ...

## Not reasons to set up a remote lab
AKA "out of scope".
* Working with devices-under-test (DUTs) that are not supported upstream in LAVA. 
* Using an installation as a testbed to upstream integration support for new devices.

## Assumptions

## Infrastructure requirements

* There are minimum requirements (CPU, memory, I/O robustness) for the hardware that hosts the Worker instance
* DUT interface hardware (USB, power control) needs to be robust
* For a many-Worker Remote Lab, scalable administration, deployment & configuration tools should be used

## Set Up Tasks

### Other Documentation

The LAVA documentation [1] provides documentation which covers installing and configuring LAVA components. 

### Proving the Connection

Whilst the LAVA dispatcher's communication with the central server is based on standard protocols, and all communication originates at the dispatcher, It's not possible to guarantee that a remote LAVA dispatcher will be able to communicate with the central server in all possible situations. This can be because of corporate or other firewalls and other infrastructure constraints.
In order to test the connection, Linaro will supply a containerised test dispatcher which should be run at the remote site to prove the connection capability.
If the connection test is not successful, it will be necessary either to debug the corporate firewall infrastructure, or, more likely, install a dedicated external connection for the remote lab.


### Installing lava-dispatcher

### Connecting the dispatcher

## Maintenance Tasks

### SLA/Point-of-Contact/Support

### Upgrades

## Troubleshooting

### Downgrades

## References

[1] LAVA documentation *what's the official site?*
