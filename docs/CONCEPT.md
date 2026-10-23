# Concept : RTS de survie et d'extraction réaliste

Nom provisoire : RTS-Zombie. Unity 6, rendu 3D pixelisé, vue de dessus.

## En une phrase

Tu diriges un groupe de survivants depuis une planque, dans une ville infestée. Tu envoies une escouade en expédition dans un quartier : elle fouille et ramasse ce qu'elle peut, puis doit rejoindre une sortie avant d'être submergée. Chaque blessure compte, chaque bruit se paie. Un survivant qui meurt est perdu pour toujours, avec tout ce qu'il portait.

- **RTS** : on commande une escouade, avec pause active, pas un seul personnage.
- **Project Zomboid** : vision limitée et brouillard, bruit, blessures détaillées, infection, fouille, monde persistant qui se dégrade.
- **Escape from Tarkov** : on ne garde que ce qu'on ramène, sorties conditionnelles, temps limité, inventaire en grille, planque à améliorer.
- **Ebbtide**, **They Are Billions** : ville moderne vue de dessus, murs à tenir, hordes, destructions persistantes.

## Piliers

1. **Réalisme avant confort.** Le joueur ne sait que ce que ses survivants voient et entendent. Une morsure ne se soigne pas. Une jambe cassée ralentit toute l'escouade.
2. **Chaque survivant compte.** Il a un nom, un passé, des traits, des compétences, des blessures et son équipement. Sa mort est définitive.
3. **Le bruit est l'ennemi.** Un coup de feu sauve une vie et attire la moitié du quartier.
4. **Rentrer, c'est gagner.** Le butin ne vaut rien tant qu'il n'est pas extrait.
5. **Le monde se souvient.** Maisons fouillées, murs abattus, cadavres, bâtiments effondrés : tout reste en place d'une expédition à l'autre.

## Boucle de jeu

```
Planque ──► Préparer l'escouade ──► Expédition ──► Extraction réussie ──► Planque
   ▲                                    │                                   │
   │                                    └── Mort : survivant et équipement perdus
   └──────────── Nuit de horde : défendre la planque ◄───────────────────────┘
```

Le temps avance : chaque expédition, chaque nuit de repos et chaque soin consomme des heures de jeu. Le groupe mange, boit, guérit ou s'aggrave.

## 1. Vision et brouillard (comme Project Zomboid)

Le joueur ne voit que ce que voit au moins un survivant. Aucune information gratuite.

- **Cône de vision** par survivant : environ 150° devant lui. Hors du cône, il ne perçoit que les deux ou trois mètres autour de lui.
- **Portée variable** :
  - lumière : plein jour, crépuscule, nuit, lampe torche ;
  - météo : pluie, brouillard réel ;
  - traits : myope, œil de lynx ;
  - état : la panique rétrécit le cône, une blessure à la tête trouble la vue ;
  - hauteur : depuis un toit ou un étage, on voit plus loin.
- **Ce qui bloque la vue** : murs, portes fermées, rideaux, véhicules. Haies et hautes herbes la bloquent en partie. Les fenêtres laissent voir.
- **Trois états du brouillard** :
  - inconnu : noir ;
  - mémorisé : désaturé et sombre, avec les bâtiments tels qu'ils étaient au dernier regard, sans les zombies ;
  - visible : en couleur, avec tout ce qui bouge.
- **Dernières positions connues** : un zombie qui sort du champ de vision laisse un marqueur fantôme qui s'estompe avec le temps.
- **Intérieurs cachés** : le toit d'un bâtiment masque l'intérieur jusqu'à ce qu'un survivant y entre ou regarde par une porte ou une fenêtre. Les murs entre la caméra et les survivants s'abaissent pour qu'on les voie.
- **Vision partagée, mais physique** : un survivant dans une maison ne voit pas la rue. L'escouade doit se placer pour voir.

## 2. Ouïe et bruit

Chaque action produit un bruit, avec un rayon et une intensité.

| Source | Rayon indicatif |
| --- | --- |
| Accroupi | quasi nul |
| Marche | 2 m |
| Course | 6 m |
| Ouvrir une porte | 4 m |
| Briser une vitre | 15 m |
| Porte enfoncée | 12 m |
| Coup de feu, pistolet | 40 m |
| Coup de feu, fusil | 60 m et plus |
| Alarme de maison ou de voiture | 80 m, dure longtemps |
| Mur qui s'effondre | 20 m |
| Générateur, moteur | 30 m, continu |

