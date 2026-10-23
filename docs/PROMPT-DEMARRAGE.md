# Démarrer le développement

L'objectif de départ est la **V1** : une petite carte, une dizaine de zombies et un seul personnage. Elle est décrite en tête de [FEUILLE-DE-ROUTE.md](FEUILLE-DE-ROUTE.md).

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
5. Installer **Codex** d'OpenAI ([dépôt officiel](https://github.com/openai/codex)) et l'ouvrir dans le dossier `RTS-Zombie`.
   - Codex lit automatiquement `AGENTS.md`, la directive du projet.
   - Utiliser Codex sur ton PC, pas dans le cloud : il doit pouvoir lancer Unity et les tests.
   - Claude Code marche aussi : il lit `CLAUDE.md`, qui renvoie vers `AGENTS.md`.

## 2. La V1, en trois sessions

Une session par étape. À la fin de chaque étape, lancer la scène dans Unity, vérifier ce que l'agent décrit, puis passer à l'étape suivante.

### Étape A — Carte, personnage, caméra, rendu

```text
Tu travailles sur RTS-Zombie, un jeu de survie et d'extraction réaliste
(Project Zomboid + Escape from Tarkov), caméra comme Project Zomboid, rendu 3D
pixelisé réaliste, sous Unity 6. Lis d'abord AGENTS.md, docs/CONCEPT.md et
docs/FEUILLE-DE-ROUTE.md.

Objectif de cette session : l'étape A de la V1 dans la feuille de route, et rien de plus.

1. Vérifie que le projet Unity est en place à la racine (Assets, Packages,
   ProjectSettings, URP, Input System). S'il manque quelque chose, dis-moi exactement
   quoi faire dans l'éditeur.
2. Crée l'arborescence, les assembly definitions et les projets dotnet/ décrits dans
   AGENTS.md (simulation en netstandard2.1 et C# 9, tests NUnit lancés avec dotnet test).
3. Simulation :
   - grille de 1 m en chunks de 32 x 32 ; types de case sol, rue, herbe, mur, porte,
     fenêtre, muret, voiture, chacun avec une hauteur d'obstacle et un matériau ;
   - petite carte d'environ 64 x 64 m chargée depuis un plan texte lisible : un
     carrefour, trois maisons avec pièces, portes et fenêtres, un muret, deux voitures,
     une zone de sortie ;
   - tick fixe de 20 Hz ; chaque action est un ordre sérialisable appliqué au tick
     suivant ; la présentation ne lit que des instantanés, via un serveur intégré dans
     le même processus (le jeu sera en ligne plus tard) ;
   - personnage : déplacement au clavier sur les touches physiques (ZQSD en AZERTY,
     WASD en QWERTY), regard vers la souris, marche, course (Maj), accroupi (Ctrl),
     collisions avec la grille, portes à ouvrir et fermer (E).
4. Présentation :
   - scène construite depuis la grille, matériaux réalistes simples (asphalte, herbe,
     brique, bois, béton), lumière avec ombres douces ;
   - caméra comme Project Zomboid : orthographique, isométrique fixe, sans rotation,
     zoom par paliers entiers, qui suit le personnage ;
   - rendu pixelisé avec un facteur réglable, caméra calée sur la grille de pixels pour
     qu'il n'y ait aucun scintillement ; interface en pleine résolution par-dessus.
   Crée la scène Assets/_Project/Scenes/Sandbox.unity avec un script d'éditeur, pas à la main.
5. Tests : chargement du plan, chunks, collisions, portes, ordre sérialisé puis relu à
   l'identique.
6. Lance dotnet test, puis les tests Unity en ligne de commande comme indiqué dans
   AGENTS.md. Corrige jusqu'à ce que tout compile et passe. Si tu ne trouves pas
   Unity.exe, demande-moi son chemin.
7. Coche ce qui est fait dans docs/FEUILLE-DE-ROUTE.md, note les écarts, fais un commit.
8. Explique-moi en français simple quoi tester dans l'éditeur et ce que je dois voir.

Contraintes : aucun asset externe pour l'instant, aucun code ni asset repris d'un autre
jeu, aucune règle de jeu dans un MonoBehaviour.
```

### Étape B — Vision, bruit, zombies

