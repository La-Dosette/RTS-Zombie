# Concept : RTS de survie et d'extraction

Nom provisoire : RTS-Zombie.

## En une phrase

Tu diriges un groupe de survivants depuis une planque, dans une ville infestée. Tu envoies une escouade en expédition dans un quartier : elle fouille et ramasse ce qu'elle peut, puis doit rejoindre une sortie avant d'être submergée. Un survivant qui meurt est perdu pour toujours, avec tout ce qu'il portait.

- **RTS** : on commande une escouade, on ne joue pas un seul personnage.
- **Project Zomboid** : le bruit attire les zombies, une morsure ne pardonne pas, les maisons se fouillent pièce par pièce et le monde se dégrade.
- **Escape from Tarkov** : on ne garde que ce qu'on ramène, les sorties ont des conditions, le temps est limité, l'inventaire est en grille et la planque s'améliore entre deux expéditions.

## Piliers

1. **Chaque survivant compte.** Il a un nom, des compétences, des blessures et son équipement. Sa mort est définitive.
2. **Le bruit est l'ennemi.** Un coup de feu sauve une vie et attire la moitié du quartier.
3. **Rentrer, c'est gagner.** Le butin ne vaut rien tant qu'il n'est pas extrait.
4. **Les murs tiennent, jusqu'à ce qu'ils cèdent.** Murs et bâtiments sont destructibles, par les zombies comme par le joueur.
5. **Le monde se souvient.** Les maisons fouillées restent vides, les murs abattus restent à terre, les morts restent dans la rue.

## Boucle de jeu

```
Planque ──► Préparer l'escouade ──► Expédition ──► Extraction réussie ──► Planque
   ▲                                    │                                   │
   │                                    └── Mort : survivant et équipement perdus
   └──────────── Nuit de horde : défendre la planque ◄───────────────────────┘
```

### À la planque, entre deux expéditions

- **Stock** en grille : chaque objet a une taille et un poids.
- **Survivants** : soigner, nourrir, équiper, recruter. Les blessures guérissent avec le temps et les soins.
- **Modules** à construire et améliorer : générateur, établi, infirmerie, récupérateur d'eau, cuisine, murs et tours.
- **Échanges** avec d'autres groupes de survivants : revendre le butin de valeur, acheter munitions et soins, missions avec réputation.
- **Besoins du groupe** : nourriture et eau consommées chaque jour. Il faut donc sortir.

### En expédition

- On choisit un quartier, de 1 à 5 survivants et leur équipement. Tout ce qui part peut être perdu.
- **Durée limitée** : la pression monte avec le temps, avec des hordes de plus en plus grosses et la nuit qui tombe.
- **Plusieurs sorties**, certaines conditionnelles :
  - toujours ouverte mais loin ;
  - à payer ;
  - sans sac à dos (passage étroit, égouts) ;
  - à déclencher au prix de beaucoup de bruit (fusée pour un hélicoptère, générateur à relancer).
- **Fouille** : chaque conteneur prend du temps à fouiller. Le butin varie selon le lieu : pharmacie, armurerie, supermarché, commissariat.
- **Poche sécurisée** : quelques cases que l'on garde même en cas de mort.

### Nuits de horde

Régulièrement, une horde attaque la planque. Les murs perdent leurs PV, les zombies cherchent le point faible, puis il faut réparer. Cette partie reprend la défense de They Are Billions et d'Ebbtide.

## Survivants

- **Santé par zone** : tête, torse, ventre, bras et jambes. Saignements, fractures et douleur. Une jambe cassée ralentit, un bras touché fait moins bien viser.
- **Infection** : une morsure est mortelle à terme ; une griffure ou une lacération ne l'est que parfois. Le survivant infecté finit par mourir puis se relève.
- **Fatigue et panique** : la panique dégrade la précision, la fatigue la vitesse.
- **Compétences** qui progressent à l'usage : armes, discrétion, médecine, construction, fouille.
- **Traits** de départ, comme ancien soldat, infirmier, maladroit ou insomniaque.
- **Poids porté** : surchargé, on va moins vite et on fait plus de bruit.

## Zombies

- **Sens** : ils réagissent au bruit (pas, tirs, vitres brisées, moteurs) et à la vue (ligne de vue bloquée par les murs).
- **États** : errance, recherche de l'origine d'un bruit, poursuite, attaque.
- **Types** : marcheur lent, coureur, lourd (qu'un blindé ne peut pas écraser), grimpeur, et hordes migrantes qui traversent les quartiers.
- **Murs** : ils frappent les obstacles et s'empilent contre les murs hauts. Ils visent le point le plus faible, pas le plus proche.

## Contrôles RTS

- Sélection au clic ou par rectangle, groupes de contrôle, déplacement, attaque, attaque en avançant.
- **Allures** : accroupi (silencieux, lent), marche, course (rapide, bruyante).
- **Ordres** : fouiller, ouvrir, barricader une porte ou une fenêtre, soigner un équipier, tenir la position, tirer à vue ou seulement en riposte.
- **Pause active** pour donner des ordres à tout moment.
- **Brouillard de guerre** et vision réelle : on ne voit que ce que l'escouade voit.

## Ce qui viendra plus tard

- Véhicules, avec carburant et bruit.
- Pillards humains contrôlés par l'IA, et armure contre pénétration des munitions.
- Coupure de l'eau et de l'électricité après quelques jours.
- Agriculture et artisanat avancé.
- Multijoueur coopératif, voire compétitif comme Tarkov.

## Premier prototype jouable

Une seule expédition complète, de la planque à la planque.

| Inclus | Pas encore |
| --- | --- |
| Un quartier de quelques pâtés de maisons | Plusieurs quartiers |
| Escouade de 3 survivants, contrôles RTS, pause | Contrôle direct d'un survivant |
| Marcheurs et coureurs, bruit et vue | Grimpeurs, lourds, hordes migrantes |
| Murs et portes destructibles, barricades | Effondrement de bâtiments |
| 10 à 15 conteneurs, inventaire en grille | Échanges, missions |
| 2 sorties, dont une conditionnelle, durée limitée | Nuit de horde sur la planque |
| Morsure, saignement, soins | Santé complète par zone |
| Planque minimale : stock, soins, recrutement, sauvegarde | Modules de planque |

## Décisions à prendre avant de coder

1. **Moteur** : Unity (comme Ebbtide), Godot (comme Withstand The Horde) ou navigateur web.
2. **Rendu** : scène 3D rendue en pixels, comme Ebbtide, ou sprites 2D dessinés. La 3D simplifie les destructions.
3. **Vue** : de dessus, comme le post de mantakustudio, ou isométrique.

Les jeux de référence sont détaillés dans [ANALYSE-REFERENCES.md](ANALYSE-REFERENCES.md).
