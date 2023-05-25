# Regle d'export PEER

    ip prefix-list PREFIX_AS11 permit 179.0.10.0/24
    ip prefix-list PREFIX_AS12 permit 179.0.31.0/24

    ip prefix-list PREFIX_AS15 permit 179.0.32.0/24
    ip prefix-list PREFIX_AS16 permit 179.0.31.0/24


    ip prefix-list OWN_PREFIX permit 14.0.0.0/8

    route-map PERMIT_FILTER permit 10 
    match ip address prefix-list PREFIX_AS15
    match ip address prefix-list PREFIX_AS16
    match ip address prefix-list OWN_PREFIX
    exit

    route-map PERMIT_FILTER permit 10 
    match ip address prefix-list PREFIX_AS15
    match ip address prefix-list PREFIX_AS16
    exit

    route-map DENY_FILTER deny 10 
    match ip address prefix-list PREFIX_AS11
    match ip address prefix-list PREFIX_AS12
    exit

    router bgp 14
    neighbor 179.0.30.1 route-map PERMIT_FILTER out
    neighbor 179.0.30.1 route-map DENY_FILTER out