```text
Lis AGENTS.md, docs/CONCEPT.md et docs/FEUILLE-DE-ROUTE.md. L'étape A de la V1 est
faite. Réalise l'étape B de la V1, et rien de plus :

- cône de vision d'environ 150° qui suit la souris, perception proche à 360° ; murs et
  portes fermées bloquent la vue, fenêtres la laissent passer ;
- brouillard à trois états (inconnu, mémorisé, visible) rendu dans les shaders ; toits
  qui masquent les intérieurs ; murs qui s'abaissent devant le personnage ;
- bruit des pas, de la course et des portes, étouffé par les murs, affiché en cercles ;
- 10 à 15 zombies : errance, recherche de l'origine d'un bruit, poursuite, attaque,
  mémoire de la dernière position ; ils défoncent les portes, lentement ;
- marqueurs fantômes des zombies sortis du champ de vision ;
- corps à corps avec une batte (clic gauche) : coup, repousser, coût en endurance.

Simulation d'abord avec ses tests (ligne de vue, cône, mémoire du brouillard,
propagation du bruit, états des zombies), puis présentation. Lance dotnet test et les
tests Unity, corrige jusqu'à ce que tout passe. Coche la feuille de route, fais un
commit, puis explique-moi quoi tester dans l'éditeur et ce que je dois voir.
```

### Étape C — Visée libre, blessures, fouille, sortie

```text
Lis AGENTS.md, docs/CONCEPT.md et docs/FEUILLE-DE-ROUTE.md. Les étapes A et B de la V1
sont faites. Réalise l'étape C de la V1, et rien de plus :

- pistolet en visée libre : clic droit pour viser, clic gauche pour tirer vers le point
  3D sous le curseur, avec le même lancer de rayon pour le curseur et la balle ;
  dispersion affichée par le réticule ; ligne de tir rouge si un obstacle la coupe ;
  un muret couvre un personnage accroupi ; les balles traversent le bois et les
  portes ; tête : mort, corps : chancelle, jambes : tombe ; deux chargeurs seulement ;
  bruit de 40 m qui attire les zombies ;
- blessures simplifiées : zone du corps touchée ; égratignure, lacération ou morsure,
  avec saignement et volume sanguin ; pansement sur une touche ; infection cachée
  pendant la partie (25 % lacération, 7 % égratignure, 100 % morsure), révélée sur
  l'écran de fin ;
- quatre conteneurs à fouiller avec un temps de fouille : pansements, munitions,
  objets de valeur, dans un inventaire en simple liste ;
- sortie : rester 10 secondes dans la zone ; partie de 15 minutes ;
- écran de fin : extrait ou mort, butin ramené, infecté ou non.

Simulation d'abord avec ses tests (trajectoire identique avec la même graine,
couverture par le muret, traversée du bois, zone touchée, saignement, probabilités
d'infection sur un grand nombre de tirages, conditions de sortie), puis présentation.
Lance dotnet test et les tests Unity, corrige jusqu'à ce que tout passe. Coche la
feuille de route, fais un commit, puis explique-moi quoi tester dans l'éditeur et ce
que je dois voir.
```

## 3. Après la V1 : un jalon par session

```text
Lis AGENTS.md et docs/FEUILLE-DE-ROUTE.md. Réalise le prochain jalon non terminé, et
seulement celui-là : simulation d'abord avec ses tests, puis présentation. Appuie-toi sur
docs/CONCEPT.md pour les détails. Lance les tests avec dotnet test puis dans Unity en
ligne de commande, et corrige jusqu'à ce qu'ils passent. Mets à jour la feuille de route,
fais un commit, puis explique-moi quoi tester dans l'éditeur et ce que je dois voir.
```

## 4. Quand quelque chose ne va pas

```text
J'ai testé <l'étape ou le jalon> dans l'éditeur. Voici ce que je vois : <description et
capture>. Voici ce que j'attendais : <description>. Trouve la cause dans la simulation ou
la présentation, ajoute un test qui reproduit le problème si c'est une règle de jeu,
corrige, relance les tests, puis fais un commit.
```

## 5. Conseils

- **Une session par étape.** Si une session devient longue, demander un commit, puis repartir d'une session neuve avec le prompt de l'étape.
- **Tester soi-même dans l'éditeur** à chaque fin d'étape, et répondre avec des captures d'écran : c'est le seul moyen de juger le rendu et les sensations.
- **Changer d'avis est permis** : modifier d'abord `docs/CONCEPT.md` ou `AGENTS.md`, puis demander d'appliquer le changement. La directive reste ainsi la référence.
