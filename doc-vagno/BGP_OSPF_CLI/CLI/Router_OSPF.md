# Commandes en vrac 
    router# ?
        clear       Reset functions
        configure   Configuration from vty interface
        exit        Exit current mode and down to previous mode
        no          Negate a command or set its defaults
        ping        Send echo messages
        quit        Exit current mode and down to previous mode
        show        Show running system information
        traceroute  Trace route to destination
        write       Write running configuration to memory, network, or terminal

# Conf router interface 
    router# conf t
    router(config)# interface INTERFACENAME
    router(config-if)# ip address 1.0.0.1/24

    router# show interface brief

Each router has interfaces to its neighbouring routers whose names
follow the pattern port_<neighbor>.
For instance, the interface on NEWY connected to PITT is named port_PITT.
Moreover, each router has an interface connected to the host
named host and a loopback interface called lo.
An interface connecting to another AS is called ext_<AS-number>_<router-name>. For example,
the interface on CHIC connecting to NASH in AS 82 has the name ext_82_NASH.



# Activer OSPF 

    router# conf t
    router(config)# router ospf
    router(config)# network 1.0.0.0/24 area 0
    router(config)# network 2.0.0.0/24 area 0

## Conf le poid

    router# conf t
    router(config)# interface INTERFACENAME
    router(config-if)# ip ospf cost 900

Voir la conf OSPF

    router# show ip ospf neighbor

    router# show interface brief

# debuggage 

L'id du demon ospf devient l'address de loopback

    router ospf
    ospf router-id ip-lo

    router# show ip ospf neighbor


# exemple 

## conf ospf DETR -> BROO 
    router# conf t
    router(config)# interface portBROO
    router(config-if)# ip address 14.0.2.2/24
    
    router(config)# router ospf
    router(config-if)# network 14.0.2.2/24 area 0

(Oublie pas l'hôte `host` et le loopback `lo` à chaque fois)