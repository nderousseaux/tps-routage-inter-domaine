# Mini internet 

> Lien du [wiki](https://gitlab.unistra.fr/jr.luttringer/routage-inter-mini-internet/-/wikis/Home)

## VPN
    $ sudo openfortivpn vpn.unistra.fr:443 -u vandrianandrasan  

    sur un autre terminal 
    $ while [ 1 ]; do sl ; done 

## Liens utiles

> [Matrice, looking glass, connection et krill](http://mai-mini-internet.u-strasbg.fr/as-connections/14)

__looking glass__  
On peut prendre le point de vue d'un autre domaine. Voir comment les autres voit les messages envoyés. 
(C'est pas parce que ça marche dans mon sens que ça marche dans l'autre.)

## Commande de connection
    ssh -p 2014 root@mai-mini-internet.u-strasbg.fr
    Password : 07d5dc185febda30 f64160d66c38cf5e

Measurement container 

  ssh -p 2099 root@mai-mini-internet.u-strasbg.fr
  The password to access the measurement container is 89818a700f0e0fa6 

## Recuperer les fichers de configs
    scp -r -P 2014 root@mai-mini-internet.u-strasbg.fr:~/path_to_the_directory .

# Connection à un routeur
    ./goto.sh DETR router 

# Connection à un hôte
    ./goto.sh BROO host

# Fichiers 

- Note_d'aide  
    - Debug.md -> Liste des commandes pour l'aide au deboggage
    - host_OSPF.md -> Liste les commandes pour conf les hôtes ospf
    - Indications.md -> Les indications du profs. 
    - Natha_eBGP.md -> Les commandes eBGP de Natha 
    - Natha_iBGP.md -> Les commandes iBGP de Natha 
    - Note_community.md -> 
    - Router_BGP.md -> Configuration BGP d'un router
    - Router_OSPF.md -> Configuration OSPF d'un router

- Correction 
  - 

- Commandes -> Liste des commandes explicitement passer dans le terminal
  - Cassé_Peer.md
  - Costumer.md
  - PEER.md
  - Provider.md

- Doc_aide -> Document d'aide. (schémas)

- Ficher_de_config
  - Ficher de config explicite

- Liste_commande.md -> Liste de commande random.

## BGP policies

- `Provider` : On peut lui exporter les routes apprises de ses clients, mais pas celles apprises de ses providers ou de ses pairs.
- `Customer` : On peut lui exporter les routes apprises de ses providers, ses pairs et ses clients
- `Peer`     :  On peut lui exporter les routes apprises de ses clients, mais pas celles apprises de ses providers ou de ses pairs.



