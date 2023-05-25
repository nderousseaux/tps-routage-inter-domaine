# BGP

## voir la session BGP
    router# show ip bgp summary 


## Configurer la session BGP 
    router# conf t  

`Configuration de la session BGP`  

    router(config)# router bgp 14

`eBGP`  

    router(config-router)# neighbor 150.0.0.1 remote-as 15  

`iBGP`  

    router(config-router)# neighbor 2.0.0.2 remote-as 14  

## Creer une route-map qui accept tout (ici elle s'appelle ACCEPT_ALL)
    router# conf t  
    router(config)# route-map ACCEPT_ALL permit 10  
    router(config-route-map)#  

Ensuite, vous devez appliquer cette route-map sur la session BGP, pour les préfixes annoncés et reçus :  
  
    router# conf t
    router(config)# router bgp 2  
    router(config-router)# neighbor 150.0.0.1 route-map ACCEPT_ALL in  
    router(config-router)# neighbor 150.0.0.1 route-map ACCEPT_ALL out  


# Allouer la route-map precedement creer à une session BGP (en entrée et sortie)
    router# conf t  
    router(config)# router bgp 14  
    router(config-router)# neighbor 150.0.0.1 route-map ACCEPT_ALL in  
    router(config-router)# neighbor 150.0.0.1 route-map ACCEPT_ALL out  

# Pour supprimer une route-map
    $ no route-map ACCEPT_ALL 

# annoncer son propore prefix  
    router# conf t  
    router(config)# router bgp 14  
    router(config-router)# network LOCALPREFIX/24  

# Update-source (you want to configure an iBGP session from CHIC to CHAR)  
    router# conf t  
    router(config)# router bgp 14  
    router(config-router)# neighbor 7.153.0.1 remote-as 14  
    router(config-router)# neighbor 7.153.0.1 update-source lo  

`7.153.0.1 is the loopback interface IP of CHAR.`  
`With update-source lo you make sure that the CHIC router is using its loopback interface address as source address.`  

## Route Stattic 

    router# conf t  
    router(config)# ip route 14.0.0.0/8 null  

Mettre une route stattic à null permet d'informer le router emetteur du paquet que la destination est inateignable.  
En mode sur un réseau /8 il y a des toues et la route static à *null* comble les trous.   



----------------------------------------

# prefixt-list 

## bgp community-list 

    router# conf t
    router(config)# route-map MY_ROUTE_MAP permit 10
    router(config-route-map)# match ?

# Les BGP policies

## Route maps 

La partie `match` est un prédicat booléen qui  
décide sur quelles routes BGP la _route-map_ applique les actions (généralement, l'attribut
manipulation) défini dans la partie `set`.

## Match part

    router# conf t  
    router(config)# route-map MY_ROUTE_MAP permit 10  
    router(config-route-map)# match ?  
        as-path  
            Match BGP AS path list  
        community  
            Match BGP community list  
        interface  
            Match first hop interface of route  
        ip  
            IP information  
        metric  
            Match metric of route  
        origin  
            BGP origin code  
        peer  
            Match peer address  


Whenever you create a *route-map*, you must indicate if you want to __permit__ or __deny__ the input data. Intuitively, only  permitted routes will go through the set of actions, while denied routes won't be considered further. If you have no match part, the  set part is applied to all BGP routes  

## Set part

    router# conf t
    router(config)# route-map MY_ROUTE_MAP permit 10  
    router(config-route-map)set ?  
        as-path  
            Transform BGP AS-path attribute  
        community  
            BGP community attribute  
        ip  
            IP information  
        metric  
            Metric value for destination routing protocol  
        local-pref  
            BGP local preference path attribute  
        origin  
            BGP origin code  

## Apply the route-map
    router# conf t
    router(config)# router bgp 15 
    router(config-router)# neighbor 2.0.0.2 route-map MY_ROUTE_MAP in  

If you use __out__ instead of __in__, the route-map is applied to all outgoing routes instead.

## Prefix-list  
    router# conf t  
    router(config)# ip prefix-list OWN_PREFIX permit 192.168.0.0/16  

## Assigner à une route-map 
    router(config)# route-map filter permit 10  
    router(config-route-map)# match ip address prefix-list OWN_PREFIX  
    router(config-route-map)# set ...  

## Prefix range
    router# conf t
    router(config)# ip prefix-list OWN_PREFIX seq 5 permit 192.168.0.0/16 le 20

matches all prefixes with prefix length lower or equal to /20.  
In other words it matches prefixes from 192.168.0.0/16 to 192.168.0.0/20.  

## BGP community values 

if you are AS number 15 and you want to use the community value to tag certain BGP routes with the value 100,
you could use the following BGP community value: `15:100`. You can also add more than one BGP community value to a route.

> Community list  
The community list has the name/number 1.  
  
    router# conf t  
    router(config)# bgp community-list 1 permit 15:100  

# Example

Let's assume you have a router which has two eBGP connections to two different  
neighbors with the IPs __11.11.11.11 and 22.22.22.22__.  
You want to set the local preference of all routes coming from __11.11.11.11 to 1000__.  
Furthermore, you want to use BGP community values to make sure that only the routes  
coming from neighbor __11.11.11.11 are advertised to neighbor 22.22.22.22__:

    router# conf t  
    router(config)# route-map MAP_IN permit 10  
    router(config-route-map)# set community 15:100  
    router(config-route-map)# set local-preference 1000  
    router(config-route-map)# exit  

    router(config-router)# bgp community-list 1 permit 15:100  

    router(config-router)# route-map MAP_OUT permit 10  
    router(config-route-map)# match community 1  
    router(config-route-map)# exit  

    router(config)# router bgp 15  
    router(config-router)# neighbor 11.11.11.11 route-map MAP_IN in  
    router(config-router)# neighbor 22.22.22.22 route-map MAP_OUT out 

# Permit
C'est un ordre de `priorité` quand on met plusieurs route-map sur un même voisin.

    router# conf t
    router(config)# ip prefix-list OWN_PREFIX permit 192.168.0.0/16


    router(config)# route-map filter permit 10
    router(config-route-map)# match ip address prefix-list OWN_PREFIX
    router(config-route-map)# set ...

- `Provider` : On peut lui exporte les routes apprises de ses clients, mais pas celles apprises de ses providers ou de ses pairs.
- `Customer` : On peut lui exporte les routes apprises de ses providers et ses pairs, mais pas celles apprises de ses clients
- `Peer`     :  On peut lui exporter les routes apprises de ses clients, mais pas celles apprises de ses providers ou de ses pairs.


## BGP AS path 

