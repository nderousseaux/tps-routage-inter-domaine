# Config

    ip prefix-list OWN_PREFIX permit 14.0.0.0/8
    bgp community-list 1 seq 5 permit 14:10

    route-map OWN_PREFIX permit 10
        match ip address prefix-list OWN_PREFIX
        exit

    route-map LOCAL_PREF_OUT_11 permit 5
        match ip address prefix-list OWN_PREFIX
        exit

    route-map LOCAL_PREF_OUT_11 permit 10
        match community 1
        exit

    route-map LOCAL_PREF_IN_11 permit 10 
        set community 14:30
        set local-preference 20
        exit

    router bgp 14
        address-family ipv4 unicast
        network 14.0.0.0/8
        neighbor 179.0.10.1 route-map LOCAL_PREF_IN_11 in
        neighbor 179.0.10.1 route-map LOCAL_PREF_OUT_11 out
