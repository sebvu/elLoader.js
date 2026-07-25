# elLoader

The most basic implementation for inserting your elements dynamically.

One call to rule them all.

```javascript
import Loader from "elLoader.js"

Loader.loadElements(Loader.newEl("p", { text: "Hello World!" }))
```

### Why?

Dynamically adding elements with raw JS can suck. This is a **one-usage no-bloat** solution.

Take for example this extremely simple example.

- Within `document.body`, a h1 title that says "Fun Facts!" proceeded by a parent div that has three divs, all with class "container" and each with a unique ID, with each div containing a paragrapn that has a fun fact.

Here are three solutions.

<details>
<summary>Classic</summary>

```js
const title = document.createElement("h1");
const wrapperDiv = document.createElement("div");
const divOne = document.createElement("div");
const divTwo = document.createElement("div");
const divThree = document.createElement("div");
const divOneParagraph = document.createElement("p")
const divTwoParagraph = document.createElement("p")
const divThreeParagraph = document.createElement("p")

title.textContent = "Fun Facts!"

wrapperDiv.setAttribute("id", "wrapper")
wrapperDiv.classList.add("container")

divOne.setAttribute("id", "childOne")
divOne.classList.add("container")

divTwo.setAttribute("id", "childTwo")
divTwo.classList.add("container")

divThree.setAttribute("id", "childThree")
divThree.classList.add("container")

divOneParagraph.textContent = "at this very moment i am hungry"
divTwoParagraph.textContent = "a lot of people die from mosquito bites"
divThreeParagraph.textContent = "i hate ants"

wrapperDiv.appendChild(divOne)
wrapperDiv.appendChild(divTwo)
wrapperDiv.appendChild(divThree)
divOne.appendChild(divOneParagraph)
divTwo.appendChild(divTwoParagraph)
divThree.appendChild(divThreeParagraph)

document.body.appendChild(title)
document.body.appendChild(wrapperDiv)
```
</details>

<details>
<summary>elLoader.js (inline)</summary>

```js
import Loader from "elLoader.js"

const elements = Loader.createElements(
    Loader.newEl("h1", { text: "Welcome to my fun facts!" }),
    Loader.newEl("div", { 
        classList: "container",
        id: "wrapper",
        children: [
            Loader.newEl("div", {
                classList: "container",
                id: "childOne",
                children: [ Loader.newEl("p", { text: "at this very moment i am hungry" }) ]
            }),
            Loader.newEl("div", {
                classList: "container",
                id: "childTwo",
                children: [ Loader.newEl("p", { text: "a lot of people die from mosquito bites" }) ]
            }),
            Loader.newEl("div", {
                classList: "container",
                id: "childThree",
                children: [ Loader.newEl("p", { text: "i hate ants" }) ]
            })
        ]
    })
)

for (const el of elements) {
    document.body.append(el)
}
```
</details>

<details>
<summary>elLoader.js (with variables)</summary>

```js
import Loader from "elLoader.js"

const divOne = Loader.newEl("div", {
    classList: "container",
    id: "childOne",
    children: [ Loader.newEl("p", { text: "at this very moment i am hungry" }) ]
}),

const divTwo = Loader.newEl("div", {
    classList: "container",
    id: "childTwo",
    children: [ Loader.newEl("p", { text: "a lot of people die from mosquito bites" }) ]
}),

const divThree Loader.newEl("div", {
    classList: "container",
    id: "childThree",
    children: [ Loader.newEl("p", { text: "i hate ants" }) ]
})

const elements = Loader.createElements(
    Loader.newEl("h1", { text: "Welcome to my fun facts!" }),
    Loader.newEl("div", { 
        classList: "container",
        id: "wrapper",
        children: [
            divOne,
            divTwo,
            divThree
        ]
    })
)

for (const el of elements) {
    document.body.append(el)
}
```
</details>

> Ok... looks like the same amount of lines so why?

EVERY element is configurable within its own block. It is easy to remove if needed, and create one with the call of one method.

For more extreme cases, this could save you up to hundreds of lines like it did for me.

And more importantly, **this is the only functionality!** There is absolutely no bloat. Only tradeoff is cleaner code.. :)

### Usage

There are only *two* other methods used in conjunction with `.loadElements()`

##### newEl()

```js
Loader.newEl(
    elementType, 
    { 
        id = null, 
        classList = null, 
        attrsList = null,
        isES = false,
        children = null,
        text = null
    } = {}
    
) => { /* ... */ }
```

- **Required**
    - {string} elementType: A valid HTML element type
- **Optional**
    - {string} id: ID name, no spaces.
    - {string|string[]} classList: A single class name, or an array of class strings.
    - {Object[]} attrsList: An object consisting of `key: "value"` pairs, where `key` is the attribute and `value` is the attribute value.
    - {boolean} isNS: Whether to declare an element as a 'NS' element.
    - {Object[]} children: **An array of elLoader objects that will be nested under this parent element**
    - {string} text: textContent string

> [!NOTE]
> `isNS` only uses the namespaceURI for **SVGs** at this moment. There is also no support for `inline` styles at this point.

##### newTextNode()

```js
Loader.newTextNode(msg) => { /* ... */ }
```

-- **Required**
    - {string} text: textContent string

### Where to now?

This project for the most part is unserious. I may post it to `npm` sometime though. I want to extend its capabilities as well, but it'll really only come to me when I *need* it since I do use this for my webpack applications.
