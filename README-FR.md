# TP1 - Configuration et Routage Statique Cisco

**Auteur** : ALPHA Ziad Ramdan
**Filiere** : L1 Cybersecurite
**Ecole** : Institut Superieur de Management (ISM) - Dakar, Senegal
**Date** : 11 Mars 2026
**Outil** : Cisco Packet Tracer 8.2.2 - Routeur 2911

---

## Description

Configuration de deux routeurs Cisco 2911 interconnectes via un lien point a point, avec un PC de chaque cote simulant deux reseaux locaux distincts. La configuration a ete realisee depuis PC0 via cable console.

---

## Topologie du reseau

![Topologie Packet Tracer](screenshots/topology.png)

```
PC0 (192.168.1.10)
        |
     [Gi0/0]
        R1              LAN1 : 192.168.1.0/24
     [Gi0/1]
        |
   10.0.0.0/30          Lien point a point
        |
     [Gi0/0]
        R2              LAN2 : 192.168.2.0/24
     [Gi0/1]
        |
PC1 (192.168.2.10)
```

---

## Plan d'adressage IP

| Equipement | Interface | Adresse IP | Masque | Gateway |
|------------|-----------|------------|--------|---------|
| R1 | Gi0/0 | 192.168.1.1 | 255.255.255.0 /24 | - |
| R1 | Gi0/1 | 10.0.0.1 | 255.255.255.252 /30 | - |
| R2 | Gi0/0 | 10.0.0.2 | 255.255.255.252 /30 | - |
| R2 | Gi0/1 | 192.168.2.1 | 255.255.255.0 /24 | - |
| PC0 | Fa0 | 192.168.1.10 | 255.255.255.0 /24 | 192.168.1.1 |
| PC1 | Fa0 | 192.168.2.10 | 255.255.255.0 /24 | 192.168.2.1 |

NB : Le masque /30 entre les deux routeurs offre exactement 2 adresses utilisables. Un /24 aurait ete un gaspillage d'adresses sans aucun benefice.

---

## Securite configuree

| Parametre | Valeur |
|-----------|--------|
| Mot de passe mode privilegie | enable secret cisco |
| Mot de passe acces console | password cisco1 |
| Mot de passe acces distant VTY 6 sessions | password cisco12 |
| Chiffrement des mots de passe | service password-encryption |
| Banniere d'avertissement | Acces strictement interdit |

---

## Routes statiques configurees

R1 - pour atteindre LAN2 via R2 :
```
ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

R2 - pour atteindre LAN1 via R1 :
```
ip route 192.168.1.0 255.255.255.0 10.0.0.1
```

---

## Verification - show ip interface brief

![Show IP Interface Brief R2](screenshots/show_ip_int_br_r2.png)

```
Interface             IP-Address     OK?  Method  Status                 Protocol
GigabitEthernet0/0    10.0.0.2       YES  manual  up                     up
GigabitEthernet0/1    192.168.2.1    YES  manual  up                     up
GigabitEthernet0/2    unassigned     YES  unset   administratively down  down
```

---

## Resultats des tests de connectivite

![Resultats des pings](screenshots/ping_results.png)

| Source | Destination | Resultat | Observation |
|--------|-------------|----------|-------------|
| PC0 (192.168.1.10) | R1 Gi0/0 (192.168.1.1) | 100% | Communication locale parfaite |
| PC0 (192.168.1.10) | PC1 (192.168.2.10) | 100% | Communication inter-reseau reussie |

NB : Les 2 premiers paquets du premier ping vers PC1 etaient perdus lors de la resolution ARP. Au second essai : 100% de succes. Comportement tout a fait normal.

---

## Competences acquises

- Naviguer entre les modes IOS Cisco (Utilisateur, Privilegie, Configuration)
- Securiser un routeur avec plusieurs niveaux de mots de passe
- Chiffrer tous les mots de passe avec service password-encryption
- Configurer des interfaces GigabitEthernet et les activer avec no shutdown
- Mettre en place un plan d'adressage IP coherent avec choix justifie des masques
- Comprendre et configurer des routes statiques entre deux reseaux
- Tester la connectivite de bout en bout avec la commande ping
- Sauvegarder une configuration sur la memoire non volatile NVRAM
- Travailler en conditions reelles via cable console depuis un PC

---

## Commandes de verification

| Commande | Raccourci | Description |
|----------|-----------|-------------|
| show ip interface brief | sh ip int br | Etat de toutes les interfaces |
| show ip route | sh ip ro | Table de routage complete |
| show running-config | sh ru | Configuration en cours |
| copy running-config startup-config | cop ru st | Sauvegarde sur NVRAM |
| ping [adresse] | ping | Test de connectivite |

---

## Structure du repository

```
cisco-routing-labs/
├── README-EN.md
├── README-FR.md
├── configs/
│   └── TP1_Configs_ALPHA_Ziad.txt
├── report/
│   └── TP1_Cisco_ALPHA_Ziad.docx
└── screenshots/
    ├── topology.png
    ├── show_ip_int_br_r2.png
    └── ping_results.png
```

---

*ALPHA Ziad Ramdan - L1 Cybersecurite - ISM Dakar - 11 Mars 2026*
