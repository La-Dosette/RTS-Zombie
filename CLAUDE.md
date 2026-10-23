# RTS-Zombie : directive de développement

RTS de survie et d'extraction réaliste, en PvPvE, dans une ville américaine infestée de zombies. Chaque joueur commande une escouade de 1 à 5 survivants, avec la vision, le bruit et les blessures de Project Zomboid et la boucle d'expédition d'Escape from Tarkov. Unity 6, rendu 3D pixelisé, caméra comme Project Zomboid.

À lire avant tout travail :
- [docs/CONCEPT.md](docs/CONCEPT.md) : ce que le jeu doit être ;
- [docs/FEUILLE-DE-ROUTE.md](docs/FEUILLE-DE-ROUTE.md) : les jalons et leur état ;
- [docs/ANALYSE-REFERENCES.md](docs/ANALYSE-REFERENCES.md) : les jeux de référence.

## Choix fixés

- Unity 6 LTS (6000.x), Universal Render Pipeline, Input System, C#.
- Projet Unity à la racine du dépôt : `Assets/`, `Packages/`, `ProjectSettings/`.
- **Caméra comme Project Zomboid** :
  - orthographique, angle isométrique fixe, sans rotation ;
  - zoom par paliers entiers, pour garder des pixels nets ;
  - suit l'escouade ou le survivant pris en main ;
  - murs et toits s'effacent autour des survivants.
- **Rendu** 3D en basse résolution, agrandi sans lissage.
  - Commencer simple : la caméra rend dans une RenderTexture en filtre Point, affichée en plein écran.
  - Caler la caméra sur la grille de pixels pour éviter le scintillement.
  - Passer à un Renderer Feature (Render Graph) seulement si c'est nécessaire.
- **Grille** de 1 m.
  - Un seul niveau pour l'instant ; les étages viendront plus tard.
  - Chaque case a une hauteur d'obstacle : un muret ou une voiture couvre à mi-hauteur, un mur entièrement.
- **Grande carte** : environ 1 km² par quartier (≈ 1 000 × 1 000 cases), pour des expéditions de 60 à 90 minutes.
  - Grille découpée en chunks de 32 × 32 cases dès J0.
  - Vision, bruit, chemins et IA ne travaillent que sur les chunks utiles.
  - Les zombies éloignés sont simulés en version allégée.
