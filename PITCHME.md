# Immutabilité en javascript
## TOURS.JS #6
### Sans bibliothèque !

---

# Pourquoi ?

---

# Sur une collection

---

# Ajouter un élément

+++

## ES5

```javascript
var original = ['angular', 'react', 'vue']

var newAfter = original.concat('preact')

var newBefore = ['preact'].concat(original)

var newSomeWhere = original.slice(0, 2).concat('preact').concat(original.slice(2))
```

+++

## ES6 / ES2015

```javascript
const original = ['angular', 'react', 'vue']

const newAfter = [...original, 'preact']

const newBefore = ['preact', ...original]

const [one, two, ...rest] = original

const newSomeWhere = [one, two, 'preact', ...rest]
// ou
// const newSomeWhere = [...original.slice(0, 2), 'preact', ...original.slice(2)]
```

---

# Retirer un élément

+++

## ES5

```javascript
var original = ['angular', 'react', 'vue']

var lastRemoved = original.slice(0, -1)

var firstRemoved = original.slice(1)

var newSomeWhere = original.slice(0, 1).concat(original.slice(2))
```

+++

## ES6 / ES2015

```javascript
const original = ['angular', 'react', 'vue']

const lastRemoved = original.slice(0, -1)

const [ , ...firstRemoved] = original

const newSomeWhere = [...original.slice(0, 1), ...original.slice(2)]
```

---

# Altérer/filtrer les valeurs

+++

## ES5

```javascript
var original = ['angular', 'react', 'vue']

var modified = original.map(function(valeur, i) {
    var prefix = i % 2 === 0 ? 'Old Fashioned' : 'Hyper'
    return prefix + ' ' + valeur
})

var hypeDriven = original.filter(function (valeur, i) {
    return i % 2 == 1
})
```

+++

## ES5 / ES2015

```javascript
const original = ['angular', 'react', 'vue']

const modified = original.map((valeur, i) =>{
    const prefix = i % 2 === 0 ? 'Old Fashioned' : 'Hyper'
    return `${prefix} ${valeur}`
})

const hypeDriven = original.filter((valeur, i) => i % 2 === 1)
```

---

# Sur des Objets

---

# Ajouter/Modifier un champ

+++

## ES5

```javascript
var post = {
    title: 'A New Awesome JS Framework is Born !',
    description: 'The Toto.js Framework is gonna blow your head !',
    random: 5
}

var datedPost = JSON.parse(JSON.stringify(post)) // sale ?
// Il existe des libs qui font ça proprement, mais... Trop long pour l'écran !

datedPost.createdDate = new Date()
delete datedPost.random // Pas de vrai solution
```

+++

### ES6 / ES2015

```javascript
const post = {
    title: 'A New Awesome JS Framework is Born !',
    description: 'The Toto.js Framework is gonna blow your head !'
}

const datedPost = Object.assign({}, post, {createdDate: new Date()})
// Attention cela le clone que le premier niveau
delete datedPost.random // Toujours pas de vrai solution, mettre null ou undefined ?
```
+++

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

# Questions ?