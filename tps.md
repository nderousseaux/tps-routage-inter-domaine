# TPS - Mini-net

## Question 1.1

### Pour définir une route entre routeur sur une interface

```
router# conf t
router(config)# interface INTERFACENAME
router(config-if)# ip address ADDRESS
```

### Activer OSPF sur les routeurs

Pour définir une route OSPF sur une router :

```
router# conf t
router(config)# router ospf
router(config)# network IPOFINTERFACE/24 area 0
```

Pour afficher l'états des voisins ospf :

```
show ip ospf neighbor
```

Pour définir une addresse de loopback sur fixe avec ospf :

```
router ospf
ospf router-id IPLOOPBACK
```

### Convention de nommage des interfaces

Each router has interfaces to its neighbouring routers whose names follow the pattern `port_<neighbor>`. For instance, the interface on `NEWY` connected to `PITT` is named `port_PITT`. Moreover, each router has an interface connected to the host named `host` and a loopback interface called `lo`.

### Visualiser toutes les interfaces

```
show int b
```

### Assigner une addresse ip au client

```
ip address add ADDRESS dev INTERFACE
```

The interface to the router is called `<router-name>router`, e.g. `PITTrouter` for `PITT`. The loopback interface has the name `lo`.

IP **X.[100+Y].0.2/24** avec X le numéro d'as (14) et Y le numéro du routeur

Ensuite il faut donner la passerelle par défaut :

```
ip route add default via ADDRESSROUTER
```

- IP **X.[100+Y].0.1/24** avec X le numéro d'as (14) et Y le numéro du routeur

### Connectivité

Pour chaque routeur, il faut mettre en place la connectivité + ospf 

- avec ses voisins
  - IP : Sur la figure
  - Interface **port_NAME_OF_NEIGHBOR**
- l'adresse de loopback 
  - IP **X.[150+Y].0.1/24** avec X le numéro d'as (14)  et Y le numéro du routeur
  - Interface : **lo**
- Et l'hote
  - IP **X.[100+Y].0.1/24** avec X le numéro d'as (14) et Y le numéro du routeur
  - Interface **host**

Et sur l'hote

- IP : IP **X.[100+Y].0.2/24** avec X le numéro d'as (14) et Y le numéro du routeur
- Interface **ROUTERNAMErouter**

### Traceroute entre CHARhost et DETRhost

```bash
CHAR_host:~# traceroute 14.105.0.1
traceroute to 14.105.0.1 (14.105.0.1), 30 hops max, 46 byte packets
 1  CHAR-host.group14 (14.103.0.2)  0.241 ms  0.004 ms  0.004 ms
 2  PITT-CHAR.group14 (14.0.5.2)  0.212 ms  0.151 ms  0.157 ms
 3  DETR-PITT.group14 (14.0.7.2)  0.200 ms  0.193 ms  0.199 ms
 4  host-DETR.group14 (14.105.0.1)  0.359 ms  0.213 ms  0.217 ms
```

## Question 1.2

### Changer le cout d'une connection OSPF

```
router# conf t
router(config)# interface INTERFACENAME
router(config-if)# ip ospf cost 900
```

Le cout maximum est 65535.

```
STLO_host:~# traceroute 14.102.0.1
traceroute to 14.102.0.1 (14.102.0.1), 30 hops max, 46 byte packets
 1  STLO-host.group14 (14.107.0.2)  3.036 ms  0.004 ms  0.004 ms
 2  NASH-STLO.group14 (14.0.12.2)  2.130 ms  2.186 ms  4.765 ms
 3  CHAR-NASH.group14 (14.0.6.1)  0.279 ms  0.247 ms  0.205 ms
 4  NEWY-CHAR.group14 (14.0.4.1)  0.317 ms  0.272 ms  0.462 ms
 5  host-NEWY.group14 (14.102.0.1)  0.553 ms  0.484 ms  0.494 ms
```

## Question 2.1 - iBGP

Pour activer iBGP sur un router distant, on utilise ces commandes

```
router# conf t
router(config)# router bgp NO_AS
router(config-router)# neighbor LOOPBACK_DEST remote-as NO_AS
router(config-router)# neighbor LOOPBACK_DEST update-source lo
router(config-router)# neighbor LOOPBACK_DEST next-hop-self
```

Il faut faire les mêmes commandes dans l'autre sens pour établir une session entre deux routeurs.

