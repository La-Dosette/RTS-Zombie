# Feuille de route

Chaque jalon doit être jouable et testé avant de commencer le suivant. Cocher une case seulement quand le critère est vérifié dans l'éditeur et que les tests passent. Noter sous le jalon les écarts et les décisions prises en cours de route.

Le concept complet est dans [CONCEPT.md](CONCEPT.md), les règles de travail dans [CLAUDE.md](../CLAUDE.md).

## J0 — Socle

- [ ] Projet Unity 6 (URP, Input System) à la racine du dépôt, arborescence et assembly definitions de CLAUDE.md.
- [ ] Projets `dotnet/` : la simulation compile hors Unity (netstandard2.1, C# 9), et ses tests passent avec `dotnet test`.
- [ ] Simulation : grille de 1 m découpée en chunks de 32 × 32 cases. Types de case : sol, rue, mur, porte, fenêtre, muret. Chaque case a une hauteur d'obstacle.
- [ ] Simulation : plan chargé depuis un fichier texte. Un quartier de test de trois ou quatre maisons avec pièces, portes et fenêtres.
- [ ] Simulation : chemin A* en 8 directions, sans couper les coins. Un survivant suit son chemin sur un tick fixe de 20 Hz.
- [ ] Ordres et instantanés : le déplacement est un ordre sérialisable appliqué au tick suivant. La présentation ne lit que des instantanés, via un serveur intégré dans le même processus.
- [ ] Présentation : scène construite depuis la grille, avec des formes simples.
- [ ] Caméra comme Project Zomboid :
  - orthographique, isométrique fixe, sans rotation ;
  - zoom par paliers entiers ;
  - déplacement au clavier et au bord de l'écran.
- [ ] Rendu pixelisé stable : pas de scintillement quand la caméra bouge. Interface en pleine résolution par-dessus.
- [ ] Sélection du survivant au clic, déplacement au clic droit, chemin affiché.
- [ ] Tests :
  - chargement du plan et découpage en chunks ;
  - A* qui contourne un mur, passe une porte, ne coupe pas un coin et signale l'absence de chemin ;
  - un ordre sérialisé puis relu produit le même déplacement.

## J1 — Vision et brouillard

- [ ] Cône de vision d'environ 150° et perception proche à 360°.
- [ ] Murs, portes fermées et rideaux bloquent la vue, les fenêtres la laissent passer.
- [ ] Brouillard à trois états : inconnu, mémorisé, visible. Le mémorisé est désaturé et ne montre aucune entité.
- [ ] Rendu du brouillard dans les shaders, transitions en tramage.
- [ ] Portes à ouvrir et fermer. Toits masquant les intérieurs jusqu'à ce qu'on y voie. Murs qui s'abaissent devant les survivants.
- [ ] Portée qui dépend de la lumière : jour, nuit, lampe torche.
- [ ] Perception par joueur : l'instantané envoyé à la présentation ne contient que ce que l'escouade du joueur voit.
- [ ] Tests :
  - ligne de vue à travers un mur, une porte ouverte ou fermée, une fenêtre ;
  - cône de vision ;
  - mémoire du brouillard ;
  - aucune entité hors perception dans l'instantané d'un joueur.

## J2 — Bruit et zombies

- [ ] Événements sonores avec rayon et intensité, étouffés par les murs, affichés en cercles.
- [ ] Bruit perçu hors de vue affiché comme « ? » dans le brouillard.
- [ ] Zombies : errance, recherche de l'origine d'un bruit, poursuite, attaque, mémoire de la dernière position.
- [ ] Zombies : vue courte, ouïe forte, grognements qui en attirent d'autres.
- [ ] Zombies qui défoncent les portes et passent par les fenêtres brisées.
- [ ] Corps à corps simple : coup, repousser, achever au sol, coût en endurance.
- [ ] Encerclement : à trois zombies au contact, risque d'être agrippé et mis à terre.
- [ ] Marqueurs fantômes des zombies sortis du champ de vision.
- [ ] 500 zombies sans chute de fluidité.
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

- [ ] Escouade de 1 à 5 survivants : sélection par rectangle, groupes de contrôle, ordres en file avec Maj.
- [ ] Allures (accroupi, marche, course) avec leur bruit et leur vitesse.
- [ ] Règles d'engagement : ne pas tirer, riposter, tirer à volonté, corps à corps.
- [ ] Sans pause : réactions automatiques au danger (se mettre à couvert, riposter, reculer).
- [ ] Ordres contextuels : fouiller, ouvrir, barricader, briser une vitre, escalader, soigner un équipier, porter un blessé.
- [ ] Panique qui dégrade la précision et peut faire refuser un ordre suicidaire.
- [ ] Prise en main d'un survivant :
  - déplacement au clavier, regard vers le curseur, cône de vision qui suit ;
  - frappe vers le curseur ;
  - bascule entre survivants et retour à la vue RTS.
- [ ] Tests : file d'ordres, effets des allures, réactions automatiques, refus d'ordre.

## J5 — Armes à feu, visée libre et armures

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
- [ ] Gilets et casques avec classe de protection, munitions plus ou moins perforantes, blessures par balle sur les 17 zones.
- [ ] Prise en main : clic droit pour viser (ralentit, resserre, voit plus loin), clic gauche pour tirer.
- [ ] Mode RTS : tir automatique selon les règles d'engagement, et ordre de tir sur un point précis.
- [ ] Armes : calibres, chargeurs remplis balle par balle, balle chambrée, enrayements selon l'état, bruit propre à chaque arme, flamme visible la nuit.
- [ ] Deux escouades contrôlées en local (une par le joueur, une par une IA simple) pour tester les combats entre humains.
- [ ] Tests :
  - trajectoire identique avec la même graine ;
  - arrêt par un muret selon la hauteur ;
  - traversée selon le matériau ;
  - zone touchée selon le point visé ;
  - armure contre pénétration ;
  - dispersion moyenne qui correspond au cône affiché, sur un grand nombre de tirs.

## J6 — Réseau

- [ ] Serveur dédié `dotnet/RtsZombie.Server.csproj` qui fait tourner la simulation.
- [ ] Choix du transport noté ici, avec ses raisons.
- [ ] Deux joueurs au moins dans le même quartier, chacun avec son escouade.
- [ ] Le serveur n'envoie à chaque joueur que ce que son escouade perçoit.
- [ ] Interpolation des entités côté client, prédiction locale en prise en main.
- [ ] Compensation de latence pour les tirs, dans une limite de temps.
- [ ] Déconnexion : l'escouade reste et passe en défense automatique ; reconnexion à tout moment pendant l'expédition.
- [ ] Simulation de latence et de pertes de paquets pour tester.
- [ ] Test de charge : 12 clients simulés de 5 survivants chacun dans la même expédition.
- [ ] Tests :
  - aucune entité hors perception dans les paquets d'un joueur ;
  - un ordre invalide ou falsifié par un client est refusé ;
  - après une reconnexion, l'état est identique à celui du serveur ;
  - compensation de latence bornée dans le temps.

## J7 — Fouille et inventaire

- [ ] Conteneurs avec temps de fouille et objets révélés un par un. Butin selon le type de lieu.
- [ ] Inventaire en grille : poches, sac, gilet, poche sécurisée. Glisser-déposer et rotation.
- [ ] Fouille des corps, survivants d'autres joueurs compris.
- [ ] Poids et encombrement (vitesse, bruit, endurance).
- [ ] Inventaire décidé par le serveur : le client ne fait que demander un déplacement d'objet.
- [ ] Définitions d'objets en ScriptableObjects, converties en données de simulation et exportées en JSON pour le serveur.
- [ ] Tests : placement en grille avec rotation, poids, tirage du butin, refus d'un déplacement d'objet impossible.

## J8 — Expédition PvPvE complète et planque minimale

- [ ] Quartier complet d'environ 1 km² : générateur de quartier (rues, parcelles, modèles de bâtiments) et lieux remarquables placés à la main.
- [ ] Préparation : quartier, heure, escouade, équipement.
- [ ] Mise en relation simple : 8 à 12 escouades par expédition, points d'entrée différents et tirés au hasard.
- [ ] Durée de 60 à 90 minutes, réglable par quartier. Survivants portés disparus à la fin.
- [ ] Sorties permanentes, aléatoires et conditionnelles (sans sac à dos, fusée entendue par tout le quartier).
- [ ] Mort : survivant et équipement perdus sauf la poche sécurisée. Le corps reste et peut être fouillé par tous.
- [ ] Pas de sortie volontaire : abandonner l'expédition rend les survivants portés disparus.
- [ ] Comptes et persistance côté serveur : survivants, stock, planque. L'état ne fait qu'avancer.
- [ ] Planque en menu : stock, survivants, soins et guérison avec le temps, recrutement.
- [ ] Première boucle complète : planque, expédition PvPvE, retour.
- [ ] Tests :
  - conditions de sortie ;
  - pertes à la mort et à l'abandon ;
  - persistance après redémarrage du serveur ;
  - quartier généré identique avec la même graine ;
  - impossible de revenir à un état antérieur.

## J9 — Destruction et planque physique

- [ ] Murs en segments d'un mètre avec PV et matériau ; gravats ; effondrement de ce qui n'est plus soutenu.
- [ ] Planque comme lieu réel à fortifier, en instance PvE : murs, portes, tours de guet.
- [ ] Nuits de horde : champ de flux qui intègre la solidité des murs, la horde vise le point faible.
- [ ] Zombies grimpeurs qui s'empilent contre les murs hauts ; les tirs font tomber la pile.
- [ ] Réparation des murs.
- [ ] Tests : dégâts par matériau, effondrement, choix du point faible.

## J10 — Monde vivant

- [ ] Jour et nuit, météo (pluie, brouillard), température.
- [ ] Besoins : faim, soif, fatigue, maladie.
- [ ] Coupures d'eau et d'électricité, générateurs.
- [ ] Hordes migrantes, attirées entre autres par les fusillades entre joueurs.
- [ ] État général de la ville qui évolue pour tous les joueurs (coupures, dégradation, infestation).
- [ ] Survivant mort de l'infection relevé en zombie avec son équipement.

## J11 — Pillards IA et commerce

- [ ] Pillards contrôlés par l'IA, qui fouillent, tirent et font du bruit.
- [ ] Groupes de survivants contrôlés par le jeu : commerce, missions, réputation, radio.

## Performances, à suivre dès J2

Une expédition, c'est environ 1 km², un million de cases, jusqu'à 60 survivants de joueurs (12 escouades de 5), des pillards et environ 500 zombies. Avec autant de joueurs, la vision et la perception de chaque escouade coûtent plus cher que les zombies.

- Ne simuler en détail que les chunks proches des escouades et des bruits importants ; ailleurs, simulation allégée.
- Données en tableaux de structures, rendu instancié, Jobs et Burst côté client.
- Serveur : moins de 10 ms par tick pour une expédition complète.
- Passer à DOTS seulement si c'est mesuré nécessaire.
- Mesurer à chaque jalon à partir de J2 : temps par tick de simulation, images par seconde, mémoire.
