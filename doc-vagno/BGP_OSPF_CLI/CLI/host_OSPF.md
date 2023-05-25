# Configurer un hôte OSPF
## Conf host interface

The interface to the router is called
<router-name>router, e.g. `PITTrouter` for PITT. The loopback interface
has the name `lo`

## Liste des interfaces 
    ip address show

## Assigner une addresse IP à une interface.
    ip address add 14.105.0.1/24 dev BROOrouter

## Ajouter une route par defaut 
    ip route add default via IP_ADDRESS

## Pour la voir 
    netstat -rn

## Pour la supp 
    ip route del default via IP_ADDRESS

