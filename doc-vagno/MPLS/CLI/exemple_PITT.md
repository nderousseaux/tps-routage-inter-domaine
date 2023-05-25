# PITT LDP

    PITT_router# conf t 
    PITT_router(config)# mpls ldp 
    PITT_router(config-ldp)# address-family ipv4
    PITT_router(config-ldp-af)# discovery transport-address 14.154.0.1
    PITT_router(config-ldp-af)# interface port_DETR 
    PITT_router(config-ldp-af-if)# exit
    PITT_router(config-ldp-af)# interface port_CHAR 

CHAR 14.153.0.1