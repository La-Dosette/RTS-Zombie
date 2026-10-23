# Feuille de route

Chaque jalon doit être jouable et testé avant de commencer le suivant. Cocher une case seulement quand le critère est vérifié dans l'éditeur et que les tests passent. Noter sous le jalon les écarts et les décisions prises en cours de route.

Le concept complet est dans [CONCEPT.md](CONCEPT.md), les règles de travail dans [CLAUDE.md](../CLAUDE.md).

## J0 — Socle

- [ ] Projet Unity 6 (URP, Input System) à la racine du dépôt, arborescence et assembly definitions de CLAUDE.md.
- [ ] Simulation : grille de 1 m découpée en chunks de 32 × 32 cases. Types de case : sol, rue, mur, porte, fenêtre, muret. Chaque case a une hauteur d'obstacle.
- [ ] Simulation : plan chargé depuis un fichier texte. Un quartier de test de trois ou quatre maisons avec pièces, portes et fenêtres.
- [ ] Simulation : chemin A* en 8 directions, sans couper les coins. Un survivant suit son chemin sur un tick fixe.
- [ ] Présentation : scène construite depuis la grille, avec des formes simples.
- [ ] Présentation : caméra RTS (déplacement clavier et bord d'écran, zoom, rotation par quarts de tour).
- [ ] Rendu pixelisé stable : pas de scintillement quand la caméra bouge. Interface en pleine résolution par-dessus.
- [ ] Sélection du survivant au clic, déplacement au clic droit, chemin affiché.
- [ ] Tests EditMode : chargement du plan, découpage en chunks, A* (contourne un mur, passe une porte, ne coupe pas un coin, signale l'absence de chemin).

## J1 — Vision et brouillard

- [ ] Cône de vision d'environ 150° et perception proche à 360°.
- [ ] Murs, portes fermées et rideaux bloquent la vue, les fenêtres la laissent passer.
- [ ] Brouillard à trois états : inconnu, mémorisé, visible. Le mémorisé est désaturé et ne montre aucune entité.
- [ ] Rendu du brouillard dans les shaders, transitions en tramage.
- [ ] Portes à ouvrir et fermer. Toits masquant les intérieurs jusqu'à ce qu'on y voie. Murs qui s'abaissent devant les survivants.
- [ ] Portée qui dépend de la lumière : jour, nuit, lampe torche.
- [ ] Tests : ligne de vue à travers mur, porte ouverte et fermée, fenêtre ; cône ; mémoire du brouillard.

## J2 — Bruit et zombies

- [ ] Événements sonores avec rayon et intensité, étouffés par les murs, affichés en cercles.
- [ ] Bruit perçu hors de vue affiché comme « ? » dans le brouillard.
- [ ] Zombies : errance, recherche de l'origine d'un bruit, poursuite, attaque, mémoire de la dernière position.
- [ ] Zombies : vue courte, ouïe forte, grognements qui en attirent d'autres.
- [ ] Zombies qui défoncent les portes et passent par les fenêtres brisées.
- [ ] Corps à corps simple : coup, repousser, achever au sol, coût en endurance.
- [ ] Encerclement : à trois zombies au contact, risque d'être agrippé et mis à terre.
- [ ] Marqueurs fantômes des zombies sortis du champ de vision.
- [ ] 200 zombies sans chute de fluidité.
- [ ] Tests : propagation et étouffement du bruit, transitions d'états, choix de cible.

## J3 — Blessures

- [ ] 17 zones du corps et types de blessures du concept.
- [ ] Volume sanguin, saignements, seuils (vertiges, lenteur, perte de connaissance, mort).
- [ ] Pansements qui se salissent, infection bactérienne et fièvre, douleur.
- [ ] Effets tactiques : jambe (lent, bruyant, pas d'escalade), bras (visée, rechargement).
- [ ] Infection zombie cachée (25 % lacération, 7 % égratignure, 100 % morsure), symptômes par étapes, mort puis réveil.
- [ ] Protection des vêtements par zone, usure et trous.
- [ ] Soins qui prennent du temps : pansement, désinfection, suture, attelle, antidouleur. Ils dépendent de la compétence.
- [ ] Interface : silhouette du corps avec les blessures, indicateurs d'état.
- [ ] Tests : tirage des blessures, saignement dans le temps, effets des soins, probabilités d'infection sur grand nombre de tirages.

## J4 — Escouade RTS et prise en main

- [ ] Plusieurs survivants : sélection par rectangle, groupes de contrôle, ordres en file avec Maj.
- [ ] Allures (accroupi, marche, course) avec leur bruit et leur vitesse.
- [ ] Règles d'engagement : ne pas tirer, riposter, tirer à volonté, corps à corps.
- [ ] Pause active et ralenti.
- [ ] Ordres contextuels : fouiller, ouvrir, barricader, briser une vitre, escalader, soigner un équipier, porter un blessé.
- [ ] Panique qui dégrade la précision et peut faire refuser un ordre suicidaire.
- [ ] Prise en main d'un survivant :
  - déplacement au clavier, regard vers le curseur, cône de vision qui suit ;
  - frappe vers le curseur ;
  - bascule entre survivants et retour à la vue RTS.
- [ ] Tests : file d'ordres, effets des allures, refus d'ordre.

## J5 — Armes à feu et visée libre

- [ ] Lancer de rayon de la simulation en 2,5D : grille, hauteurs d'obstacle, matériaux, zones de touche des corps (tête, torse, bras, jambes).
- [ ] Le curseur désigne un point 3D avec ce même lancer de rayon. La balle part du canon vers ce point.
- [ ] Dispersion qui dépend de :
  - l'arme et son recul ;
  - la compétence, la panique, la douleur, un bras blessé ;
  - le mouvement, la posture et le temps de visée.
- [ ] Réticule qui affiche la dispersion réelle. Ligne de tir rouge quand un obstacle la coupe.
- [ ] Couverture par la hauteur : un muret protège un survivant accroupi.
- [ ] Traversée du bois, du plâtre et des portes, avec perte de puissance. Arrêt sur la brique et le béton.
- [ ] Tir à la tête mortel, tir au corps qui fait chanceler, tir dans les jambes qui fait tomber.
- [ ] Tir ami : un équipier dans la ligne de tir est touché.
- [ ] Prise en main : clic droit pour viser (ralentit, resserre, voit plus loin), clic gauche pour tirer.
- [ ] Mode RTS : tir automatique selon les règles d'engagement, et ordre de tir sur un point précis.
- [ ] Armes : calibres, chargeurs remplis balle par balle, balle chambrée, enrayements selon l'état, bruit propre à chaque arme, flamme visible la nuit.
- [ ] Tests :
  - trajectoire identique avec la même graine ;
  - arrêt par un muret selon la hauteur ;
  - traversée selon le matériau ;
  - zone touchée selon le point visé ;
  - dispersion moyenne qui correspond au cône affiché, sur un grand nombre de tirs.

## J6 — Fouille et inventaire

- [ ] Conteneurs avec temps de fouille et objets révélés un par un. Butin selon le type de lieu.
- [ ] Inventaire en grille : poches, sac, gilet, poche sécurisée. Glisser-déposer et rotation.
- [ ] Poids et encombrement (vitesse, bruit, endurance).
- [ ] Définitions d'objets en ScriptableObjects, converties en données de simulation.
- [ ] Tests : placement en grille avec rotation, poids, tirage du butin.

## J7 — Expédition complète et planque minimale

- [ ] Quartier complet d'environ 1 km² : générateur de quartier (rues, parcelles, modèles de bâtiments) et lieux remarquables placés à la main.
- [ ] Préparation : quartier, heure, escouade, équipement.
- [ ] Points d'entrée tirés au hasard.
- [ ] Durée de 60 à 90 minutes, réglable par quartier. Survivants portés disparus à la fin.
- [ ] Sorties permanentes, aléatoires et conditionnelles (sans sac à dos, fusée bruyante).
- [ ] Mort : survivant et équipement perdus sauf la poche sécurisée. Le corps reste sur la carte.
- [ ] Sauvegarde unique : quitter une expédition en cours et la reprendre, sans retour en arrière possible.
- [ ] Planque en menu : stock, survivants, soins et guérison avec le temps, recrutement.
- [ ] Sauvegarde et chargement en JSON.
- [ ] Première boucle complète : planque, expédition, retour.
- [ ] Tests :
  - conditions de sortie ;
  - pertes à la mort ;
  - sauvegarde puis rechargement identiques, y compris en pleine expédition ;
  - quartier généré identique avec la même graine.

## J8 — Destruction et planque physique

- [ ] Murs en segments d'un mètre avec PV et matériau ; gravats ; effondrement de ce qui n'est plus soutenu.
- [ ] Planque comme lieu réel à fortifier : murs, portes, tours de guet.
- [ ] Nuits de horde : champ de flux qui intègre la solidité des murs, la horde vise le point faible.
- [ ] Zombies grimpeurs qui s'empilent contre les murs hauts ; les tirs font tomber la pile.
- [ ] Réparation des murs.
- [ ] Tests : dégâts par matériau, effondrement, choix du point faible.

## J9 — Monde vivant

- [ ] Jour et nuit, météo (pluie, brouillard), température.
- [ ] Besoins : faim, soif, fatigue, maladie.
- [ ] Coupures d'eau et d'électricité, générateurs.
- [ ] Hordes migrantes, persistance des quartiers (butin, cadavres, destructions).
- [ ] Survivant mort de l'infection relevé en zombie avec son équipement.

## J10 — Humains et commerce

- [ ] Pillards contrôlés par l'IA.
- [ ] Armures et pénétration des munitions.
- [ ] Groupes de survivants : commerce, missions, réputation, radio.

## Performances, à suivre dès J2

Un quartier d'environ 1 km² peut contenir des milliers de zombies et un million de cases.

- Ne simuler en détail que les chunks proches de l'escouade et des bruits importants ; ailleurs, simulation allégée.
- Données en tableaux de structures, Jobs et Burst pour la simulation, rendu instancié.
- Passer à DOTS seulement si c'est mesuré nécessaire.
- Mesurer à chaque jalon à partir de J2 : temps par tick de simulation, images par seconde, mémoire.
