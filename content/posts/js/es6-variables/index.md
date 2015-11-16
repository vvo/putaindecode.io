---
date: "2015-11-15"
title: "ES6 : les variables"
tags:
  - javascript
  - ES6
  - ES2015
authors:
  - Nyalab
header:
  credit: https://www.flickr.com/photos/4everyoung/2505890793/
  linearGradient: 160deg, rgb(204, 51, 51), rgba(204, 51, 51, .6)
---

# Introduction

ES6 (appelé aussi ES2015) est la nouvelle version majeure du langage javascript. Elle apporte un grand nombre de fonctionnalités toutes plus utiles les unes que les autres, si toutefois on sait faire le tri parmi elles. Je vais me limiter pour cet article à vous exposer les nouveautés concernant les variables : leurs déclarations, leurs cas d'utilisation, ainsi que leurs pièges et leurs subtilités.

# let, const, var

ES6 introduit 2 nouvelles manières de déclarer vos variables: `const`, qui permet de déclarer des variables qui peuvent être assignées une seule et unique fois et qui sont block scopées ; et `let`, qui est l'équivalent de `var`, mais version block scopée. `var` est toujours disponible à l'utilisation, même si ses cas d'usages sont extrêmement limités voire inexistants.

## const

`const` vous permet donc de déclarer une variable block scopée à assignation unique. Ne vous méprenez pas si vous lisez le terme constante pour évoquer `const`, ce ne sont pas des vraies constantes au sens valeur, mais des constantes au sens référence.

```javascript
function fn(arg) {
  const foo = "bar"
  foo = "baz" // TypeError

  const arg = "argument" // SyntaxError

  const bar // SyntaxError

  const scope = "parent"
  if(true) {
    const scope = "child"
    console.log(scope) // "child"
  }
  console.log(scope) // "parent"
}
```

## let

## Utilisations, erreurs et pièges

Simplement : utilisez `const` quasiment tout le temps, `let` quand vous êtes obligés, et `var` quand vous savez exactement pourquoi vous en avez besoin. ES5 nous a donné de mauvaises habitudes et ne nous forçait pas à rélféchir à nos déclarations de variables, changeons ça de suite.

Pour ce qui est des différents types d'erreurs possibles :
* `const` doit être initialisé avec une valeur (tout simplement, vous devez avoir un signe = après le nom de la variable déclarée avec const)
* une `const` ne peut pas être réassignée
* déclarer 2 fois une variable avec let dans un même scope lève une TypeError, mais pas dans le corps d'une fonction
* let et const ne sont pas [hoistés](#hoisting,-tdz)

```javascript
let foo
let foo // TODO TypeError, la variable "foo" a déjà été déclarée  

function fn() {
  let foo = true
  if(true) {
    const foo // SyntaxError, const a besoin d'une valeur d'initialisation
    const foo = "bar"
    foo = "baz" // SyntaxError, foo est une const, en lecture seule
    let baz
  }
  console.log(baz) // ReferenceError, baz n'existe pas dans ce scope
  let foo = false // TODO Je ne crois pas que ça lève d'erreur dans le body d'une fonction, à vérifier
}
```

## Hoisting, TDZ

Pour rappel, javascript possède un mécanisme de hoisting, par exemple, vous pouvez écrire :

```javascript
function fn() {
  console.log(foo) // "bar"
  var foo = "bar"
}
```

Concrètement, le moteur d'exécution javascript va lire toutes les déclarations et remonter les déclarations avec `var` au début du scope de votre fonction.

`let` et `const` ne bénéficient pas de ce mécanisme de hoisting, ce qui peut mener à des problèmes de TDZ (Temporal Dead Zone). Vu que la déclaration de votre variable n'est pas remontée au scope de la fonction, il existe un moment où votre variable n'existe pas, ce moment, c'est la TDZ.

```javascript
function fn() {
  console.log(foo) // ReferenceError, on est dans la TDZ pour la variable foo
  let foo = "bar"
}
```

# Outro
