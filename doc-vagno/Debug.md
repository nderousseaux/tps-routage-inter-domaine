# Commande pour connaitre les config

## Connaitre les routes BGP 
    router# show ip bgp summary
    router# sh ip bgp
    ou
    looking glass

## Reset une session BGP 
    router# clear ip bgp *

## Connaitre la config général
    router# sh run 

# Voir les interfaces
    router# show interface brief

# Voir les voisins OSPF
    router# show ip ospf neighbor

# MPLS


## Voir les voisins mpls

    show mpls ldp neighbor


## Les interfaces avec LDP 

    show mpls ldp interface

## Voir les lien dest-label 

    show ip route

## Voir MPLS table (LFIB)

    show mpls table

## Check the Label Information Base (LIB) 

    show mpls ldp ipv4 binding
