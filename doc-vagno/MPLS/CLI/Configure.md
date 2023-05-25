# MPLS Configuration

[Lien](https://gitlab.unistra.fr/jr.luttringer/routage-inter-mini-internet/-/wikis/2.-Tutorial/2.3-Configuring-IP-routers/2.3.7-Configure-MPLS-LDP)

## Activer LDP

    router-id -> c'est la loopback address
    router# conf t
    router(config)# mpls ldp
    router(config-ldp)# router-id 18.158.0.1

## Activer UHP 

    router(config-ldp-af)# label local advertise explicit-null


