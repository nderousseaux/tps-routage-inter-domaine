# Voir la session iBGP

Pour activer iBGP sur un router distant, on utilise ces commandes

    router# conf t
    router(config)# router bgp NO_AS
    router(config-router)# neighbor LOOPBACK_DEST remote-as NO_AS
    router(config-router)# neighbor LOOPBACK_DEST update-source lo
    router(config-router)# neighbor LOOPBACK_DEST next-hop-self


Il faut faire les mêmes commandes dans l'autre sens pour établir une session entre deux routeurs.

Update source : 
L'addresse de loopback est propre à chaque routeur. 
En l'utilisant (et en indiquant au routeur local d'envoyer avec la sienne) on s'assure que même si l'interface tombe, le routeur va quand même essayer d'envoyer ses messages, à travers une autre route si elle existe.

Next-hop-self : permettra au routeur de bordure, de transmettre les requêtes de eBGP aux routeurs internes au travers de sa propre addresse ip. (Si ce l'était au travers du routeur externe, le routeur interne ne reconnaitrait pas le préfixe.)