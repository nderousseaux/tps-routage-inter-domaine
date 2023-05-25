# Regle d'export Costumer 

## CHAR

    ip prefix-list OWN_PREFIX permit 14.0.0.0/8

    route-map OWN_PERMIT_FILTER permit 1
    match ip address prefix-list OWN_PREFIX
    exit

    route-map PERMIT_FILTER permit 5
    match local-preference 20 
    exit

    route-map PERMIT_FILTER_2 permit 10
    match local-preference 10 
    exit

    route-map DENY_FILTER deny 20
    match local-preference 30 
    exit

    router bgp 14
    neighbor 179.0.32.2 route-map OWN_PERMIT_FILTER out
    neighbor 179.0.32.2 route-map PERMIT_FILTER out
    neighbor 179.0.32.2 route-map PERMIT_FILTER_2 out
    neighbor 179.0.32.2 route-map DENY_FILTER out

    route-map MAP_IN permit 100
    set local-preference 30
    exit

    router bgp 14
    neighbor 179.0.32.2 route-map MAP_IN in
    exit


## NASH 

    ip prefix-list OWN_PREFIX permit 14.0.0.0/8

    route-map OWN_PERMIT_FILTER permit 1
    match ip address prefix-list OWN_PREFIX
    exit

    route-map PERMIT_FILTER permit 5
    match local-preference 20 
    exit

    route-map PERMIT_FILTER_2 permit 10
    match local-preference 10 
    exit

    route-map DENY_FILTER deny 20
    match local-preference 30 
    exit

    router bgp 14
    neighbor 179.0.31.2 route-map OWN_PERMIT_FILTER out
    neighbor 179.0.31.2 route-map PERMIT_FILTER out
    neighbor 179.0.31.2 route-map PERMIT_FILTER_2 out
    neighbor 179.0.31.2 route-map DENY_FILTER out

    route-map MAP_IN permit 100
    set local-preference 30
    exit

    router bgp 14
    neighbor 179.0.31.2 route-map MAP_IN in
    exit


