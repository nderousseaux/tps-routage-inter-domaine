# Question MPLS 

[Lien](https://gitlab.unistra.fr/jr.luttringer/routage-inter-mini-internet/-/wikis/1.-Assignment/1.3.2-Questions-MPLS)

# LDP

Reminder: Always prefer to launch traceroute from the hosts because they can use the DNS service (routers cannot).

(On ne se concentre que sur le tunnel DETR - PITT - CHAR)

# Question 1.1 

## Changer les ip de lo en /32

`DETR` (la même pour PITT et CHAR)

    DETR_router# conf t
    DETR_router(config)# interface lo
    DETR_router(config-if)# no ip address 14.155.0.1/24
    DETR_router(config-if)# ip address 14.155.0.1/32
    DETR_router(config-if)# exit
    DETR_router(config)# router ospf
    DETR_router(config-router)# network 14.155.0.1/32 area 0
    DETR_router(config-router)# exit

## On active LDP

`DETR`

    DETR_router# conf t
    DETR_router(config)# mpls ldp
    DETR_router(config-ldp)# router-id 14.155.0.1

PITT 14.154.0.1/32

## discovery transport-address

    DETR_router(config)# mpls ldp
    DETR_router(config-ldp)# address-family ipv4
    DETR_router(config-ldp-af)# discovery transport-address 14.155.0.1

PITT 14.154.0.1/32

## Activer MPLS pour les interfaces necessaires

DETR_router# conf t
DETR_router(config)# mpls ldp
DETR_router(config-ldp)# address-family ipv4
DETR_router(config-ldp-af)# interface port_PITT 

# Question 1.2 

Reminder: The `hostnames` follow a specific naming scheme.  
As an example, tracerouting to the host linked to the PITT router within AS 10 would be done through traceroute `host-PITT.group10`. Tracerouting the PITT interface of the PITT-DETR link can be done through traceroute `PITT-DETR.group10`

## The measurement container
For example, 
the operator of AS8 would launch a traceroute from one of its providers, AS6, using the command 

    ./launch_traceroute.sh 6 CHAR-host.group8


*As Tier 1 ASes do not have measurement containers, ASes having them as providers should instead traceroute from NASH to BROO*

