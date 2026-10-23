# RTS zombie : jeux de référence

Recherche du 23 octobre 2026. Sources : pages publiques, critiques, guides de joueurs, posts des développeurs et code source public de Withstand The Horde. Aucun jeu payant n'a été téléchargé ni exécuté.

## Direction visée

Un RTS de défense contre les zombies dans une **ville moderne** : murs à tenir, soldats et blindés, bâtiments destructibles, rendu **3D pixelisé**. La direction a ensuite évolué vers un RTS de survie et d'extraction en PvPvE, avec la caméra de Project Zomboid. La référence à jour est [CONCEPT.md](CONCEPT.md).

Point de départ : un post Instagram de **mantakustudio** du 23 octobre 2026, « Walls must be hold » (#isometricgame #rts #indiedev). On y voit des pâtés de maisons, une ligne de murs avec quatre blindés derrière, des soldats et des zombies dans les rues. Les commentaires demandent plusieurs types de zombies (les véhicules les écrasent trop facilement) et une logistique intéressante. Le nom du jeu n'a pas été trouvé : Instagram n'est pas indexé par les moteurs de recherche.

Le jeu publié le plus proche de cette direction est **Ebbtide**.

## Top 3 des RTS de référence

### 1. Ebbtide : le jeu le plus proche

[Steam](https://store.steampowered.com/app/4915700/Ebbtide/) · [X](https://x.com/ProjectAliveDev) · [YouTube](https://www.youtube.com/@EbbtideGame) · ProjectAlive, un seul développeur, Unity 6 et C#. En développement, sortie non annoncée, pas de démo publique.

- **Campagne.** On commande la TF-1 « Ark », dernier groupe aéronaval de l'humanité. La guerre commence dans les collines d'Hollywood et s'étend jusqu'au port de Los Angeles.
- **Deux échelles sans chargement.** Une carte d'opérations, en pause ou accélérée jusqu'à ×4, et un zoom continu jusqu'au combat de rue en 3D pixelisée.
- **Monde persistant.** Pas de missions séparées : pertes, zombies tués et bâtiments détruits restent inscrits sur la carte.
- **Unités.** Escouades, tanks et hélicoptères, pilotables en ordres RTS ou en contrôle direct. Appuis feu terrestres, navals et aériens : frappes de chasseurs, tirs de destroyer, missiles Tomahawk.
- **Bases avancées (FOB).** Murs HESCO et béton, portes, échafaudages pour défendre depuis les murs, nids de mitrailleuses, CIWS, mortiers, dépôt de munitions, point de carburant, éclairage. Il faut aussi garder les lignes de ravitaillement ouvertes et secourir des survivants.
- **Zombies qui grimpent.** Inspirés de World War Z : ils s'empilent pour atteindre les soldats, les tirs les font tomber et la pile peut s'effondrer. Un toit peut devenir une ligne de front.
- Les hordes sont dirigées par une intelligence collective, le « Nexus ».
- Le développeur indique avoir utilisé l'IA pour une partie de la musique, des voix, des sons, des illustrations et de certains modèles 3D.

À reprendre : la ville réelle comme champ de bataille, les murs militaires et leurs défenses, la pile de zombies qui escalade, la logistique munitions et carburant, et les destructions qui persistent.

### 2. Infection Free Zone : la ville réelle comme forteresse

[Steam](https://store.steampowered.com/app/1465460/Infection_Free_Zone/) · Jutsu Games.

RTS zombie sur une carte du monde réel : on transforme des pâtés de maisons existants en zone défendue. Les infectés percent les murs, d'où les conseils de doubler les murs et d'ajouter des miradors. Ils se cachent aussi dans les grands bâtiments, où se forment des nids de plusieurs dizaines de zombies. On peut faire s'effondrer un bâtiment en le visant de loin. Statut à vérifier : la page Steam indique toujours l'accès anticipé, alors que deux critiques d'octobre 2026 annoncent une sortie complète le 27 août 2026.

À reprendre : les nids dans les bâtiments, les murs qui cèdent, les bâtiments qu'on peut abattre.

### 3. They Are Billions : la boucle de défense

[Steam](https://store.steampowered.com/app/644930/They_Are_Billions/) · Numantian Games, 2019.

La référence du genre : bâtir une colonie, la fermer de murs et de tours, survivre à des vagues de milliers d'infectés, en temps réel avec pause. Les zombies qui atteignent un bâtiment infectent ses habitants, qui rejoignent la horde : un seul zombie entré dans la colonie peut tout faire tomber. Les guides de joueurs signalent aussi que les chantiers en cours peuvent être attaqués.

À reprendre : l'infection en cascade, la pause tactique, la pression croissante des vagues. Le style est en 2D dessinée, steampunk.

## Références complémentaires

### Cataclismo : la destruction des structures

[Wikipédia](https://en.wikipedia.org/wiki/Cataclismo) · Digital Sun / Hooded Horse, sortie complète le 20 mars 2025.

Ses ennemis ne sont pas des zombies mais des « Horreurs ». C'est pourtant la meilleure référence pour les structures destructibles : on construit de jour, pièce par pièce (plus de 100 pièces), et on se défend la nuit. Chaque pièce a ses propres règles de stabilité : la hauteur et le matériau décident de la solidité. Le bois est souple mais fragile, la pierre chère mais solide et renforcée à partir de quatre blocs de haut. Si une fondation en pierre tombe, ce qui repose dessus peut tomber aussi. Une fois la brèche ouverte, seules les troupes arrêtent la horde. Perdre un mur ne fait pas perdre la partie, il faut le reconstruire.

### Withstand The Horde : un RTS zombie au code public

| | |
| --- | --- |
| Auteur | Pere Brau (Arperor), seul développeur, Instagram [@perebrau](https://www.instagram.com/perebrau/) |
| État | En développement, démo gratuite |
| Où jouer | [Steam (démo)](https://store.steampowered.com/app/5291660/), [itch.io](https://arperor.itch.io/withstand-the-horde), [Game Jolt](https://gamejolt.com/games/withstand-the-horde/1107809) |
| Moteur | Godot 4.7, 2D, physique Jolt |
| Code | [github.com/Arperor/Arperor](https://github.com/Arperor/Arperor), **sans licence** : on peut l'étudier, pas en reprendre le code ni les assets |

Le principe : construire une ville fortifiée, gérer des ressources, puis tenir face à la horde. Mode campagne et mode infini, temps réel avec pause. Le devlog de la démo annonce environ 2 500 zombies simulés sur la carte graphique (compute shader) et huit unités : éclaireur, soldat, sniper, costaud, fusil à pompe, bombes, médecin et constructeur.

Le dépôt GitHub contient une version antérieure à la démo actuelle : scripts GDScript et exports `.pck` 0.2 à 0.6. La simulation des zombies sur carte graphique n'y figure pas encore. Ce qui suit décrit cette version.

**Destruction et réparation.** Un bâtiment à 0 PV n'est pas supprimé. Il devient une ruine grise et translucide : plus de collision, plus de production, sa population est retirée. Un constructeur lui rend des PV ; à 100 %, le bâtiment se réactive. Un chantier neuf suit le même chemin, puisqu'il naît à l'état de ruine. Un même état sert donc pour « détruit » et « en construction ».

**Infection.** Un bâtiment détruit libère des zombies autour de lui : trois par défaut, aucun pour un mur. C'est le principe de They Are Billions.

**Murs.** Une case de 64 px, 2 000 PV, coût 10 or et 20 bois. Chaque mur émet aussi une zone d'attraction de 767 px de rayon : il attire les zombies vers lui et sert d'appât.

**Attraction.** Les bâtiments envoient périodiquement un signal aux zombies présents dans leur zone, plus fort quand le zombie est proche. Un zombie ne change de cible que si le nouveau signal dépasse l'actuel d'au moins 0,15, ce qui l'empêche d'hésiter entre deux bâtiments.

**Comportement des zombies.** Quatre états : errance, poursuite, inactif et horde. Un zombie sans humain à moins de 1 500 px passe inactif pour économiser du calcul. Il se réveille au hasard (30 % de chance toutes les 8 s, pendant 2 s). En chemin, il attaque n'importe quel bâtiment situé à moins de 80 px : les murs se font donc ronger au passage. Il sprinte (×2) à moins de 120 px de sa cible. Un zombie sur dix est une variante rapide teintée de rouge.

**Hordes.** Elles commencent 120 s après la construction de la base, puis reviennent toutes les 60 s. La première compte 10 zombies, chaque suivante 5 de plus. Elles apparaissent dans la zone non explorée, à 200–400 px de ce que le joueur a découvert, donc toujours juste hors de vue. Dans cette version, la carte ne contient pas plus de 60 ennemis à la fois.

**Déplacement.** Un champ de flux (flow field) est calculé une fois par parcours en largeur depuis la base, sur une grille de 64 px, en huit directions, sans couper les angles. Seuls l'eau et les montagnes bloquent : **les bâtiments n'entrent pas dans le calcul**. La horde avance en ligne droite et attaque le premier mur qu'elle rencontre. Pour éviter que les zombies se superposent, chacun interroge la physique pour trouver jusqu'à 12 voisins, une image sur quatre, avec une notion de masse.

**Passage au GPU.** L'auteur a publié [un article sur gamedev.net](https://gamedev.net/blogs/entry/2298519-putting-2500-zombies-on-the-gpu-in-godot-4-the-bottleneck-was-the-readback) sur ce passage. Sa première version GPU était à peine plus rapide que la version CPU, parce qu'il relisait chaque image les positions des zombies pour la sélection, les dégâts et les collisions avec les murs. Cette relecture oblige à attendre la carte graphique. Sa solution : laisser le GPU propriétaire des données, et faire envoyer au CPU des intentions plutôt que des requêtes de positions.

## Ce que cela donne pour notre jeu

| Mécanique | Référence | Proposition |
| --- | --- | --- |
| Ville réelle, bâtiments destructibles | Ebbtide, Infection Free Zone | Pâtés de maisons modulaires dont les dégâts restent visibles et persistent |
| Murs militaires | Ebbtide | HESCO (bon marché, rapide) et béton (cher, solide), portes, échafaudages pour tirer depuis le mur |
| Zombies qui escaladent | Ebbtide | Pile de zombies au pied du mur : la hauteur du mur compte, les tirs et explosions font tomber la pile |
| Murs qui cèdent | Cataclismo, Infection Free Zone | PV par segment, états visibles (intact, abîmé, brèche, gravats), gravats qui ralentissent le passage |
| Véhicules | Ebbtide, post mantakustudio | Blindés puissants mais dépendants du carburant et des munitions, et des zombies lourds qu'on ne peut pas écraser |
| Logistique | Ebbtide | Dépôts de munitions et de carburant à ravitailler, routes de ravitaillement à protéger |
| Bâtiment détruit → zombies | They Are Billions, Withstand | Les bâtiments habités libèrent des zombies quand ils tombent |
| Ruine réparable | Withstand | Un même état pour « détruit » et « en chantier » |
| Nids | Infection Free Zone | Grands bâtiments abandonnés qui génèrent des zombies tant qu'on ne les nettoie pas |
| Choix du point d'attaque | Withstand (limite) | Intégrer les murs au champ de flux avec un coût lié à leurs PV : la horde vise le point faible au lieu de foncer droit |
| Milliers de zombies | Withstand (article GPU) | Données des zombies dans des tableaux compacts ou sur le GPU, pas un objet physique par zombie |

## Choix retenu : 3D pixelisée sous Unity 6

Ebbtide, comme le post de mantakustudio, ne dessine pas ses sprites à la main : la scène est en 3D, rendue en basse résolution puis agrandie sans lissage. Cette approche facilite les bâtiments destructibles, puisque des morceaux de modèles 3D tombent au lieu d'un sprite à redessiner pour chaque état de dégât. Elle facilite aussi la caméra de dessus. C'est le rendu retenu, sous Unity 6 comme Ebbtide. Voir [AGENTS.md](../AGENTS.md).

## Outils

[REA](https://github.com/morluto/rea) sait lire les paquets Godot `.pck` (Withstand) et les jeux Unity, qu'ils soient en .NET ou en IL2CPP (Ebbtide, quand une démo sortira). Commande type : `rea inspect-artifact chemin/du/jeu.pck --detail summary --evidence_output preuves.json --json`. Pendant cette recherche, REA n'a pas pu être installé dans l'environnement cloud, et itch.io, gamedev.net, YouTube, Instagram et Steam y étaient bloqués : les informations viennent des moteurs de recherche et du code public de Withstand.
