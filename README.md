# Module Jeu du Yinsh

Implémentation en **Java** du **modèle** du jeu de plateau abstrait **YINSH**, de son **IA Minimax** (élagage Alpha-Bêta) et d'un module de **coordonnées hexagonales**, jouable en **ligne de commande**.

Projet réalisé dans le cadre de la **SAÉ 2.1**, BUT Informatique, Université de Caen Normandie.

## Le jeu en bref
 
YINSH se joue à deux (Blancs et Noirs) sur un plateau hexagonal. Chaque joueur dispose de 5 anneaux.
 
1. À son tour, un joueur place un **pion** de sa couleur dans l'un de ses anneaux, puis **déplace cet anneau** en ligne droite.
2. Les pions franchis par l'anneau sont **retournés**.
3. Quand un joueur aligne **5 pions** de sa couleur, il retire cette ligne ainsi que l'un de ses anneaux.
4. Le premier joueur à retirer **3 anneaux** gagne la partie.
   
## Fonctionnalités
 
- **Modèle complet du jeu** : plateau, pions, anneaux, déplacements, retournement des pions, détection et retrait des lignes, détection du vainqueur.
- **État immuable** : chaque action produit un nouvel état, ce qui facilite l'exploration de l'arbre de jeu par l'IA.
- **IA Minimax avec élagage Alpha-Bêta**, dont la profondeur de recherche est paramétrable.
- **Fonction d'évaluation** prenant en compte les anneaux restants, les lignes formées et la mobilité des anneaux.
- **Deux systèmes de coordonnées hexagonales** : cubiques et doublées.
- **Plusieurs plateaux de départ** : vide, anneaux placés aléatoirement, scénarios de test.
- **Trois programmes en console** : Joueur vs Joueur, Joueur vs IA et IA vs IA.
- **Tests unitaires** JUnit 5 sur le modèle, l'IA et les coordonnées.

## Architecture
 
Le projet est un projet **Maven multi-modules** (Java 22, modules Java `module-info`).

```
.
├── pom.xml                              # POM parent
├── HexagonalCoordinate/                 # Module de coordonnées hexagonales
│   └── src/main/java/iut/gon/coordinate/
│       ├── Coordinate                   # Abstraction d'une coordonnée
│       ├── CoordinateCube               # Coordonnées cubiques
│       ├── CoordinateDoubled            # Coordonnées doublées
│       └── Direction, Mode, Point, DifferentAxisException
└── othello/                             # Modèle du jeu et IA
    └── src/main/java/iut/gon/othello/
        ├── CUIMain.java                 # Joueur vs Joueur (console)
        ├── model/                       # Modèle du jeu
        │   ├── state/                   # État de la partie (IState, State)
        │   ├── actions/                 # Actions : Action, Move, RemoveLine
        │   ├── tokens/                  # Pièces : Token, Pawn, Ring
        │   ├── factory/                 # Génération de plateaux (IFactory, FactoryCube, FactoryDoubled)
        │   └── Model, Team
        └── IA/                          # Intelligence artificielle
            ├── AI, MiniMaxAI, Node, Resultat
            └── IAMain, AIVsAIMain       # Joueur vs IA / IA vs IA (console)
```
>  Le module `othello` porte ce nom pour des raisons historiques (intitulé du sujet) : il contient bien le jeu **YINSH**.

### Patrons de conception utilisés
- **Factory** (`IFactory`, `FactoryCube`, `FactoryDoubled`) : création des plateaux selon le système de coordonnées.
- **State** (`IState`, `State`) : état immuable de la partie, un nouvel état étant produit à chaque action.
- **Strategy** (`AI`, `MiniMaxAI`) : l'algorithme de décision est interchangeable derrière l'interface `AI`.


### Fonction d'évaluation de l'IA
 
| Critère | Effet sur le score |
|---|---|
| Anneaux retirés (adversaire − IA) | ×1000 |
| Lignes de 5 pions disponibles | +50 si c'est le tour de l'IA, −50 sinon |
| Mobilité des anneaux (IA − adversaire) | ×0,5 |

## Prérequis
 
- **JDK 22** ou supérieur
- **Maven 3.8** ou supérieur

## Compilation et lancement
 
```bash
git clone https://github.com/nathtlg/Module-Jeu-du-Yinsh-.git
cd Module-Jeu-du-Yinsh-
 
# Compiler et lancer les tests
mvn clean install
```
 
Lancer ensuite l'un des programmes en console (sous Windows) :
 
```bash
# Joueur vs Joueur
java -cp othello/target/classes:HexagonalCoordinate/target/classes iut.gon.othello.CUIMain
 
# Joueur vs IA (la profondeur de l'IA est demandée au lancement)
java -cp othello/target/classes:HexagonalCoordinate/target/classes iut.gon.othello.IA.IAMain
 
# IA vs IA
java -cp othello/target/classes:HexagonalCoordinate/target/classes iut.gon.othello.IA.AIVsAIMain
```

## Tests
 
```bash
mvn test
```

## Utilisation comme dépendance
 
Ce module est n'est pas utilisé par le projet [Jeu du Yinsh](https://github.com/nathtlg/Jeu-du-Yinsh), qui fournit l'interface graphique et s'appuie sur le modèle, l'IA et les coordonnées d'un enseignant plutôt que celle ci, mais a pour principe de l'être.

## Équipe
 
| Membres |
|---|
| [TOULORGE Nathan](https://github.com/nathtlg) |
| ANGER RENALT Victor |
| BLAISON Nicolas |
| [CONSTANTIN THOMAS](https://github.com/GalaxyShadow99) |
| GAUMONT Gabriel |
 
