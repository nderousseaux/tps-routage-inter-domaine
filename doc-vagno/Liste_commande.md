# Commande BGP policies

    CHAR_router# conf t
    CHAR_router(config)# route-map MAP-IN permit 100 
    CHAR_router(config-route-map)# set community 14:30
    CHAR_router(config-route-map)# set local-preference 30
    CHAR_router(config-route-map)# exit
    CHAR_router(config)# router bgp 14
    CHAR_router(config-router)# neighbor 179.0.32.2 route-map MAP-IN in 
    CHAR_router(config-router)# exit 
    CHAR_router(config)# exit

# DETER filter        
    DETR_router(config)# ip prefix-list PREFIX_AS13 permit 179.0.30.0/24
                          
    DETR_router(config)# ip prefix-list PREFIX_AS12 permit 179.0.11.0/24
    DETR_router(config)# route-map DENY_FILTER deny 10 
    DETR_router(config-route-map)# match ip address prefix-list PREFIX_AS12
    DETR_router(config-route-map)# match ip address prefix-list PREFIX_AS13
    DETR_router(config-route-map)# exit
    DETR_router(config)# router bgp 14
    DETR_router(config-router)# neighbor 179.0.10.1 route-map DENY_FILTER out