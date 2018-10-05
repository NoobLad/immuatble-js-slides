# Immuabilité en javascript
### Sans bibliothèque !
#### ou pas...

---

## C'est qui ?

### Ludovic Lafole
* Développeur Web Indépendant
* Membre Actif de PaloAltours
 * Animateur du Tours.js et des Hacking Mondays
 * Organisateur des rassemblements Ludum Dare à Tours et des Hitbox Apéros

---

## Pourquoi l'immuabilité ?

+++

## Multithreading
## Ok... pas en js...

+++

## Vérifier le changement d'une valeur

* PureComponent et Functional Component de React
* ChangeDetection.onPush pour Angular

+++

## Programmation fonctionnelle

---

## Vérifier le changement
### sans immuabilité

```javascript
let a = { b : { c : 1 } }

let previous = copyA()
function aChanged() {
    const changed = previous !== next 
        || previous.b !== next.b 
        || previous.b.c !== next.b.c
    previous = copyA()
    return changed
}

function copyA() {
    return { b: { c: a.b.c } }
}
```

+++

## Vérifier le changement
### avec immuabilité

```javascript
let a = { b : { c : 1 } }

let previous = a;
function aChanged() {
    const changed = previous !== a
    previous = a
    return changed
}
```

---

# Attention !
## _*const*_ ne veut pas dire que votre donnée est immuable ! 

---

# Sur une collection

---

# Ajouter un élément

+++

## Ajouter un élément
### ES5

```javascript
var original = ['angular', 'react', 'vue']

var newAfter = original.concat('preact')

var newBefore = ['preact'].concat(original)

var newSomeWhere = original
                    .slice(0, 2)
                    .concat('preact')
                    .concat(original.slice(2))
```

+++

## Ajouter un élément
### ES6 / ES2015

```javascript
const original = ['angular', 'react', 'vue']

const newAfter = [...original, 'preact']

const newBefore = ['preact', ...original]

const [one, two, ...tail] = original

const newSomeWhere = [one, two, 'preact', ...tail]
// ou
const newSomeWhere2 = [
    ...original.slice(0, 2),
    'preact',
    ...original.slice(2)
]
```

+++

## Ajouter un élément
### Immutable.js

```javascript
import {List} from "immutable"

const original = List.of('angular', 'react', 'vue')

const newAfter = original.push('preact')

const newBefore = original.unshift('preact')

const newSomeWhere = original.insert(2, 'preact')
```

---

# Retirer un élément

+++

## Retirer un élément
### ES5

```javascript
var original = ['angular', 'react', 'vue']

var lastRemoved = original.slice(0, -1)

var firstRemoved = original.slice(1)

var newSomeWhere = original
                    .slice(0, 1)
                    .concat(original.slice(2))
```

+++

## Retirer un élément
### ES6 / ES2015

```javascript
const original = ['angular', 'react', 'vue']

const lastRemoved = original.slice(0, -1)

const [ , ...firstRemoved] = original

const newSomeWhere = [
    ...original.slice(0, 1),
    ...original.slice(2)
]
```

+++

## Retirer un élément
### Immutable.js

```javascript
import {List} from "immutable"

const original = List.of('angular', 'react', 'vue')

const lastRemoved = original.pop()

const firstRemoved = original.shift()

const newSomeWhere = original.delete(2)
```

---

# Altérer/filtrer les valeurs

+++

## Altérer/filtrer les valeurs
### ES5

```javascript
var original = ['angular', 'react', 'vue']

var modified = original.map(function(valeur, i) {
    var prefix = i % 2 === 0 ? 'Old Fashioned' : 'Hype'
    return prefix + ' ' + valeur
})

var hypeDriven = original.filter(function (valeur, i) {
    return i % 2 == 1
})
```

+++
## Altérer/filtrer les valeurs
### ES6 / ES2015

