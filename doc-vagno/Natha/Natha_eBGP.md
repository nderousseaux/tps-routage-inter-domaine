## Question 2.2 - eBGP

Mettre en place eBPG avec les AS voisins (et les IXPs).

On configure l'addresse ip de l'interface, puis on lance une session BGP avec le router distant.

router# conf t
router(config)# interface INTERFACENAME
router(config-if)# ip address LOCAL_ADDRESS
router(config-if)# exit

router(config)# route-map ACCEPT_ALL permit 10
router(config-route-map)# exit

router(config)# router bgp AS_LOCAL
router(config-router)# neighbor ADDRESS_DEST remote-as AS_DIST
router(config-router)# neighbor ADDRESS_DEST route-map ACCEPT_ALL in
router(config-router)# neighbor ADDRESS_DEST route-map ACCEPT_ALL out
router(config-router)# neighbor ADDRESS_DEST update-source lo
router(config-router)# address-family ipv4 unicast
router(config-router)# network LOCALPREFIX
router(config-router)# neighbor ADDRESS_DEST next-hop-self


route-map : ??????

network : On indique quel est notre préfixe réseaux aux autres AS. (Juste avant on lance address-family ipv4 unicast pour déclarer le type d'adresses).