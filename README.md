
* TOC
{:toc}

# Notes préalables
Ce document écrit constitue le résultat de ma veille technologique menée dans le cadre du cours MSO 4.4.

Vous pouvez retrouvez les supports de la présentation orale [ici](https://stity.github.io/future-javascript/diaporama) ainsi qu'une [version pdf](https://stity.github.io/future-javascript/diaporama/presentation.pdf).


# Objectifs

A travers cette veille technologique, j'ai choisi de montrer quels sont les problèmes de ce langage et les solutions qui ont été mise en place ou qui vont l'être.

Cette présentation s'articule donc en trois partie. Je vais d'abord vous présenter JavaScript et ses caractéristiques, nous verrons ensemble les principals reproches qui lui sont généralement fait avant de mettre en avant les solutions existantes ou en cours de développement.

Ces solutions, comme nous le verrons au cours de ce document, peuvent émaner de plusieurs sources. Elles proviennent tantôt des évolutions des spécifications du langage et tantôt de la communauté, très active.

Il est à noter que les "reproches" présentés ici sont loins d'être exhaustifs et peuvent être subjectifs. Ils ne sont pas ceux de l'auteur de cet article mais ceux qui ont souvent été lus et entendus par l'auteur.

# Qu'est ce que JavaScript

## Présentation

JavaScript est un langage interprété à typage faible et dynamique. Décomposons ensemble la signification de cette phrase qui a son importance pour la suite de l'exposé.

- Interprété : par opposition aux langages compilés, le moteur JavaScript extrait directement du code source les instructions à éxécuter, on parle de compilation Just In Time [^1]
- Typage faible : le type d'une variable peut changer au cours de l'éxécution.
- Typage dynamique : le type d'une variable est déterminé au moment de l'attribution de sa valeur à la variable.

Inventé en 1987 par Tim Berners Lee, il est principalement connu pour être le langage du Web.

La dernière version stable de ses spécifications se nomme ECMAScript2016 et a été publié le 17 juin 2016. La version actuelle des spécification, encore en développement, est ECMAScript 2017.

[^1]: Pour plus d'informations sur la compilation Just In Time, je vous recommande [cet article "A cartoon Intro to WebAssembly"](https://hacks.mozilla.org/2017/02/a-cartoon-intro-to-webassembly/) par Lin Clark qui explique, en autres, le fonctionnement des compilateurs Just In Time et leur intéret

## Moteurs

La plupart des ordinateurs, tablettes et téléphones sont équipées d'un moteur JavaScript puisqu'il suffit d'un navigateur Web. Chaque navigateur possède son propre moteur :

- V8 pour Google Chrome
- Spider Monkey pour Mozilla Firefox
- Chakra pour Microsoft Edge (et Internet Explorer)
- JavaScript Core pour Safari

Mais il n'est pas nécessaire d'éxécuter JavaScript dans un navigateur; le framework [Node.js](https://nodejs.org) utilise le moteur V8 pour éxécuter JavaScript et permet l'éxecution de JavaScript sans navigateur. Ce framework est principalement utilisé côté serveur pour des applications Web.

## Un langage polyvalent

JavaScript est bien évidemment utilisé dans les pages Web mais pas seulement. On le retrouve également côté serveur avec Node.js, en application "Desktop" avec [Electron](http://electron.atom.io) (exemple d'application développée avec Electron : Bracket, le célèbre éditeur de texte orienté Web et développé par Adobe et Atom son concurrent développé par Github.)

Hello World version navigateur :

```javascript
document.body.innerHTML = "Hello World !";
```

Serveur Hello World avec Node.js :

```javascript
const http = require('http');

http.createServer( function(req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end("Hello World !\n");
});
server.listen(8080);

```



JavaScript est généralement décrit comme un langage multiparadigme : il peut être impératif, orienté objet (même les fonctions sont des objets en JavaScript), fonctionnel, ...

Ci-dessous un exempe de programmation impérative :
```javascript
var a = 1;
var b = 2;
var c = 1;

if (b*b - 4*a*c >= 0) {
    console.log("solution !");
}
```

Ci-dessous un exempe de programmation fonctionnelle :
```javascript
// exemple fonctionnel
```

Ci-dessous un exempe de programmation orientée objet :
```javascript
function Personne(nom) {
  this.nom = nom;
  console.log('Nouvel objet Personne créé');
}

var personne1 = new Personne('Alice');
var personne2 = new Personne('Bob');

//on affiche le nom de personne1 
console.log('personne1 est ' + personne1.nom); // personne1 est Alice
console.log('personne2 est ' + personne2.nom); // personne2 est Bob
```


Ces différents exemples démontrent la simplicité et la polyvalence de JavaScript.

## Développement actif

Selon [W3techs](https://w3techs.com/technologies/details/cp-javascript/all/all), près de 94% des sites Web utilisent JavaScript. Le langage est [n°1 des tendances sur GitHub de 2013](https://github.com/blog/2047-language-trends-on-github). Pour rappel, GitHub est la plateforme principale de gestion de version pour les projets et notamment pour les projets OpenSource. Plus de [390 000 packages](http://www.modulecounts.com/) sont disponibles sur [NPM](http://www.npmjs.com) (Node Packages Manager), la plateforme de gestion des modules JavaScript. Chaque jour, près de 525 nouveaux packages rejoignent en moyenne la plateforme.

JavaScript est le langage le plus populaire (connu par les développeurs) et le plus discuté (en terme de nombre de tags) sur [StackOverflow](http://stackoverflow.com/research/developer-survey-2016#technology).

![popularité des langages](sondage_popularity.png)


Les langages connus par les développeurs, sondage effectué en 2016 par StackOverflow.

Selon [HTTPArchive](http://httparchive.org/trends.php), la quantité moyenne de code JavaScript présent sur une page web s'élève à 420Ko et ce chiffre est en augmentation.


## Compatibilité

Le développement de JavaScript est constant. Des fonctionnalités sont sans cesse ajoutées aux spécifications. De ce fait, les moteurs peinent parfois à implémenter toutes les fonctionnalités et ils n'implémentent pas tous les mêmes.

Une table de compatibilité est [disponible en ligne](http://kangax.github.io/compat-table/es6/) et recensent les fonctionnalités implémentées par les moteurs et leurs différentes versions. Ci dessous, une capture d'écran de cette table pour ES6 :
![table de compatibilité](kangax.png)

## Polyfills

Il n'existe pas de numéro de version du langage JavaScript, c'est un parti pris. Le programme doit tester si une fonction est disponible avant de pouvoir l'utiliser.

Les polyfills permettent de redéfinir les fonctions manquantes. Ils sont notamment utiles pour garantir l'éxecution du code dans les navigateurs les plus anciens qui ne disposent pas des fonctionnalités les plus récentes.

Dans ce domaine, on peut citer [Babel](https://babeljs.io/) qui permet de transpiler du code JavaScript utilisant les dernières versions d'ECMAScript vers une version plus anciennes afin de pouvoir émuler les nouvelles fonctionnalités dans les navigateurs les plus anciens.

```javascript

let [a,b,c] = [1,2,3];

const carre = [1,2,3].map(x => x**2);

```

Ce code peut être transpiler en utilisant Babel et devient alors :
```javascript
"use strict";

var a = 1,
    b = 2,
    c = 3;


var carre = [1, 2, 3].map(function (x) {
  return Math.pow(x, 2);
});
let [a,b,c] = [1,2,3];

const carre = [1,2,3].map(x => x**2);

```

# Les reproches

Les reproches qui seront ici abordés sont les suivants :

- Pas de typage fort
- Callback Hell
- Mauvaise lisibilité
    - Callback Hell
    - Syntaxe étrange
- Pas de mémoire partagée
- Lenteur

## Typage

L'absence de typage fort rend le débuggage difficile, l'autocomplétion difficile pour les environnements de développement et l'éxécution plus lente.

Le débuggage est plus difficile car une même variable peut changer de type, des propriétés peuvent être ajoutées ou supprimer.
Pour ces mêmes raisons, l'autocomplétion peine à connaitre les méthodes ou propriétés d'un objet. 
L'éxécution est plus lente car le compilateur doit deviner le type d'un objet, si l'objet change de type ou s'il s'est trompé en devinant il perd du temps à réassigner l'objet.

## Les alternatives

Plusieurs alternatives ont été développées pour contrer ce problème de typage :

 - [Dart](http://www.dartlang.org) développé par Google
 - [TypeScript](http://www.typescriptlang.org/) popularisé par le framework Angular
 - [asm.js](http://asmjs.org/) 
 
### Dart

```javascript
exemple de dart
```

### TypeScript

```javascript
class Greeter {
    constructor(public greeting: string) { }
    greet() {
        return "<h1>" + this.greeting + "</h1>";
    }
};

var greeter = new Greeter("Hello, world!");

document.body.innerHTML = greeter.greet();
```

### asm.js

asm.js est un sous-ensemble de JavaScript. Les variables sont annotées pour aider le compilateur.

```javascript
exemple de asm
```

## Callback Hell

Couramment décrié par les réfractaires à JavaScript, le Callback Hell est un syndrome qui consiste à chaîner les appels de callback en embriquant les fonctions les unes à l'intérieur des autres.
Cette pratique rend le code difficile à lire et empêche la bonne compréhension de ce qu'il se passe réellement, i.e. des appels asynchrones qui s'enchainent.


```javascript
function foo(finalCallback) {
  request.get(url1, function(err1, res1) {
    if (err1) { return finalCallback(err1); }
    request.post(url2, function(err2, res2) {
      if (err2) { return finalCallback(err2); }
      request.put(url3, function(err3, res3) {
        if (err3) { return finalCallback(err3); }
        request.del(url4, function(err4, res4) {
          // let's stop here
          if (err4) { return finalCallback(err4); }
          finalCallback(null, "whew all done");
        })
      })
    })
  })
```

### Promises (ES6)

Les promises ont été crées pour contrer cette structure.
L'usage du mot clé "then" a été choisi pour refléter le caractère asynchrone.
D'abord implémentés par la communauté, elles ont rejoint le standard ECMAScript6.


```javascript
function foo() {
  return request.getAsync(url1)
  .then(function(res1) {
    return request.postAsync(url2);
  }).then(function(res2) {
    return request.putAsync(url3);
  }).then(function(res3) {
     return request.delAsync(url4);
  }).then(function(res4) {
     return "whew all done";
  });
}
```

Le code est nettement plus lisible mais cependant il est tout de même nécessaire de créer des fonctions callback.

### Async/Await (ES7)

Afin d'éliminer complétement la nécessité des callbacks, ECMAScript7 introduit les mots clés async et await qui permettent d'éxécuter du code asynchrone de manière non bloquante tout en restant lisible.

```javascript
async function foo() {
  var res1 = await request.getAsync(url1);
  var res2 = await request.getAsync(url2);
  var res3 = await request.getAsync(url3);
  var res4 = await request.getAsync(url4);
  return "whew all done";
}
```

