# Démarrer le développement

## 1. Préparer ton PC (une seule fois)

1. Installer **Unity Hub**, puis **Unity 6 LTS** (version 6000.x) depuis le Hub.
2. Récupérer le dépôt :
   ```powershell
   git clone https://github.com/La-Dosette/RTS-Zombie.git
   ```
3. Créer le projet Unity :
   1. Dans Unity Hub : **New project**, modèle **Universal 3D**, nom `RTS-Zombie-tmp`, dans un dossier quelconque. Le Hub refuse de créer un projet dans un dossier qui n'est pas vide.
   2. Une fois le projet ouvert, fermer Unity.
   3. Copier les dossiers `Assets`, `Packages` et `ProjectSettings` de `RTS-Zombie-tmp` vers le dossier `RTS-Zombie`.
   4. Dans Unity Hub : **Add → Add project from disk**, choisir le dossier `RTS-Zombie`.
   5. Supprimer `RTS-Zombie-tmp`.
4. Installer **Claude Code** ([instructions](https://code.claude.com/docs)), puis l'ouvrir dans le dossier du projet :
   ```powershell
   cd RTS-Zombie
   claude
   ```
   Claude Code lit automatiquement `CLAUDE.md`, la directive du projet.

## 2. Premier prompt : le socle (J0)

À copier-coller tel quel dans Claude Code :

```text
Tu travailles sur RTS-Zombie, un RTS de survie et d'extraction réaliste
(Project Zomboid + Escape from Tarkov), vue de dessus, rendu 3D pixelisé, sous Unity 6.
Lis d'abord CLAUDE.md, docs/CONCEPT.md et docs/FEUILLE-DE-ROUTE.md.

Objectif de cette session : réaliser le jalon J0 « Socle » de la feuille de route, et rien de plus.

1. Vérifie que le projet Unity est en place à la racine (Assets, Packages, ProjectSettings,
   URP, Input System). S'il manque quelque chose, dis-moi exactement quoi faire dans l'éditeur.
2. Crée l'arborescence et les assembly definitions décrites dans CLAUDE.md, dont
   RtsZombie.Simulation sans référence à UnityEngine.
3. Simulation :
   - grille de 1 m (sol, rue, mur, porte, fenêtre) chargée depuis un plan texte lisible ;
   - quartier de test : trois ou quatre maisons avec pièces, portes et fenêtres, et des rues ;
   - chemin A* en 8 directions sans couper les coins ;
   - un survivant qui suit son chemin à vitesse donnée, sur un tick fixe de 20 Hz.
4. Présentation :
   - construction de la scène depuis la grille, avec des formes simples et des couleurs ;
   - caméra RTS : déplacement au clavier et au bord de l'écran, zoom à la molette,
     rotation par quarts de tour ;
   - rendu pixelisé : rendu en basse résolution, agrandi sans lissage, caméra calée
     sur la grille de pixels pour qu'il n'y ait aucun scintillement en mouvement ;
   - sélection du survivant au clic gauche, déplacement au clic droit, chemin affiché.
   Crée la scène Assets/_Project/Scenes/Sandbox.unity avec un script d'éditeur,
   pas à la main.
5. Tests EditMode : chargement du plan ; A* qui contourne un mur, passe par une porte,
   ne coupe pas un coin, et signale l'absence de chemin quand tout est fermé.
6. Lance les tests en ligne de commande comme indiqué dans CLAUDE.md, et corrige
   jusqu'à ce que tout compile et passe. Si tu ne trouves pas Unity.exe, demande-moi son chemin.
7. Mets à jour docs/FEUILLE-DE-ROUTE.md (cases cochées, écarts) et fais un commit.
8. Explique-moi en français simple quoi tester dans l'éditeur et ce que je dois voir.

Contraintes : aucun asset externe pour l'instant, aucun code ni asset repris d'un autre
jeu, aucune règle de jeu dans un MonoBehaviour.
```

## 3. Prompts suivants : un jalon par session

```text
Lis CLAUDE.md et docs/FEUILLE-DE-ROUTE.md. Réalise le prochain jalon non terminé, et
seulement celui-là : simulation d'abord avec ses tests EditMode, puis présentation.
Appuie-toi sur docs/CONCEPT.md pour les détails. Lance les tests en ligne de commande et
corrige jusqu'à ce qu'ils passent. Mets à jour la feuille de route, fais un commit, puis
explique-moi quoi tester dans l'éditeur et ce que je dois voir.
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