- Les murs étouffent le son. Un silencieux le réduit, un boitement l'augmente.
- Les zombies vont vers la source, avec une imprécision qui dépend de la distance. Leurs grognements en attirent d'autres : une horde peut faire boule de neige.
- **Ce que voit le joueur** : son propre bruit s'affiche en cercles. Ce que ses survivants entendent hors de leur vue apparaît comme un « ? » dans le brouillard.
- **Le bruit comme outil** : réveils, pétards ou alarme déclenchée exprès servent à attirer une horde ailleurs.

## 3. Blessures et santé

### Le corps

Dix-sept zones, comme dans Project Zomboid : tête, cou, torse, abdomen, aine, haut des bras, avant-bras, mains, cuisses, tibias et pieds, à gauche et à droite. L'interface les affiche sur une silhouette.

### Types de blessures

| Blessure | Effet principal | Soin |
| --- | --- | --- |
| Égratignure | Saignement léger | Pansement, désinfection |
| Lacération | Saignement moyen | Pansement, éventuellement suture |
| Plaie profonde, éclat de verre | Saignement fort, éclat logé | Pince, suture, pansement |
| Morsure | Saignement, infection zombie certaine | Aucun remède |
| Blessure par balle | Saignement fort, balle logée | Extraction, suture |
| Fracture | Lenteur (jambe), mauvaise visée (bras) | Attelle, des semaines de guérison |
| Entorse, luxation | Douleur, lenteur | Repos, bandage |
| Brûlure | Douleur, risque d'infection | Pansement propre |
| Commotion | Vue trouble, vertiges | Repos |

### Mécanismes

- **Volume sanguin** : environ 5 litres. Les saignements le vident. En dessous de certains seuils : vertiges, lenteur, perte de connaissance, puis mort.
- **Pansements** : ils se salissent et s'imbibent, il faut les changer. Un pansement sale provoque une infection bactérienne, avec de la fièvre, qui se soigne aux antibiotiques.
- **Douleur** : elle dégrade la visée et augmente la panique. Les antidouleurs ont des effets secondaires, comme la somnolence avec la morphine.
- **Conséquences tactiques** :
  - jambe touchée : lent, bruyant, incapable d'escalader une fenêtre ;
  - bras touché : arme à deux mains difficile à tenir, rechargement lent ;
  - grosse perte de sang : tout est plus lent.
- **Soigner prend du temps**, peut être interrompu et dépend de la compétence de premiers secours et du matériel disponible. Un survivant peut en porter un autre blessé, en se déplaçant lentement.

### Infection zombie

- Probabilité d'être infecté : morsure 100 %, lacération 25 %, égratignure 7 %. Ce sont les valeurs par défaut de Project Zomboid.
- **Le joueur ne sait pas tout de suite** si une égratignure est infectée. Les symptômes arrivent par étapes : nausée, fièvre, faiblesse, mort, puis réveil en zombie.
- **Choix extrême** : amputer un membre mordu dans les minutes qui suivent. C'est un choix risqué, qui laisse un handicap à vie.
- Un survivant infecté peut faire une dernière expédition avant de mourir.

### Protection

Les vêtements et protections couvrent des zones précises : blouson de moto, gants, casque, gilet de police, jambières. Ils réduisent le risque de morsure ou d'égratignure, se trouent et s'usent. Des vêtements sales ou ensanglantés augmentent le risque d'infection des plaies.

## 4. État des survivants

