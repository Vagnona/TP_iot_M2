# Rapport TP 3 IOT

## Question 1

En examinant la fenêtre Mote output, on peut voir que l'un des paquet est étiqueté CSMA ContikiMAC. Le protocole utilisé est donc CSMA ContikiMAC. Le fait qu'il est possible d'ouvrir le document de recherche avec ce mot de passe nous indique que c'est bien la bonne réponse.

Avec ce genre de protocole, le problème classique est le problème de colisions. En effet, si deux noeuds parlent en même temps, les signaux vont s'interférer, rendant la lecture des deux signaux difficile.

## Question 3

Après avoir fait tourner la simulation durant 30 secondes, on peut voir que les noeuds envoient des paquets en continu, jusqu'à la reception de l'acquitement. 

<img src="img/timeline.png" alt="Envoie de paquet depuis 2" style="zoom:50%;" />

Dans la figure ci-dessus, on peut voir que le noeud numéro 2 envoie des paquets en continu, jusqu'à ce que les noeuds à portée (12, 21 et 29) envoient leur acquittement respéctifs.

On peut voir en vert les acquittements. Si ils ne sont pas reçu alors que le récepteurs étaient à porté, les messages s'afficheront en rouge, comme dans la figure suivante.

<img src="img/timeline-pas-recu.png" alt="pas reçu" style="zoom:50%;" />
  
Pour simuler un message envoyé, mais pas reçu, on a diminué le *success ratio* du noeud récepteur.

## Question 2 

La communication commence par un broacast de tous les noeuds qui broadcast DIS  

![DIS](Capture/DIS.png)

La racine envoie le premier DIO. Il s'agit du noeud 1

![1erDIO](Capture/1erDIO.png)

Les autres noeuds des DIO pour que les noeuds plus bas puisse rejoindre le réseau. 

![AutreDIO](Capture/AutreDIO.png)

Les noeuds envoie alors un DAO à leur parent. 
Le DAO (Destination Advertisement Object) transporte des informations sur les noeuds que l’émetteur peut joindre.

![DAO](Capture/DAO.png)

La racine est le noeud 1, elle a pour rang 256.

Le premier noeud à rejoindre l'arbre est le noeud 5 ayant pour rang 896

## Question 3

On lance le la simulation pendant 1 minute avec 10 noeuds pour simplifier. Puis on regarde le rang que partage chaque noeud dans leur message DIO.

![Shémas](img/Shemas_brute.png)

| Indice du noeud           | Rang            |
|---------------------|----------------------|
| 1  | 256   |
| 2  | 2176  |
| 3  | 2176  |
| 4  | 1536  |
| 5  | 896   |
| 6  | 896   |
| 7  | 896   |
| 8  | 1536  |
| 9  | 2176  |
| 10 | 1536  |

## Question 4

Les IPV6 utilisé par les noeuds : aaaa <=> FE80::::
![IPV6](img/IPV6.png)

Chaque IP est formé de la maniere suivante : FE80::::00AdresseMAC

![Mac et IPV6](img/Mac&IPV6.png)

On analyse une trame applicative : 

![7 vers 1](Capture/Data_trame.png)

Les différents champs : 
- Tf = 3 : Traffic class et flow label => élidée 
- nhc = true : (prochain en tête est compréssé)
- hlim = 2 : le hop limit est à 64 
- CID (Context Identifier extention) = 1 : 8 bits de CID suivent directement le DAM (Destination address mode)
- sam = 3 : L'IPv6 de la source est fe80::ff:fe00:AddrMAC
- Mcast = 0 : pas de multicast
- dam = 1 : L'Ipv6 destination est fe80::/64 + champs (champs qui correspondent au 16 bits qui suivent)

Le message envoyé est : "Hello 2 from the cli".

L'en-tête IPV6 ne correspond pas à une en-tête standard, Elle a été compressé ce qui signifie une utilisation du protocole 6Lowpan.

## Question 5 
On deplace le noeud 8 afin qu'il ne soit plus à porté du noeud 6
![Nouvelle topo](img/deplacement.png)

Il essaye d'atteindre le noeud 6 : Perte
![Perte](Capture/Perte8_6.png)

Le noeud 4 envoie un DIO à tous les noeud à proximité dont le 8 qui n'a pas reussi à joindre don parent depuis un moment.
![4 DIO](Capture/4_nvparent.png)

Le noeud 4 est à present son nouveau parent. Il lui envoie ses données applicative. 
![App](Capture/App8_4.png)

## Question 6

Le script compte le nombre de message reçu et envoyé pour chacun des noeuds et au total. 
Tout en calculant le (total paquet reçu / total paquet envoyé) le PRR.

Le resultat est cohérent avec ce que nous avons affirmé à la question 1 parce que, malgré le fait que dans la simulation les liens soient 100% fiable. le PRR (total paquets reçus / total paquets envoyés) n'est pas toujours égal à 1 et cela ne peut s'expliqué que par des collisions.

![collisions](img/collision.png)