# Démarrer le développement

## 1. Préparer ton PC (une seule fois)

1. Installer **Unity Hub**, puis **Unity 6 LTS** (version 6000.x) depuis le Hub.
2. Installer le **.NET 8 SDK** ([téléchargement](https://dotnet.microsoft.com/download/dotnet/8.0)). Il sert à tester les règles du jeu sans Unity, et plus tard au serveur.
3. Récupérer le dépôt :
   ```powershell
   git clone https://github.com/La-Dosette/RTS-Zombie.git
   ```
4. Créer le projet Unity :
   1. Dans Unity Hub : **New project**, modèle **Universal 3D**, nom `RTS-Zombie-tmp`, dans un dossier quelconque. Le Hub refuse de créer un projet dans un dossier qui n'est pas vide.
   2. Une fois le projet ouvert, fermer Unity.
   3. Copier les dossiers `Assets`, `Packages` et `ProjectSettings` de `RTS-Zombie-tmp` vers le dossier `RTS-Zombie`.
   4. Dans Unity Hub : **Add → Add project from disk**, choisir le dossier `RTS-Zombie`.
   5. Supprimer `RTS-Zombie-tmp`.
5. Installer **Claude Code** ([instructions](https://code.claude.com/docs)), puis l'ouvrir dans le dossier du projet :
   ```powershell
   cd RTS-Zombie
   claude
   ```
   Claude Code lit automatiquement `CLAUDE.md`, la directive du projet.

## 2. Premier prompt : le socle (J0)

À copier-coller tel quel dans Claude Code :

```text
Tu travailles sur RTS-Zombie, un RTS de survie et d'extraction réaliste en PvPvE
(Project Zomboid + Escape from Tarkov), caméra comme Project Zomboid, rendu 3D pixelisé,
sous Unity 6. Lis d'abord CLAUDE.md, docs/CONCEPT.md et docs/FEUILLE-DE-ROUTE.md.

Objectif de cette session : réaliser le jalon J0 « Socle » de la feuille de route, et rien de plus.

1. Vérifie que le projet Unity est en place à la racine (Assets, Packages, ProjectSettings,
   URP, Input System). S'il manque quelque chose, dis-moi exactement quoi faire dans l'éditeur.
2. Crée l'arborescence et les assembly definitions décrites dans CLAUDE.md, dont
   RtsZombie.Simulation sans référence à UnityEngine, ainsi que les projets dotnet/
   (simulation en netstandard2.1 et C# 9, tests NUnit lancés avec dotnet test).
3. Simulation :
   - grille de 1 m découpée en chunks de 32 x 32 cases, prévue pour un futur quartier
     d'environ 1 km² ; types de case sol, rue, mur, porte, fenêtre, muret, chacun avec
     une hauteur d'obstacle (la visée libre en aura besoin) ;
   - plan chargé depuis un fichier texte lisible ;
   - quartier de test : trois ou quatre maisons avec pièces, portes et fenêtres, et des rues ;
   - chemin A* en 8 directions sans couper les coins ;
   - un survivant qui suit son chemin à vitesse donnée, sur un tick fixe de 20 Hz ;
   - le déplacement est un ordre sérialisable appliqué au tick suivant, et la présentation
     ne lit que des instantanés, via un serveur intégré dans le même processus. Le jeu
     sera en ligne (PvPvE) : cette séparation doit exister dès maintenant.
4. Présentation :
   - construction de la scène depuis la grille, avec des formes simples et des couleurs ;
   - caméra comme Project Zomboid : orthographique, isométrique fixe, sans rotation,
     zoom par paliers entiers, déplacement au clavier et au bord de l'écran ;
   - rendu pixelisé : rendu en basse résolution, agrandi sans lissage, caméra calée
     sur la grille de pixels pour qu'il n'y ait aucun scintillement en mouvement ;
     interface en pleine résolution par-dessus ;
   - sélection du survivant au clic gauche, déplacement au clic droit, chemin affiché.
   Crée la scène Assets/_Project/Scenes/Sandbox.unity avec un script d'éditeur,
   pas à la main.
5. Tests : chargement du plan et découpage en chunks ; A* qui contourne un mur, passe par
   une porte, ne coupe pas un coin, et signale l'absence de chemin quand tout est fermé ;
   un ordre sérialisé puis relu produit le même déplacement.
6. Lance les tests avec dotnet test, puis dans Unity en ligne de commande, comme indiqué
   dans CLAUDE.md. Corrige jusqu'à ce que tout compile et passe. Si tu ne trouves pas
   Unity.exe, demande-moi son chemin.
7. Mets à jour docs/FEUILLE-DE-ROUTE.md (cases cochées, écarts) et fais un commit.
8. Explique-moi en français simple quoi tester dans l'éditeur et ce que je dois voir.

Contraintes : aucun asset externe pour l'instant, aucun code ni asset repris d'un autre
jeu, aucune règle de jeu dans un MonoBehaviour.
```

## 3. Prompts suivants : un jalon par session

```text
Lis CLAUDE.md et docs/FEUILLE-DE-ROUTE.md. Réalise le prochain jalon non terminé, et
seulement celui-là : simulation d'abord avec ses tests, puis présentation. Appuie-toi sur
docs/CONCEPT.md pour les détails. Lance les tests avec dotnet test puis dans Unity en
ligne de commande, et corrige jusqu'à ce qu'ils passent. Mets à jour la feuille de route,
fais un commit, puis explique-moi quoi tester dans l'éditeur et ce que je dois voir.
```

## 4. Quand quelque chose ne va pas

```text
J'ai testé le jalon <Jx> dans l'éditeur. Voici ce que je vois : <description et capture>.
Voici ce que j'attendais : <description>. Trouve la cause dans la simulation ou la
présentation, ajoute un test qui reproduit le problème si c'est une règle de jeu,
corrige, relance les tests, puis fais un commit.
```

## 5. Conseils

- **Une session par jalon.** Si une session devient longue, demander un commit, puis repartir d'une session neuve avec le prompt de la section 3.
- **Tester soi-même dans l'éditeur** à chaque fin de jalon, et répondre avec des captures d'écran : c'est le seul moyen de juger le rendu et les sensations.
- **Changer d'avis est permis** : modifier d'abord `docs/CONCEPT.md` ou `CLAUDE.md`, puis demander d'appliquer le changement. La directive reste ainsi la référence.
