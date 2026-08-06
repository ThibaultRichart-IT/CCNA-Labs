# 01 - Fonctions des appareils Cisco

## Présentation

Ce laboratoire a pour objectif de découvrir les principales fonctions des équipements Cisco IOS et d'utiliser les commandes de vérification les plus courantes.

## Objectifs

- Comprendre le rôle d'un switch et d'un routeur.
- Utiliser les commandes de diagnostic.
- Observer la table MAC d'un commutateur et la table de routage IPv4 d'un routeur.
- Configurer un interface réseau.
- Ajouter une route statique.

## Topologie

<img width="857" height="369" alt="11 Cisco Device Functions" src="https://github.com/user-attachments/assets/93c6c2e5-8a92-443a-b7b2-a9fbcc042fc8" />

## Déroulement du laboratoire

### Étape 1

Trouver les adresses logiques et physiques de chaque routeurs.

### Routeur 1
<img width="580" height="401" alt="R1" src="https://github.com/user-attachments/assets/be0f9d1e-d9e9-4acc-ad06-2e9dc84b98a9" />

### Routeur 2
<img width="588" height="430" alt="R2" src="https://github.com/user-attachments/assets/a74df742-552d-4c80-9b34-b3be020ef08f" />

### Routeur 3 
<img width="608" height="430" alt="R3" src="https://github.com/user-attachments/assets/b7e8c1e8-b16c-4e9f-865a-336c86aa43f0" />

### Routeur 4
<img width="609" height="417" alt="R4" src="https://github.com/user-attachments/assets/9c79cab6-0914-4353-9ea2-fabc87484d01" />



### Étape 2

Vérifier la connectivité entre les routeurs depuis routeur1.

Routeur 1 -> Routeur 2

<img width="516" height="107" alt="Ping R1-R2" src="https://github.com/user-attachments/assets/23c84f98-6beb-42e3-ae32-3c3fe1e9eb2e" />

Routeur 1 -> Routeur 3

<img width="495" height="111" alt="Ping R1-R3" src="https://github.com/user-attachments/assets/65522dc5-9595-4f8f-83cf-0cf95df54ce4" />

Routeur 1 -> Routeur 4

<img width="500" height="108" alt="Ping R1-R4" src="https://github.com/user-attachments/assets/f7598155-4a96-48fd-ab01-a745f8276937" />

### Étape 2.1 

Vérifier la connectivité entre les routeurs 3 et 4 depuis le routeur2.

Routeur 2 -> Routeur 3

<img width="485" height="105" alt="Ping R2-R3" src="https://github.com/user-attachments/assets/48cfa973-49ce-4a1b-9a42-37d146859566" />

Routeur 2 -> Routeur 4 

<img width="483" height="106" alt="Ping R2-R4" src="https://github.com/user-attachments/assets/12b3fd4a-26b3-488e-817a-8a9e189377df" />

### Étape 3 

Vérifier les adresses MAC apprise de manière dynamique sur SW1.

<img width="338" height="170" alt="SW1 MAC" src="https://github.com/user-attachments/assets/304977d2-8fd0-494b-a204-2d6590a1f3d3" />

Idem sur SW2.

<img width="332" height="197" alt="SW2 MAC" src="https://github.com/user-attachments/assets/c13f5c0d-bc54-428c-bd60-4c46984035a1" />

### Étape 3.1

Supprimer la table MAC de SW1.

<img width="335" height="198" alt="SW1 clear" src="https://github.com/user-attachments/assets/a90de735-7d2d-4a9f-8a9f-00aa5b4d078f" />

**Observation :** La table MAC dynamiques est supprimée, mais une adresse MAC réapparaît rapidement sur Fa0/24, car le switch apprend continuellement les adresses MAC des équipements connectés lorsqu'ils émettent le moindre trafic.

### Étape 4

Regarder la routing table sur R1.

<img width="590" height="215" alt="R1 route" src="https://github.com/user-attachments/assets/10b2f79d-06e0-4fc7-8455-83650a9ddb09" />


