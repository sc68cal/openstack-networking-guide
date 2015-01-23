# Scenario: Bare pipes with Provider Networking

This scenario describes a basic implementation of OpenStack
Networking, where the networking service integrates with legacy
networks and physical hardware. The cases in which the provider
networking extension would be required consist of, but are not limited
to the following:

* Deploying a new cloud in a mixed environment

* High performance and reliability

* Simplicity


## Deploying a new cloud in a mixed environment

In some cases, an OpenStack deployment will be installed and networked
within a datacenter that already has a sizable network infrastructure
deployment, and the applications that run on top of the OpenStack
deployment may still need to interface with physical machines on the
same L2 network segment.


## High Performance and reliability

Prior to the introduction of the Distributed Virtual Router feature in
OpenStack Networking, it was difficult to ensure the reliability of a
OpenStack Networking deployment since all traffic passed through a
dedicated network node. HA options did exist, but they were more
complex than relying on physical hardware.

Phsyical hardware switches and routers would also have better network performance
characteristics compared to most of the general purpose Linux machines
that OpenStack Networking would be installed on, which would also
contribute to the decision to use the provider networking extension.

## Simplicity

In many cases, operators are already familiar with the network
architecture of physical switches and routers. In many cases, it will
be faster to deploy the Network service, utilize the provider
networking extension to map physical networks to the cloud, then
slowly learn more about the Networking and how to operate it, and
design new architectures that can utilize the L3 features of the
Networking service in a future deployment.

# Architecture

The network architecture for a provider networking from the OpenStack
perspective is fairly simple since the OpenStack cluster is being
"plugged in" to a provisioned and configured network that includes L2
and L3 connectivity. In a provider VLAN configuration, the hardware
switch that the OpenStack cluster is connected to already has
provisioned VLANs for the management/API network and public
Internet.


# Packet Flow

The flow of packets in a provider network scenario only contains
complexity inside the compute node's OVS networking. Neutron allocates
internal VLAN tags for each Neutron Network and provides a mapping
between the internal VLAN tag used for a Neutron network, and then
inserts rules in the Open vSwitch switching infrastructure to rewrite
the internal VLAN tag back to the VLAN tag that is allocated on the
hardware switch, as packets cross the br-ex device.
