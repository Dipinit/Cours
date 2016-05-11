**Cours THL & Révisions C**

[TOC]

# Introduction
## Séances à venir
Il y aura un total de **4 séances** de soutien. La première n'inclut pas de révisions en C. Il y aura une sorte de shell facultatif à faire à l'issu de ces séances,. Ce dernier sera noté et pourra faire l'objet de défense pour valider le module. C'est avant tout du soutien: si vous avez des lacunes: **venez** et **posez vos questions**. Des exercices en plus du shell seront proposés (appels système, gestion de signaux, etc...) et apporteront une note de soutien, encore une fois: là pour vous défendre lors de votre passage devant le jury.

## Outils / Support
Le PDF (normalement destiné au TEK 3) sera disponible.

# Parser
## Réalisation
**Objectif**: Obtenir un parser rapidement et sans outil peu importe le langage.

**Étapes**:
 - Faire un BNF
 - Comprendre la grammaire a utiliser
 - Le traduire dans le langage désiré

**L'utilisation d'un BNF**: permet de décrire le comportement du pointeur interne qu'on utilisera pour lire/consommer les caractères/se déplacer. On se base sur un mécanisme de limitation: on s'impose des règles basiques afin de simplifier notre algorithme.

**Logique**: chaque règle de mon BNF est une méthode. Pour toute règle qui ne passe, je dois backtracker mon pointeur au point d'origine afin de tester les autres règles.

**Exemple de syntaxe**:
Nom de Règle se trouve avant un ```::=```  
```
Rule ::= ...
```
Clause se trouve après un ```::=```  
```
Rule ::= Bar | Foo
```
Une Règle se termine par un ```;```  
```
Rule ::= Bar | Foo;
```
Un ```'a'``` correspond à une lecture d'un ```'a'```

```
Bar ::= 'a' 'b' 'c'; // Pour "abc"
```

```|``` correspond bien à un "ou"  
```
Foo ::= 'a' 'b' | 'c' 'd'; // Pour "ab" ou "cd"
Foo ::= 'a' [ 'b' | 'c' ] 'd'; // Pour "abd" ou "acd"
```  

Utilisation de répéteurs:

- ```?``` : 0-1
- ```*``` : 0-N
- ```+``` : 1-N

```
Char ::= 'a'..'z'; // Pour char entre 'a' et 'z'
List ::= Char [',' Char]*; // Pour liste (ex: "z,c,d,e,e,a")
```
**Attention**:  
Avec l'utilisation de ```*``` dans certaines règles, si on retourne constamment ```true```:  il faut veiller à ne pas former une boucle infinie.

**Info**:
Des macros peuvent être utilisés pour les répéteurs. 

### Descent Recursive Parser
Exemple d'approche top-down pour définir l'expression d'un langage:
```
language ::= statement+;
statement ::= expr ':';
alpha ::= ['a'..'z'|'A'..'Z']+;
num ::= '0'..'9'+;
expr ::= alpha '=' add;
add ::= mul ['+' mul]*;
mul ::= prim ['*' prim]*;
prim ::= alpha | num | '(' add ')';
```

À savoir sur les BNF:  

- Chaque règle est une méthode de type ```boolean```
- Une clause peut appeler une autre règle.
- Voir Slides pour le reste

### Conclusion: Descent Recursive Parser
- Pas le plus rapide ni le plus intelligent mais il est facile à faire
- Peut être écrit à la main

### Top-down et bottom-up

Autre exemple pour définir une liste en **top-down**:

```
list ::= item [separator list]*;
```

En **bottom-up** (producteur/consommateur): tokenization avec système de stack (dépilage des expressions)

<!--Ce qui va être expliqué et utilisé: PEG (packrat, top down). -->