**Observation :** "10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks" indique que dans le réseau 10.0.0.0/8, le routeur connaît 2 sous-réseaux, avec 2 masques différents.
**10.10.10.0/24 sur Ge0/0** avec la lettre **C**, qui signifie "Connected", on déduit donc que ce réseau est directement connecté à l'interface Ge0/0
et
**10.10.10.1/32 sur Ge0/0 ** avec la lettre **L**, qui signifie "Local", on déduit donc qu'il s'agit de l'adresse IP du Routeur1, connecté a l'interface Ge0/0.
Je remarque aussi la présence d'une route en /32 avec le code L (Local). Cette route représente l'adresse IP exacte de l'interface du routeur et permet au routeur d'identifier le trafic qui lui est destiné directement.

### Étape 5

Configurer une IP 10.10.20.1/24 sur l'interface Ge0/1 sur R1.

<img width="438" height="134" alt="R1 G0-1" src="https://github.com/user-attachments/assets/7df375b3-83f3-4d9f-9cec-320e3a1e21ad" />

L'adresse est créer, je convertis le masque /24 en 255.255.255.0 pour l'attribution manuel.

### Étape 5.1

Je vérifie les interfaces Ge0/0 et Ge0/1

<img width="567" height="111" alt="R1 G0-1 1" src="https://github.com/user-attachments/assets/e1c88c69-e710-4463-bb28-1e2950083059" />

**Observation :** L'adresse que je viens de créer est bien affichée sur mon port Ge0/1 en revanche je remarque que le statuts de mon port Ge0/1 est "administratively down", il faut donc que j'active ce port.

<img width="628" height="357" alt="R1 - G0-1 up" src="https://github.com/user-attachments/assets/860ea57d-218c-49a3-ac6b-638c13853bc8" />

Les interfaces de routeurs sont administrativement fermée par défaut, la commande 'no shutdown' les rend ouverte.

### Étape 5.2 

Vérifier la routing table de R1


<img width="481" height="245" alt="R1 route 2" src="https://github.com/user-attachments/assets/b2265243-d3fc-4d27-80ab-4863e9b874f9" />

Observation : Le routeur a maintenant deux routes pour chaque interfaces et peux envoyer du trafic entre les hôtes sur les réseaux 10.10.10.0/24 et 10.10.20.0/24.


### Étape 6

Configurer une route statique pour 10.10.30.0/24 vers 10.10.10.2 sur Routeur1

<img width="471" height="89" alt="R1 iproute" src="https://github.com/user-attachments/assets/7377f1f9-0759-476b-a17b-cecf6ff8cfc1" />

Observation : C'est comme dire au routeur 'Pour aller vers ce réseau 10.10.30.0/24, envoie les paquets au routeur correspondant a l'adresse 10.10.10.2 (donc ici le routeur2) 

### Étape 6.1

Vérifier comment a réagit le routeur1 suite a la création d'une route statique vers le routeur2.

<img width="701" height="313" alt="resultat static" src="https://github.com/user-attachments/assets/e7c6d896-b413-474b-bb17-65495ed5a5f0" />

Observation : Le routeur1 est maintenant configuré pour que les paquets en destination du réseau 10.10.30.0/24 soit directement envoyé au routeur2 via 10.10.10.2 (l'adresse ip du routeur2).



## Commandes utilisées

<img width="890" height="201" alt="commande   descrpiton" src="https://github.com/user-attachments/assets/47142585-376c-4cb5-91f6-fd4abbf18f0b" />


## Ce que j'ai appris

J'ai compris le fonctionnement de la table de routage des routeurs et de la table MAC des commutateurs. J'ai appris à utiliser des commandes de diagnostic, configurer une interface réseau et ajouter une route statique sur un routeur.

## Difficultés rencontrées

Légère confusion quand je supprime la table MAC du switch et quand je vois qu'il y a (encore) une entrée dans la table MAC, au final c'est parce qu'une fois la table supprimer, le switch n'as pas d'entrée dans sa table et le moindre transfert de paquet et remis a jour dans sa table MAC.
Donc c'était en fait parfaitement normal.


## Conclusion

Ce laboratoire m'a permis de comprendre le fonctionnement des tables MAC et des tables de routage, de configurer des interfaces réseau et d'ajouter des routes statiques sur un routeur. J'ai également appris à utiliser des commandes de diagnostic afin de vérifier la connectivité et corriger les problèmes de configuration.


## Fin de ce laboratoire