```javascript
const original = ['angular', 'react', 'vue']

const modified = original.map((valeur, i) => {
    const prefix = i % 2 === 0 ? 'Old Fashioned' : 'Hype'
    return `${prefix} ${valeur}`
})

const hypeDriven = original.filter((valeur, i) => i % 2 === 1)
```

+++
## Altérer/filtrer les valeurs
### Immutable.js

```javascript
import {List} from "immutable"

const original = List.of('angular', 'react', 'vue')

const modified = original.map((valeur, i) => {
    var prefix = i % 2 === 0 ? 'Old Fashioned' : 'Hype'
    return `${prefix} ${valeur}`
})

const hypeDriven = original.filter((valeur, i) => i % 2 == 1)
```

---

# Sur des Objets

---

# Ajouter/Modifier un champ

+++
## Ajouter/Modifier un champ
### ES5

```javascript
var post = {
    title: 'A New Awesome JS Framework is Born !',
    description: 'The Toto.js Framework is gonna blow your head !',
    random: 5
}

var datedPost = JSON.parse(JSON.stringify(post)) // sale ?
// Il existe des libs qui font ça proprement, mais...
// Trop long pour les slides !

datedPost.createdDate = new Date()
delete datedPost.random
```

+++
## Ajouter/Modifier un champ
### ES6 / ES2015

```javascript
const post = {
    title: 'A New Awesome JS Framework is Born !',
    description: 'The Toto.js Framework is gonna blow your head !'
}

const datedPost = Object.assign(
    {},
    post,
    {createdDate: new Date()}
)
// Attention cela ne clone que le premier niveau
delete datedPost.random // Toujours pas de vrai solution,
// mettre null ou undefined ?
```
+++
## Ajouter/Modifier un champ
### ES8 / ES2017

```javascript
const post = {
    title: 'A New Awesome JS Framework is Born !',
    description: 'The Toto.js Framework is gonna blow your head !'
}

const {random, ...intermediate} = post // Stage 3

const datedPost = {
    ...intermediate, // Stage 3
    date: new Date()
}
// Attention cela ne clone que le premier niveau
```

+++
## Ajouter/Modifier un champ
### Immutable.js

```javascript
import {Map} from "immutable"

const post = Map({
    title: 'A New Awesome JS Framework is Born !',
    description: 'The Toto.js Framework is gonna blow your head !'
})

const intermediate = post.delete('random')

const datedPost = post.set('date', new Date())
```

---

## C'est bien jolie mais je peux toujours modifier l'objet !
### pour les version en js

### ES5

```javascript
const post = Object.freeze({ title: 'super titre' })
// Attention n'est pas récursif
```

---

## Quand est-ce que j'en ai besoin ?

### Bonne question !

+++

## Surtout pour votre état global d'application
### Ca rend le tracking de qui modifie quoi plus simple
### Et le debug aussi

+++

## Pas besoin que tout soit immuable
### Pour les données interne de vos composants c'est peut-être pas nécéssaire 

---

## Quand est-ce que je perds en performance ?

* Sur une grosse structure de données qui bouge souvent
* Dans vue

---

## Oui mais immutable.js c'est fait par facebook !

* Avec immutable.js vous avez de véritable objets/collections immuables !
* Convertir d'immutable vers du js natif coûte en performance

+++

## Quand éviter ?
* Besoin de collection native à js
* Eviter 15Kb gzippé de js en plus
* Alergie à la programmation fonctionnelle
* Avec vuejs ou preact par exemple

+++

## Quand l'utiliser ?
* Avoir une meilleur API de collection
* Forcer l'immuabilité sans s'en soucier

+++

## Et dans Angular et React ?
Ils supportent tout deux les Iterables (ES6/ES2015) du coup l'utilisation est transparente.

---

## Conclusion
#### On peut utiliser l'immuabilité et gagner en performance mais
* Il faut savoir pourquoi on en a besoin
* Il faut prévenir vos outils que vos données sont immuables

---

# Questions ?
