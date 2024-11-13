# Projet Calculabilités(Cours M2S1 INALCO) - Générateur Automatique de Pré et Post Conditions pour MTddV - Module 1

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

Dans le Module 1 de notre analyseur syntaxique, la gestion des erreurs est un aspect essentiel qui vise à identifier, enregistrer et signaler les erreurs rencontrées dans le code source d’un programme de la machine de Turing de M. del Vigna (MTddV). Cette gestion permet de s'assurer que le programme respecte les règles syntaxiques spécifiques avant son passage au Module 2, responsable de la génération des pré et post-conditions.

##### Objectifs de la Gestion des Erreurs:
- **Détection proactive des erreurs** : Identifier toute anomalie syntaxique au moment où elle est rencontrée.
- **Précision dans le rapport des erreurs** : Enregistrer le type exact d’erreur et sa position pour permettre un retour clair à l’utilisateur.
- **Robustesse de l’analyse** : Assurer que le programme peut continuer l’analyse, même après une erreur, afin d'identifier toutes les erreurs possibles dans un seul passage.
- **Création d’un rapport d’erreurs** : Générer un fichier ou une structure de données récapitulant toutes les erreurs détectées pour une lecture et une résolution simplifiées.
##### Types d'Erreurs Détectées
1. **Erreurs de Syntaxe des Structures de Contrôle**
Boucles non fermées (boucle sans fin ou sans }) : Chaque bloc boucle doit être correctement clôturé. Une boucle non fermée est signalée comme une erreur avec la position du début de la boucle non clôturée.
Conditions non fermées (si sans }) : Les blocs si doivent également être correctement fermés avec }. Une condition sans fermeture appropriée génère une erreur.
Instructions fin en dehors de boucle : L’instruction fin doit obligatoirement être dans une boucle. Toute instruction fin hors d’un bloc boucle est considérée comme une erreur de syntaxe.
2. **Erreurs dans les Conditions**
Condition mal formée dans si : Une condition doit être représentée par 0 ou 1. Si une autre valeur est rencontrée ou si la syntaxe est incorrecte, cela génère une erreur.
Parenthèses manquantes : Dans les blocs conditionnels, les parenthèses sont nécessaires pour indiquer la condition (si (0) {...}). Si une parenthèse ouvrante ou fermante est manquante, l’erreur est détectée et signalée.
3. **Erreur de Syntaxe Générale**
Instruction inconnue : Si une instruction ne correspond à aucune instruction reconnue (I, P, G, D, 0, 1, si, boucle, fin, #), elle est signalée comme une erreur.
4. **Erreur de Fin de Programme**
Marqueur de fin manquant : Le programme doit se terminer par un #. Si ce symbole est absent ou mal placé, une erreur est signalée.

#### 3. **Interfaces et Formats Utilisés** :
**Interface du Module 1** :
**Entrée** : Le code source du programme MTddV sous forme de chaîne de caractères lue à partir d’un fichier TS.(err_programme_mtddv.ts ou programme_mtddv.ts)
**Sortie** : Une structure de données représentant le programme sous forme de JSON. Le contenu JSON est sauvegardé dans un fichier spécifique dans le dossier `results`(parsed_structure_with_errors.json ou parsed_structure_valid.json)

#### 4. Librairies utilisées et justification des choix 
**re**(expression réugulière): pour la tokenisation et la segmentation du code source. **re** est choisi pour sa flexibilité et sa puissance dans le traitement de la segmentation des tokens de texte complexes mais aussi sa facilité d'utilisation.

**json**: pour la sérialisation de la structure produite sous format JSON. Elle permet d'enregistrer la structure analysée ainsi que les erreurs dans un fichier JSON pour faciliter l’interopérabilité et la lecture par d'autres modules.

#### 5. **Spécifications des Fonctions**

`tokenize()` 
- *Description* : transforme le code source en une liste de jetons
- *Paramètre* : chaîne de caractères du `code`
- *Retour* : liste de tokens

`current_token()`
- *Description* : renvoie le token actuel en cours ou `None` si tous les tokens ont été traités
- *Paramètre* : sans paramètre
- *Retour* : Token actuel

`advance()`
- *Description* : fait avancer l'analyse au token suivant
- *Paramètre* : sans paramètre
- *Retour* : Aucun

`parse()`
- *Description* : analyse la structure globale et renvoie une liste structurée
- *Paramètre* : sans paramètre
- *Retour* : Arborescence JSON et erreurs éventuelles

`parse_instruction()`
- *Description* : analyse une instruction spécifique et retourne sa représentation
- *Paramètre* : sans paramètre
- *Retour* : Dictionnaire représentant l’instruction ou une erreur

`parse_condition()`
- *Description* : traite les conditions dans les instructions `si`
- *Paramètre* : sans paramètre
- *Retour* : Condition sous forme de chaîne ou erreur si mal formée

## Structure des fichiers du projet

**analyseur_syntaxique.ipynb** :

C’est un notebook Jupyter qui contient le code Python pour l’analyseur syntaxique du module 1. Ce fichier sert de support pour l’exécution et la mise en forme du code de parsing, ainsi que pour l’analyse des programmes écrits pour la machine de Turing de M. del Vigna (MTddV). Dans ce fichier, vous trouverez le code de parsing (comme celui que nous avons décrit précédemment) ainsi que les tests et la documentation liés.

**err_programme_mtddv.ts** :

Ce fichier .ts contient un programme MTddV qui présente des erreurs syntaxiques ou de structure. Il sert de cas de test pour vérifier que l’analyseur syntaxique est capable de détecter les erreurs et de les rapporter correctement dans le module d’extraction et d’écriture. L’utilisation de ce fichier aide à vérifier que l’analyseur remplit bien ses fonctions de détection d’erreurs.

**programme_mtddv.ts** :

Ce fichier .ts contient un autre programme MTddV, sans erreurs syntaxiques ou structurelles. Ce programme sert d’exemple de cas correct pour valider que l’analyseur syntaxique peut interpréter correctement un code MTddV valide. L’analyseur devrait être capable de parser ce fichier sans générer d’erreurs, et le résultat devrait être sauvegardé dans un fichier indiquant un parsing sans erreurs.

**README.md** :

Ce fichier est un document en Markdown qui fournit probablement des explications et des instructions sur le projet. Il peut contenir des informations sur la configuration, les objectifs, la structure du code, et la façon d’utiliser l’analyseur syntaxique. Le fichier README.md est souvent essentiel dans un projet car il aide les utilisateurs et les développeurs à comprendre le fonctionnement et l’objectif du code.

**results** :

Ce dossier contient les fichiers de sortie générés par l’analyseur syntaxique. En fonction de la présence ou non d’erreurs dans les fichiers analysés (err_programme_mtddv.ts ou programme_mtddv.ts), l’analyseur crée des fichiers de sortie au format JSON (parsed_structure_with_errors.json ou parsed_structure_valid.json) qui détaillent la structure analysée et les éventuelles erreurs rencontrées.
