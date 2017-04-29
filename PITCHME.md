# Immutabilité en javascript
## TOURS.JS #6
### Sans bibliothèque !

---

# Pourquoi ?

Cela peut permettre dans certains cas d'augmenter les performances.

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

const [one, two, ...rest] = original

const newSomeWhere = [one, two, 'preact', ...rest]
// ou
const newSomeWhere2 = [
    ...original.slice(0, 2),
    'preact',
    ...original.slice(2)
]
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

const modified = original.map((valeur, i) =>{
    const prefix = i % 2 === 0 ? 'Old Fashioned' : 'Hype'
    return `${prefix} ${valeur}`
})

const hypeDriven = original.filter((valeur, i) => i % 2 === 1)
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
// Attention cela le clone que le premier niveau
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
// Attention cela le clone que le premier niveau
```

---

## C'est bien jolie mais je peux toujours modifier l'objet !

### ES5

```javascript
const post = Object.freeze({ title: 'super titre' })
// Attention ne freeze que le premier niveau
```

---

## Oui mais immutable.js c'est fait par facebook !

Avec immuatble.js vous avez de véritable objets/collections immuables !

Quand éviter ?
* Besoin de collection native à js
* Eviter 15Kb gzippé de js en plus
* Alergie à la programmation fonctionnelle
* Avec vuejs ou preact par exemple

Quand l'utiliser ?
* Avoir une meilleur API de collection
* Forcer l'immuabilité sans s'en soucier

+++

# Et dans Angular et React ?
Ils supportent tout deux les Iterables (ES6/ES2015) du coup l'utilisation est transparente.

---

# Questions ?