# Projet Calculabilités - Générateur Automatique de Pré et Post Conditions pour MTddV - Module 1

## Objectif général du projet

Le projet a pour objectif de réaliser un générateur automatique de pré et post-conditions pour un programme de la machine de Turing de M. del Vigna (MTddV). Pour cela, il faut développer un **Module 1**, qui sera responsable de l’analyse syntaxique d’un programme écrit en MTddV. Ce module doit reconnaître la syntaxe du programme, la transformer en une structure de données représentative (arborescente), puis transmettre cette structure au **Module 2**, qui se chargera de la génération d'assertions (pré et post conditions).

Dans l'équipe de Xinlei et Léa, nous nous occupons principalement du Module 1.

## Module 1 : Analyseur Syntaxique

### Description générale du module 1

Le Module 1 devra prendre un fichier contenant le programme MTddV, l'analyser et produire une structure arborescente qui représente la syntaxe de ce programme. Cette structure sera ensuite transmise au Module 2 pour générer les pré et post conditions.

### Objectifs du Module 1
- **Reconnaître la syntaxe** d’un programme spécifique à la Machine de Turing de M. del Vigna (MTddV).
- **Extraire les instructions** et organiser ces instructions dans une **structure arborescente** représentant la hiérarchie du programme (niveaux d’imbrication des boucles, des conditions, etc.).
- **Gérer les erreurs syntaxiques** : rapporter les erreurs comme les blocs non fermés, les instructions non reconnues, ou les erreurs de syntaxe dans les structures conditionnelles ou itératives.


### Architecture du Module 1

#### 1. **Analyseur Syntaxique (Parser)**

##### Fonctionnement :
Le module doit effectuer un **parsing** du code source, c'est-à-dire qu'il doit :
1. **Tokeniser le code source** : Diviser le code en éléments de base (tokens).
2. **Analyser les tokens** : Vérifier que les tokens sont conformes à la syntaxe définie du langage MTddV.
3. **Gérer les structures imbriquées** : Identifier les structures comme les boucles `boucle`, les conditions `si` et leurs niveaux d’imbrication.

##### Stratégie d'implémentation :
- **Tokenisation** : Utiliser des expressions régulières pour découper le texte source en tokens (mots, symboles, etc.).
- **Analyse syntaxique** : Une fois les tokens extraits, un analyseur va les parcourir et valider la structure du programme.
- **Représentation arborescente** : Une fois l’analyse effectuée, le programme générera une structure arborescente représentant l’organisation du programme, c'est-à-dire une liste des instructions avec leurs niveaux d'imbrication.
  
  L’arbre doit inclure des informations comme :
  - **Les instructions de base** : `I`, `P`, `G`, `D`, `0`, `1`.
  - **Les structures conditionnelles et itératives** : `si`, `boucle`.
  - **Les erreurs rencontrées** : Si le programme a des erreurs (comme des boucles non fermées), celles-ci doivent être rapportées.

#### 2. **Gestion des erreurs** :

Lors de l'analyse, le module détecte les erreurs courantes:
- **Bloc non fermés**: Boucles ou conditions sans fermetures appropriées.
- **Conditions mal formées** dans la structure "si"

#### 3. **Interfaces et Formats Utilisés** :
**Interface du Module 1** :
**Entrée** : Le code source du programme MTddV sous forme de chaîne de caractères lue à partir d’un fichier.
**Sortie** : Une structure de données représentant le programme sous forme de JSON.

#### 4. Librairies utilisées et justification des choix 
**re**(expression réugulière): pour la tokenisation et la segmentation du code source. **re** est choisi pour sa flexibilité et sa puissance dans le traitement de la segmentation des tokens de texte complexes mais aussi sa facilité d'utilisation.

**json**: pour la sérialisation de la structure produite sous format JSON. Elle permet d'enregistrer la structure analysée ainsi que les erreurs dans un fichier JSON pour faciliter l’interopérabilité et la lecture par d'autres modules.

#### 5. **Spécifications des Fonctions**

`tokenize()` 
*Description* : transforme le code source en une liste de jetons *Paramètre* : chaîne de caractères du `code`
*Retour* : liste de tokens

`current_token()`
*Description* : renvoie le token actuel en cours ou `None` si tous les tokens ont été traités
*Paramètre* : sans paramètre
*Retour* : Token actuel

`advance()`
*Description* : fait avancer l'analyse au token suivant
*Paramètre* : sans paramètre
*Retour* : Aucun

`parse()`
*Description* : analyse la structure globale et renvoie une liste structurée
*Paramètre* : sans paramètre
*Retour* : Arborescence JSON et erreurs éventuelles

`parse_instruction()`
*Description* : analyse une instruction spécifique et retourne sa représentation
*Paramètre* : sans paramètre
*Retour* : Dictionnaire représentant l’instruction ou une erreur

`parse_condition()`
*Description* : traite les conditions dans les instructions `si`
*Paramètre* : sans paramètre
*Retour* : Condition sous forme de chaîne ou erreur si mal formée

