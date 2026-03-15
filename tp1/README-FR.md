# TP1 - Configuration du Routage Statique Cisco

**Auteur :** ALPHA Ziad Ramdan  
**Établissement :** ISM Dakar - L1 Cybersécurité  
**Date :** 11 Mars 2026  
**Outil :** Cisco Packet Tracer  

---

## Objectif

Configurer deux routeurs Cisco 2911 avec du routage statique pour permettre une communication de bout en bout entre deux réseaux LAN distincts.

---

## Topologie Réseau

![Topologie Réseau](topology.png)

---

## Plan d'Adressage IP

| Équipement | Interface    | Adresse IP     | Masque            | Rôle              |
|------------|-------------|----------------|-------------------|-------------------|
| R1         | Gi0/0       | 192.168.1.1    | 255.255.255.0     | Passerelle LAN1   |
| R1         | Gi0/1       | 10.0.0.1       | 255.255.255.252   | Lien vers R2      |
| R2         | Gi0/0       | 10.0.0.2       | 255.255.255.252   | Lien vers R1      |
| R2         | Gi0/1       | 192.168.2.1    | 255.255.255.0     | Passerelle LAN2   |
| PC0        | NIC         | 192.168.1.10   | 255.255.255.0     | Hôte LAN1         |
| PC1        | NIC         | 192.168.2.10   | 255.255.255.0     | Hôte LAN2         |

---

## Routes Statiques

| Routeur | Réseau de destination | Masque          | Prochain saut |
|---------|-----------------------|-----------------|---------------|
| R1      | 192.168.2.0           | 255.255.255.0   | 10.0.0.2      |
| R2      | 192.168.1.0           | 255.255.255.0   | 10.0.0.1      |

---

## Configuration de Sécurité

Chaque routeur est sécurisé avec :

- Mot de passe enable secret : `cisco`
- Mot de passe console : `cisco1`
- Mot de passe VTY (accès distant) : `cisco12`
- Chiffrement des mots de passe activé
- Bannière de connexion : `Acces strictement interdit`

---

## Vérification

### État des Interfaces (R2)

![Show IP Interface Brief R2](show_ip_int_br_r2.png)

### Résultats des Pings - PC0 vers PC1

![Résultats des Pings](ping_results.png)

Les 2 premiers paquets peuvent être perdus lors de la résolution ARP. Dès le second essai, le taux de succès atteint 100%.

---

## Compétences Acquises

- Adressage IP et segmentation réseau (masques /24 et /30)
- Routage statique entre deux réseaux
- Sécurisation de routeurs Cisco
- Tests de connectivité de bout en bout
- Documentation technique bilingue (FR et EN)

---

## Structure du Repository

```
cisco-routing-labs/
├── README-EN.md
├── README-FR.md
├── TP1_Configs_ALPHA_Ziad_EN.txt
├── TP1_Configs_ALPHA_Ziad_FR.txt
├── TP1_Cisco_ALPHA_Ziad_EN.docx
├── TP1_Cisco_ALPHA_Ziad_FR.docx
├── topology.png
├── show_ip_int_br_r2.png
└── ping_results.png
```