**Update source :** L'addresse de loopback est propre à chaque routeur. En l'utilisant (et en indiquant au routeur local d'envoyer avec la sienne) on s'assure que même si l'interface tombe, le routeur va quand même essayer d'envoyer ses messages, à travers une autre route si elle existe.

**Next-hop-self** permettra au routeur de bordure, de transmettre les requêtes de eBGP aux routeurs internes au travers de sa propre addresse ip. (Si ce l'était au travers du routeur externe, le routeur interne ne reconnaitrait pas le préfixe.)

### Visualiser les sessions BGP

```
router# show ip bgp summary
```

Pour le router Nash :

```
NASH_router# show ip bgp summary  

IPv4 Unicast Summary (VRF default):
BGP router identifier 14.158.0.1, local AS number 14 vrf-id 0
BGP table version 0
RIB entries 0, using 0 bytes of memory
Peers 7, using 5015 KiB of memory

Neighbor        V         AS   MsgRcvd   MsgSent   TblVer  InQ OutQ  Up/Down State/PfxRcd   PfxSnt Desc
14.151.0.1      4         14       137       137        0    0    0 02:14:04            0        0 N/A
14.152.0.1      4         14       137       137        0    0    0 02:14:04            0        0 N/A
14.153.0.1      4         14       137       137        0    0    0 02:14:04            0        0 N/A
14.154.0.1      4         14       137       137        0    0    0 02:14:04            0        0 N/A
14.155.0.1      4         14         3         6        0    0    0 00:00:18            0        0 N/A
14.156.0.1      4         14       137       137        0    0    0 02:14:04            0        0 N/A
14.157.0.1      4         14       137       137        0    0    0 02:14:04            0        0 N/A

Total number of neighbors 7
```

## Question 2.2 - eBGP

Mettre en place eBPG avec les AS voisins (et les IXPs).

On configure l'addresse ip de l'interface, puis on lance une session BGP avec le router distant.

```
router# conf t
router(config)# interface INTERFACENAME
router(config-if)# ip address LOCAL_ADDRESS
router(config-if)# exit
router(config)# ip route 14.0.0.0/8 null
router(config)# route-map ACCEPT_ALL permit 10
router(config-route-map)# exit
router(config)# router bgp NO_AS_LOCAL
router(config-router)# neighbor ADDRESS_DEST remote-as NO_AS_DIST
router(config-router)# neighbor ADDRESS_DEST route-map ACCEPT_ALL in
router(config-router)# neighbor ADDRESS_DEST route-map ACCEPT_ALL out
router(config-router)# neighbor ADDRESS_DEST update-source lo
router(config-router)# address-family ipv4 unicast
router(config-router)# network LOCALPREFIX
router(config-router)# neighbor ADDRESS_DEST next-hop-self
```

**ip address** : Permet de rejeter automatiquement les requêtes n'étant pas dans notre réseau.

**route-map** : permet de définir un type de policy de routage. En l'occurence 10.

**network :** On indique quel est notre préfixe réseaux aux autres AS. (Juste avant on lance address-family ipv4 unicast pour déclarer le type d'adresses).

## Question 3.1

Par exemple pour un peer 11, AS local 14, 11 est un tier 1, valeur import : 200, valeur export : 30,

```
-- MAP IN
router# conf t
router(config)# route-map ONZE_IN permit 10
router(config-route-map)# set community 14:30
router(config-route-map)# set local-pref 200
router(config-route-map)# exit

-- Map OUT
router(config-router)# bgp community-list 11 permit 14:30
router(config-router)# route-map ONZE_OUT permit 10
router(config-route-map)# match community 11
router(config-route-map)# exit

router(config-route-map)# exit
router(config)# router bgp 14
router(config)# neighbor ADDRESS_DEST route-map ONZE_IN in
router(config)# neighbor ADDRESS_DEST route-map ONZE_OUT in
```

Les valeurs d'imports sont : 200 si c'est un client, 100 si c'est un peer, 50 si c'est un provider.

Les valeurs d'exports sont : 30 si c'est un client, 20 si c'est un peer, et 10 si c'est un provider.

- **Provider :** On peut lui exporter les routes apprises de ses clients, mais pas celles apprises de ses providers ou de ses pairs.
- **Customer :** On peut lui exporter les routes apprises de ses providers et ses pairs, mais pas celles apprises de ses clients
- **Peer :**  On peut lui exporter les routes apprises de ses clients, mais pas celles apprises de ses providers ou de ses pairs.

Ici, on interdit les routes de 12 et 13 d'aller vers le voisin.

```
DETR_router(config)# ip prefix-list PREFIX_AS13 permit <IP-PREFIX-AS13>
DETR_router(config)# ip prefix-list PREFIX_AS12 permit <IP-PREFIX-AS12>
DETR_router(config)# route-map DENY_FILTER deny 10 
DETR_router(config-route-map)# match ip address prefix-list PREFIX_AS12
DETR_router(config-route-map)# match ip address prefix-list PREFIX_AS13
DETR_router(config-route-map)# exit
DETR_router(config)# router bgp 14
DETR_router(config-router)# neighbor <IP-PREFIX-NEIGHBOR> route-map DENY_FILTER out
```

