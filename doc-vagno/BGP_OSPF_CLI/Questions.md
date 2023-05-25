# Réponses bêtes aux questions

# Question 1.1

## Conf

    router# conf t
    router(config)# interface portBROO
    router(config-if)# ip address 14.0.2.2/24

    router(config)# router ospf
    router(config-if)# network 14.0.2.2/24 area 0

## traceroute

    DETR_host:~# traceroute 14.0.5.1
    traceroute to 14.0.5.1 (14.0.5.1), 30 hops max, 46 byte packets
     1  DETR-host.group14 (14.105.0.2)  0.374 ms  0.005 ms  0.004 ms
     2  PITT-DETR.group14 (14.0.7.1)  0.164 ms  0.112 ms  0.113 ms
     3  CHAR-PITT.group14 (14.0.5.1)  0.503 ms  0.208 ms  0.204 ms

# Question 1.2

## Conf le poid

    router# conf t
    router(config)# interface INTERFACENAME
    router(config-if)# ip ospf cost 900

# Question 2.1

## Pour activer iBGP sur un router distant, on utilise ces commandes

    router# conf t
    router(config)# router bgp 14
    router(config-router)# neighbor LOOPBACK_DEST remote-as 14
    router(config-router)# neighbor LOOPBACK_DEST update-source lo
    router(config-router)# neighbor LOOPBACK_DEST next-hop-self

    router(config-router)# address-family ipv4 unicast
    router(config-router)# network 10.104.0.0/24

`network` : On indique quel est notre préfixe réseaux aux autres AS. (Juste avant on lance address-family ipv4 unicast pour déclarer le type d'adresses).  

`Update source` : L'addresse de loopback est propre à chaque routeur. En l'utilisant (et en indiquant au routeur local d'envoyer avec a sienne) on s'assure que même si l'interface tombe, le routeur va quand même essayer d'envoyer ses messages, à travers une autre route si elle existe.

`Next-hop-self` : permettra au routeur de bordure, de transmettre les requêtes de eBGP aux routeurs internes au travers de sa propre addresse ip. (Si ce l'était au travers du routeur externe, le routeur interne ne reconnaitrait pas le préfixe.)

## Route statique 
Mettre une route stattic à null permet d'informer le router emetteur du paquet que la destination est inateignable.
En mode sur un réseau /8 il y a des toues et la route static à null comble les trous.

    router# conf t  
    router(config)# ip route 14.0.0.0/8 null

Voir la route statique 

    router# show ip route static
    S   3.0.0.0/24 [1/0] via 2.0.0.2 inactive

# Question 2.2

## Route-map qui accepte le passage de n'importe quel traffic. 

`Création de la route`

    router# conf t
    router(config)# route-map ACCEPT_ALL permit 10
    router(config-route-map)# 

`Application de la route` 

    router# conf t
    router(config)# router bgp 2
    router(config-router)# neighbor 150.0.0.1 route-map ACCEPT_ALL in
    router(config-router)# neighbor 150.0.0.1 route-map ACCEPT_ALL out

## Conf

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
    
# BGP policies 

Regarde les confs

## Les regles 

    Provider : On peut lui exporter les routes apprises de ses clients, mais pas celles apprises de ses providers ou de ses pairs.
    Customer : On peut lui exporter les routes apprises de ses providers, ses pairs et ses clients
    Peer : On peut lui exporter les routes apprises de ses clients, mais pas celles apprises de ses providers ou de ses pairs.
