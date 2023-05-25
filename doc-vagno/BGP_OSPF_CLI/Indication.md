# indications 

On va creer un full mesh iBGP
On va se positionner sur chacun des routeurs. 
Sur chaque voisin logique -> session iBGP
    taper update source et next-hop self

av de penser à eBGP penser iBGP

Ajouter les IP des routeurs de bordures. Utiliser l'outils d'aide (Attention de ne pas se tromper su l'IP SINON C'EST LA D FRERE)

On aura notre demon BGP
num de demon donner le num AS
declarer les sessions (update-source)
puis taper address-family ipv4 unicast
    - et annoncer le réseau (network...)
    - et positionner next-hop self

# Indication n°2

Faire les sessions iBGP mais deja fait. 
Deja fait.

appliquer les commandes network/8 
A priori elle sera refuser : 
    Seul façon de faire installer un route stattic 

Une fois que c fait 
Quand un paquet va arrivée dans le /8 

ibgp 
les routes stattic 
la commande network 

Ya un onglet -> looking glass (permet de prendre le point de vu d'un autre AS)
                    Perlet de savoir comment l'autre AS reçoit les messages. 

Pour l'instant -> connection inter-domaine qui marche en dehors des politiques (case oranges)

# Regle BGP 
__Du point de vu provider__

envoie
> C < Peer < Provider   
> 30  20       10

Le provider annonce pas ses routes aux provider

## Regle d'export 

    Exporting to a provider: 
    In exchanging routing information with a provider, an AS can export its routes and the
    routes of its customers, but can not export routes learned
    from other providers or peers. That is, an AS does not provide transit services for its provider.