- **Visée libre** : la balistique est calculée dans la simulation, en 2,5D (position au sol plus hauteur).
  - Un lancer de rayon maison traverse la grille (hauteurs d'obstacle, matériaux) et les zones de touche des corps.
  - Le curseur et la balle utilisent ce même lancer de rayon : ce que le curseur désigne est ce que la balle visera.
  - La physique Unity ne sert qu'aux effets visuels, comme les débris, jamais à une règle de jeu.
- Interface et réticule en pleine résolution, au-dessus de la scène pixelisée.
- **PvPvE avec serveur qui fait autorité** :
  - 8 à 12 escouades de joueurs par expédition (jusqu'à 60 survivants), plus des pillards IA et environ 500 zombies, à régler ;
  - pas de pause en expédition ;
  - la planque de chaque joueur est une instance PvE.

## Architecture

```
Assets/_Project/
  Scripts/Simulation/   RtsZombie.Simulation.asmdef (noEngineReferences: true)
    Map/ Vision/ Noise/ Agents/ Health/ Combat/ Inventory/ Raid/ Base/ Commands/ Core/
  Scripts/Game/         RtsZombie.Game.asmdef (référence Simulation)
    Bootstrap/ Rendering/ CameraRig/ Input/ UI/ Views/ Data/ Net/
  Scripts/Editor/       RtsZombie.Editor.asmdef (Editor uniquement)
  Data/                 ScriptableObjects : objets, armes, blessures, zombies, matériaux
  Materials/ Shaders/ Prefabs/ Scenes/ Art/
  Tests/EditMode/       RtsZombie.Tests.EditMode.asmdef
dotnet/                 projets .NET hors Unity
  RtsZombie.Simulation.csproj        netstandard2.1, LangVersion 9 : compile Scripts/Simulation
  RtsZombie.Simulation.Tests.csproj  net8.0, NUnit : compile Tests/EditMode, lancé avec dotnet test
  RtsZombie.Server.csproj            net8.0 : serveur dédié (à partir du jalon réseau)
```

### Simulation

Du C# pur, sans `UnityEngine` ni `MonoBehaviour`. Toutes les règles du jeu sont là : grille, vision, bruit, IA, blessures, combat, inventaire, expéditions, planque.

- Elle doit compiler à la fois dans Unity et dans `dotnet/RtsZombie.Simulation.csproj`. Donc C# 9 au maximum, comme dans Unity 6, et uniquement l'API .NET Standard 2.1. Pas de `namespace` de fichier, pas de `record struct`, pas de `global using`, pas de `System.Text.Json`.
- Les tests de simulation n'utilisent que NUnit, sans rien de Unity, pour tourner aussi avec `dotnet test`.
- Tick fixe de 20 Hz, indépendant du rendu.
- Générateur aléatoire maison à graine (PCG32 ou xorshift), jamais `UnityEngine.Random` ni `System.Random`. Même graine et mêmes ordres donnent le même résultat.
- **Tout passe par des ordres** :
  - chaque action d'un joueur est un ordre sérialisable (dossier `Commands/`), appliqué par la simulation au tick suivant ;
  - la présentation ne lit que des instantanés de l'état ;
  - en local, un serveur intégré tourne dans le même processus. Le passage au réseau ne change donc aucune règle.
- **Perception** : pour chaque joueur, la simulation calcule ce que son escouade voit et entend. C'est la seule chose qui lui sera transmise.

### Présentation

Elle lit les instantanés, les affiche et les interpole, puis envoie des ordres. Aucune règle de jeu dans un `MonoBehaviour`.

### Réseau (à partir du jalon réseau)

- Le serveur fait autorité : la simulation tourne sur le serveur, et le client ne décide jamais d'un état de jeu.
- Le serveur n'envoie à chaque joueur que ce que son escouade perçoit, pour qu'aucune triche ne révèle ce qui n'a pas été transmis.
- Côté client : interpolation des entités, prédiction locale du déplacement en prise en main.
- Compensation de latence pour les tirs : le serveur replace les zones de touche là où le tireur les voyait, dans une limite de temps.
- Déconnexion involontaire : l'escouade reste dans le monde et passe en défense automatique (accroupie, tient sa position, riposte) jusqu'à la reconnexion.
- Transport à choisir au jalon réseau. Piste : LiteNetLib (licence MIT) des deux côtés, puisque le serveur est un programme .NET simple.

### Données

Valeurs d'équilibrage dans des ScriptableObjects (`Data/`), converties en définitions C# simples pour la simulation. Le serveur les lit depuis un export JSON produit par un script d'éditeur. Aucun nombre magique d'équilibrage dans le code.

### Persistance

- Comptes, survivants, stock et planque sont stockés côté serveur. Le client n'est jamais cru sur parole.
- L'état ne fait qu'avancer : jamais de retour à un état antérieur.
- Quitter volontairement une expédition, c'est l'abandonner : les survivants sont portés disparus.

### Temps

- En expédition, le temps de jeu avance à la vitesse réelle, sans pause.
- À la planque, il avance par blocs : repos, soins, nuit.
- Les durées de guérison et d'infection sont exprimées en heures de jeu.

### Performances

- Zombies stockés en tableaux de structures dans la simulation, et pas d'allocation par tick dans les boucles chaudes.
- Rendu instancié quand il y a des centaines d'entités.
- Jobs et Burst côté client, puis DOTS, seulement si une mesure le justifie.
- Serveur : viser moins de 10 ms par tick pour une expédition complète, afin d'en faire tourner plusieurs par machine.

## Règles de travail

- Avancer **jalon par jalon** selon la feuille de route. Ne pas commencer le suivant tant que le précédent n'est pas validé.
- **Simulation d'abord**, avec ses tests, puis la présentation.
- Ne jamais écrire à la main de YAML Unity (`.unity`, `.prefab`, `.asset`). Créer scènes, prefabs, matériaux et ScriptableObjects avec des scripts d'éditeur, dans un menu `RTS-Zombie/…`, ou par code au démarrage. Le résultat doit pouvoir être recréé.
- Toujours committer les fichiers `.meta`.
- Code, noms et commentaires en anglais. Textes visibles par le joueur en français, regroupés dans une table de textes.
- Réalisme avant confort, mais tout mécanisme caché doit laisser un indice visible au joueur : symptôme, son, marqueur.
- Aucun code ni asset repris d'un autre jeu, Withstand The Horde compris (son dépôt n'a pas de licence). Aucun asset sous licence sans noter sa source et sa licence dans `Assets/_Project/Art/SOURCES.md`.
- Formes simples et couleurs tant que la mécanique n'est pas validée. L'art vient après.

## Lancer les tests

Tests de simulation, rapides, sans fermer Unity. Le .NET 8 SDK est nécessaire :

```powershell
dotnet test dotnet/RtsZombie.Simulation.Tests.csproj
```

Tests dans Unity, qui vérifient aussi que tout le projet compile. L'éditeur doit être fermé, car Unity verrouille le projet. Sous Windows, dans PowerShell, à la racine du dépôt :

```powershell
& "C:\Program Files\Unity\Hub\Editor\<version>\Editor\Unity.exe" -batchmode -projectPath . -runTests -testPlatform EditMode -testResults TestResults/editmode.xml -logFile TestResults/unity.log
```

Lire `TestResults/editmode.xml` pour les résultats et `TestResults/unity.log` pour les erreurs de compilation. Remplacer `<version>` par la version installée, visible dans Unity Hub.

## Un jalon est terminé quand

1. Le projet compile sans erreur et sans nouveau warning dans `Assets/_Project`, dans Unity comme avec `dotnet build`.
2. Tous les tests passent, avec `dotnet test` comme dans Unity en ligne de commande.
3. La scène `Assets/_Project/Scenes/Sandbox.unity` permet de vérifier chaque critère du jalon dans l'éditeur.
4. La feuille de route est à jour : cases cochées, écarts et décisions notés.
5. Un commit décrit ce qui a été fait.

À la fin de chaque session, expliquer à l'utilisateur en français simple ce qu'il doit tester dans l'éditeur, et ce qu'il doit voir.
