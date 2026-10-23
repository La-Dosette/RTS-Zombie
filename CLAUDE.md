# RTS-Zombie : directive de développement

RTS de survie et d'extraction réaliste dans une ville infestée de zombies. On commande une escouade de survivants, avec la vision, le bruit et les blessures de Project Zomboid et la boucle d'expédition d'Escape from Tarkov. Unity 6, rendu 3D pixelisé, vue de dessus. Solo contre l'environnement pour commencer.

À lire avant tout travail :
- [docs/CONCEPT.md](docs/CONCEPT.md) : ce que le jeu doit être ;
- [docs/FEUILLE-DE-ROUTE.md](docs/FEUILLE-DE-ROUTE.md) : les jalons et leur état ;
- [docs/ANALYSE-REFERENCES.md](docs/ANALYSE-REFERENCES.md) : les jeux de référence.

## Choix fixés

- Unity 6 LTS (6000.x), Universal Render Pipeline, Input System, C#.
- Projet Unity à la racine du dépôt : `Assets/`, `Packages/`, `ProjectSettings/`.
- Rendu 3D en basse résolution, agrandi sans lissage. Commencer simple : la caméra rend dans une RenderTexture en filtre Point, affichée en plein écran. Caler la caméra sur la grille de pixels pour éviter le scintillement. Passer à un Renderer Feature (Render Graph) seulement si c'est nécessaire.
- Grille de 1 m. Un seul niveau pour l'instant ; les étages viendront plus tard.

## Architecture

```
Assets/_Project/
  Scripts/Simulation/   RtsZombie.Simulation.asmdef (noEngineReferences: true)
    Map/ Vision/ Noise/ Agents/ Health/ Combat/ Inventory/ Raid/ Base/ Core/
  Scripts/Game/         RtsZombie.Game.asmdef (référence Simulation)
    Bootstrap/ Rendering/ CameraRig/ Input/ UI/ Views/ Data/
  Scripts/Editor/       RtsZombie.Editor.asmdef (Editor uniquement)
  Data/                 ScriptableObjects : objets, armes, blessures, zombies, matériaux
  Materials/ Shaders/ Prefabs/ Scenes/ Art/
  Tests/EditMode/       RtsZombie.Tests.EditMode.asmdef
```

- **Simulation** : du C# pur, sans `UnityEngine` ni `MonoBehaviour`. Toutes les règles du jeu sont là : grille, vision, bruit, IA, blessures, combat, inventaire, expéditions, planque.
  - Tick fixe de 20 Hz, indépendant du rendu.
  - Générateur aléatoire maison à graine (PCG32 ou xorshift), jamais `UnityEngine.Random` ni `System.Random`.
  - Même graine et mêmes ordres donnent le même résultat.
- **Présentation** : elle lit l'état de la simulation, l'affiche et l'interpole, puis lui envoie des ordres. Aucune règle de jeu dans un `MonoBehaviour`.
- **Données** : valeurs d'équilibrage dans des ScriptableObjects (`Data/`), converties au démarrage en définitions C# simples pour la simulation. Aucun nombre magique d'équilibrage dans le code.
- **Performances** :
  - zombies stockés en tableaux de structures dans la simulation ;
  - pas d'allocation par image dans les boucles chaudes ;
  - rendu instancié quand il y a des centaines d'entités ;
  - Jobs et Burst, puis DOTS, seulement si une mesure le justifie.
- **Temps** :
  - en expédition, le temps de jeu avance à la vitesse réelle, hors pause et ralenti ;
  - à la planque, il avance par blocs : repos, soins, nuit.
  - Les durées de guérison et d'infection sont exprimées en heures de jeu.

## Règles de travail

- Avancer **jalon par jalon** selon la feuille de route. Ne pas commencer le suivant tant que le précédent n'est pas validé.
- **Simulation d'abord**, avec ses tests EditMode, puis la présentation.
- Ne jamais écrire à la main de YAML Unity (`.unity`, `.prefab`, `.asset`). Créer scènes, prefabs, matériaux et ScriptableObjects avec des scripts d'éditeur, dans un menu `RTS-Zombie/…`, ou par code au démarrage. Le résultat doit pouvoir être recréé.
- Toujours committer les fichiers `.meta`.
- Code, noms et commentaires en anglais. Textes visibles par le joueur en français, regroupés dans une table de textes.
- Réalisme avant confort, mais tout mécanisme caché doit laisser un indice visible au joueur : symptôme, son, marqueur.
- Aucun code ni asset repris d'un autre jeu, Withstand The Horde compris (son dépôt n'a pas de licence). Aucun asset sous licence sans noter sa source et sa licence dans `Assets/_Project/Art/SOURCES.md`.
- Formes simples et couleurs tant que la mécanique n'est pas validée. L'art vient après.

## Lancer les tests

L'éditeur doit être fermé : Unity verrouille le projet. Sous Windows, dans PowerShell, à la racine du dépôt :

```powershell
& "C:\Program Files\Unity\Hub\Editor\<version>\Editor\Unity.exe" -batchmode -projectPath . -runTests -testPlatform EditMode -testResults TestResults/editmode.xml -logFile TestResults/unity.log
```

Lire `TestResults/editmode.xml` pour les résultats et `TestResults/unity.log` pour les erreurs de compilation. Remplacer `<version>` par la version installée, visible dans Unity Hub.

## Un jalon est terminé quand

1. Le projet compile sans erreur et sans nouveau warning dans `Assets/_Project`.
2. Tous les tests EditMode passent en ligne de commande.
3. La scène `Assets/_Project/Scenes/Sandbox.unity` permet de vérifier chaque critère du jalon dans l'éditeur.
4. La feuille de route est à jour : cases cochées, écarts et décisions notés.
5. Un commit décrit ce qui a été fait.

À la fin de chaque session, expliquer à l'utilisateur en français simple ce qu'il doit tester dans l'éditeur, et ce qu'il doit voir.
