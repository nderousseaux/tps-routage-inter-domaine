# Liste des BDG community 

## DETR <> AS 11 BROO 
    prefix_list PREFIX_AS12 179.0.11.0/24 / PREFIX_AS13 179.0.30.0/24
    OWN_PREFIX permit 14.0.0.0/8

    DENY_FILTER PREFIX_AS12 PREFIX_AS13
    PERMIT_FILTER OWN_PREFIX PREFIX_AS16
    MAP-IN 14:100 localpref 10

## CHIC <> AS 12 BROO
    14:100 MAP-PROV localpref 10
    MAP_DENY_OUT pour ratio les provider 

## STLO <> AS 13
    MAP_IN community 14:20 localpref 20 

## Char <> AS 15 
    MAP_IN community 14:30 localpref 30

la même pour NASH