- **Besoins** : faim, soif, fatigue (sommeil), endurance (effort immédiat), température (froid, chaleur, vêtements mouillés), maladie.
- **Mental** : panique (au combat, devant des cadavres, à la mort d'un équipier), stress, moral.
- **Panique et obéissance** : un survivant terrorisé peut hésiter ou refuser un ordre suicidaire.
- **Traits** de départ, positifs et négatifs : ancien militaire, infirmier, athlète, courageux, lâche, myope, malentendant, ouïe fine, fumeur, insomniaque, maladroit.
- **Compétences** qui progressent à l'usage : visée, rechargement, armes blanches courtes et longues, armes contondantes, discrétion, pas légers, agilité, premiers secours, menuiserie, mécanique, électricité, cuisine.
- **Encombrement** : au-delà de la charge supportable, on est plus lent, plus bruyant et on s'épuise.

## 5. Combat réaliste et visée libre

### Visée libre

La balle part vraiment là où tu vises. Aucun jet de dés ne décide si un tir touche : chaque tir est une trajectoire simulée.

- **Le point visé** : le curseur désigne un point précis du monde en 3D, par exemple la tête d'un zombie, sa jambe, une fenêtre, la serrure d'une porte ou une voiture. La balle part du canon vers ce point.
- **La dispersion** : la balle s'écarte du point visé dans un cône dont la largeur dépend de :
  - l'arme, son état et le recul accumulé en tir rapide ;
  - la compétence de visée ;
  - la panique, la douleur, la fatigue, un bras blessé ;
  - le mouvement (arrêt, marche, course) et la posture (debout, accroupi) ;
  - le temps passé à viser : plus on tient la visée, plus le cône se resserre.

  Le réticule affiche ce cône en temps réel : ce que tu vois, c'est ce que tu risques.
- **Ce que la balle touche** : le premier obstacle sur sa trajectoire.
  - Chaque corps a des zones de touche (tête, torse, bras, jambes), donc viser la tête compte vraiment.
  - Un équipier dans la ligne de tir prend la balle.
- **Couverture par la hauteur** : un muret, une voiture ou un comptoir arrêtent les balles tirées en dessous de leur hauteur. S'accroupir derrière protège. La ligne de tir s'affiche et devient rouge si un obstacle la coupe.
- **Traversée** : les balles traversent le bois, le plâtre et les portes, pas la brique ni le béton. Elles perdent de la puissance à chaque traversée.
- **Effets sur les zombies** : un tir à la tête tue, un tir au corps fait chanceler, un tir dans les jambes peut le faire tomber et en faire un rampant.
- **Les trois façons de tirer** utilisent la même balistique :
  - en mode RTS, les survivants tirent seuls selon les règles d'engagement. Ils visent le corps ou la tête selon leur compétence, et peuvent rater ;
  - toujours en mode RTS, on peut ordonner un tir sur un point précis : la voiture dont on veut déclencher l'alarme, une porte, un zombie particulier ;
  - en prise en main (section 10), c'est toi qui vises à la souris.

### Armes et adversaires

- **Armes à feu** :
  - calibres et chargeurs à remplir balle par balle, balle chambrée ;
  - enrayements selon l'état de l'arme, recul, usure ;
  - bruit propre à chaque arme, flamme visible la nuit.
- **Contre les humains** : classes d'armure et pénétration des munitions.
- **Corps à corps** : il coûte beaucoup d'endurance et use les armes. On peut repousser ou faire tomber un zombie, puis l'achever au sol.
- **Être encerclé** : à trois zombies au contact, le survivant risque d'être agrippé, mis à terre et mordu. Être encerclé est presque toujours mortel.
- **Tir ami** possible : il faut gérer les lignes de tir.

## 6. Zombies

- **Types** :
  - marcheur (le plus courant) et coureur (rare) ;
  - rampant, aux jambes brisées, caché sous les voitures ;
  - faux mort, allongé parmi les cadavres ;
  - cogneur, qui défonce les portes ;
  - lourd, qu'un véhicule ne peut pas écraser ;
  - grimpeur, qui s'empile contre les murs, comme dans Ebbtide.
- **Sens** : ouïe forte, vue courte, odorat. Un survivant qui saigne laisse une trace que les zombies suivent.
- **Mémoire** : ils fouillent le dernier endroit où ils ont vu ou entendu une proie.
- **Obstacles** : ils ne savent pas ouvrir une porte mais peuvent la défoncer. Ils passent par les fenêtres brisées et franchissent les clôtures basses. Ils s'empilent contre les murs hauts ; les tirs font tomber la pile.
- **Point faible** : une horde vise la partie la plus fragile d'une défense, pas la plus proche.
- **Hordes migrantes** qui traversent les quartiers, attirées par les grands bruits (hélicoptère, explosion).
- **Population persistante** par quartier : les zombies tués restent morts. Leurs cadavres pourrissent et rendent malade s'ils restent près de la planque.

## 7. Monde

- **Ville fictive** découpée en quartiers de difficulté croissante :
  - résidentiel ;
  - centre-ville ;
  - zone commerciale ;
  - hôpital ;
  - commissariat ;
  - base militaire abandonnée, très dangereuse et très riche, l'équivalent du Labs de Tarkov.
- **Jour et nuit, météo, saisons** : la nuit cache, mais aveugle aussi. La pluie couvre les bruits et mouille. Le brouillard réel réduit la vue.
- **Effondrement de la ville** : l'eau puis l'électricité sont coupées après quelques jours. Il faut alors des générateurs, bruyants et gourmands en carburant.
- **Persistance** : le butin est limité et ne réapparaît pas. Les destructions restent, les cadavres aussi.
- **Bâtiments destructibles** : murs en segments d'un mètre, avec PV et matériau (bois, plâtre, brique, béton, tôle). Un segment détruit laisse des gravats. Ce qui n'est plus soutenu s'effondre.

## 8. Expéditions (comme Escape from Tarkov)

- **Préparation** : choisir le quartier, l'heure (la nuit est plus discrète mais plus dangereuse), l'escouade (1 à 5 survivants) et son équipement. Tout ce qui part peut être perdu.
- **Arrivée** : à un point d'entrée tiré au hasard parmi plusieurs.
- **Sorties** :
  - permanentes ;
  - ouvertes seulement certaines expéditions ;
  - conditionnelles : payer un passeur, passer sans sac à dos par les égouts, relancer un générateur (bruyant), tirer une fusée pour un hélicoptère (très bruyant), repartir en véhicule s'il a du carburant.
- **Durée** : de 60 à 90 minutes réelles selon le quartier, à régler en jouant. À la fin, les survivants restés sur place sont portés disparus.
- **Taille** : un quartier fait environ 1 km², à régler aussi. C'est assez pour remplir une heure à une heure et demie de jeu prudent : plusieurs dizaines de bâtiments à fouiller et plusieurs itinéraires possibles.
- **Sauvegarde unique** : on peut quitter une expédition en cours et la reprendre plus tard, mais pas revenir en arrière. Une mort est définitive.
- **Fouille** : chaque conteneur prend du temps à fouiller et les objets apparaissent un par un. Le butin dépend du lieu.
- **Poche sécurisée** : quelques cases qui restent même en cas de mort.
- **Après une mort** :
  - le corps reste sur la carte : une expédition suivante peut aller récupérer son équipement ;
  - un survivant mort de l'infection se relève en zombie, avec son équipement sur le dos.
- **Pillards humains** contrôlés par l'IA, qui fouillent, tirent et font du bruit.

## 9. La planque

Un vrai lieu de la ville, à fortifier, et non un simple menu.

- **Stock** en grille, où chaque objet a une taille et un poids.
- **Modules** à construire et améliorer :
  - murs (planches, tôle, sacs de sable, béton), portes, tours de guet ;
  - générateur, récupérateur d'eau, potager, cuisine ;
  - établi, infirmerie (chirurgie, amputation), dortoir ;
  - radio, qui débloque les contacts avec d'autres groupes.
- **Commerce et missions** : d'autres groupes de survivants achètent, vendent et donnent des missions, avec un système de réputation.
- **Nuits de horde** : la planque attire les zombies par le bruit et la lumière. Les murs perdent leurs PV, puis on répare.

## 10. Contrôles RTS

- Sélection au clic ou par rectangle, groupes de contrôle, ordres en file d'attente avec Maj.
- **Allures** : accroupi, marche, course.
- **Règles d'engagement** : ne pas tirer, riposter seulement, tirer à volonté, corps à corps seulement.
- **Ordres contextuels** :
  - fouiller ;
  - ouvrir, fermer, verrouiller ou barricader une porte ;
  - briser une vitre, escalader ;
  - soigner, porter un blessé ;
  - poser un piège ou un appât sonore ;
  - entrer dans une pièce en se plaçant de part et d'autre de la porte.
- **Pause active** et ralenti pour donner des ordres à tout moment.
- **Prise en main d'un survivant**, avec une touche ou un double-clic :
  - déplacement au clavier ;
  - le survivant regarde vers le curseur, et son cône de vision suit la visée, comme dans Project Zomboid ;
  - clic droit maintenu pour viser : il ralentit, la dispersion se resserre et la vue porte plus loin devant ;
  - clic gauche pour tirer ou frapper vers le curseur ;
  - le reste de l'escouade continue en mode RTS. On peut changer de survivant ou revenir à la vue RTS à tout moment.
- **Interface** :
  - portraits de l'escouade, silhouette des blessures, indicateurs d'état (faim, panique, douleur) ;
  - inventaire en grille avec glisser-déposer et rotation ;
  - cercles de bruit, marqueurs fantômes.

## 11. Rendu 3D pixelisé

- Scène 3D rendue en basse résolution puis agrandie sans lissage. La caméra se cale sur la grille de pixels pour éviter le scintillement.
- Contours, éclairage en paliers et palette limitée pour l'aspect pixel art.
- Brouillard rendu dans les shaders, à partir d'une texture de visibilité :
  - visible : en couleur ;
  - mémorisé : désaturé et sombre ;
  - inconnu : noir ;
  - transitions en tramage.
- Toits et murs qui s'effacent pour montrer les intérieurs.
- Interface et réticule en pleine résolution, au-dessus de la scène pixelisée, pour que la visée reste précise au pixel près.
- Destruction : segment remplacé par sa version cassée, gravats, débris et poussière.

## Ce qui viendra plus tard

- Véhicules complets : carburant, pannes, bruit.
- Étages multiples.
- Agriculture et artisanat avancé.
- Multijoueur coopératif, voire compétitif comme Tarkov.

Les jeux de référence sont détaillés dans [ANALYSE-REFERENCES.md](ANALYSE-REFERENCES.md). Les étapes de développement sont dans [FEUILLE-DE-ROUTE.md](FEUILLE-DE-ROUTE.md).
