# Regle d'export PEER

    ip prefix-list OWN_PREFIX permit 14.0.0.0/8

    route-map OWN_PERMIT_FILTER permit 1
    match ip address prefix-list OWN_PREFIX
    exit

    route-map PERMIT_FILTER permit 5
    match local-preference 30 
    exit

    route-map DENY_FILTER deny 10
    match local-preference 10 
    exit

    router bgp 14
    neighbor 179.0.30.1 route-map PERMIT_FILTER out
    neighbor 179.0.30.1 route-map OWN_PERMIT_FILTER out
    neighbor 179.0.30.1 route-map DENY_FILTER out

    route-map MAP_IN permit 25
    set local-preference 20
    exit

    router bgp 14
    neighbor 179.0.30.1 route-map MAP_IN in
    exit
