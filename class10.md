---
theme : "sky"
highlightTheme : "agate"
---

<style>
    .reveal .slides {
      zoom: 1 !important;
      height: auto !important;
    }
    .reveal .slides section {
      top: 50% !important;
      transform: translateY(-50%) !important;
      zoom: 0.75 !important;
    }
    .reveal pre {
        width: 100% !important;
    }
    .reveal section pre code {
        overflow: hidden !important;
        max-height: none !important;
        white-space: pre-wrap !important;
    }
    .reveal img {
        border: none !important;
        background: none !important;
    }
</style>

# Web Storage

---

## Server-side storage

Most major modern web sites are dynamic — they store data on the server using some kind of database (server-side storage), then run server-side code to retrieve needed data, insert it into static page templates, and serve the resulting HTML to the client to be displayed by the user's browser.

This is the pattern used by Rails apps.

---

## Client-side storage

Client-side storage works on similar principles, but stores data on the client-side (i.e. in the user's browser).

Typical uses for client-side storage include:

* Personalizing site preferences (e.g. showing a user's choice of custom widgets, color scheme, or font size).

* Persisting previous site activity (e.g. storing the contents of a shopping cart from a previous session, remembering if a user was previously logged in).

* Saving data and assets locally so a site will be quicker (and potentially less expensive) to download, or be usable without a network connection.

* Saving web application generated documents locally for use offline

---

# Cookies

The earliest method for storing client-side data was the cookie.

A cookie is nothing more than a key/value pair.

All cookies relating to the same origin are stored together in a single semicolon-delimited string.

```
console.log(document.cookie)  // displays all the cookies for this site
```

We'll cover cookies briefly, as many sites still use them, but it is strongly recommended to avoid them for any new project.

---

## Creating a cookie

We can add a new cookie like this:

```
document.cookie = "name=John"
document.cookie = "favourite_food=pizza"
```

Note that the = isn't like a normal assignment - it actually uses a setter method on the cookie object so that the new cookie is added, rather than it replacing the entire cookie string.

---

## Getting a cookie value

This is a bit harder, because ALL cookies exist in the same string, so we need to write something to parse the cookie and find the one we want.

There's plenty of ways to solve this (even jQuery plugins), but here's one way.

```
function getCookie(key) {
    return document.cookie  // with the document.cookie
                .split(';') // split on semicolon into an array
                .find(pair => pair.includes(key)) // find the element with the desired key 
                .split('=')[1]  // split on the equals and return the value (element 1)
}

console.log(getCookie('favourite_food'))  // pizza
```

Even this is kind of ugly and awkward.

---

## Deleting a cookie

"Deleting" a cookie uses the same syntax as creating one, except we pass no value.

```
document.cookie = "favourite_food="
```

The cookie isn't actually deleted, just set to the empty string, which is false-y.

---

## Disadvantages

Apart from being awkward to work with, cookies also have several other disadvantages.

* Insecure - data is stored in plain text

* Size limit - all cookies combined can only consume about 4kb

* Cookie limit - 20 cookies per site maximum

* Can be disabled by user, potentially breaking your site

* Cannot store complex data types, only plain strings

* There are better modern alternatives

* Your site must now pop up an opt-in warning about cookies under new EU regulations
    * ...despite the violation of sovereignty inherent in AU citizens being under EU jurisdiction.

---

# localStorage

A better way of storing data on the client-side is the [localStorage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API).

The localStorage API provides a very simple syntax for storing and retrieving smaller data items consisting of a key/value pair, similar to cookies.

However, unlike cookies, localStorage is easier to use and more flexible.

The biggest benefit of localStorage is that the data will persist indefinitely, even if the tab/browser/computer is shut down and reopened/restarted at a later date.

---

## Save data in localStorage

The setItem() method allows you to save a data item in storage — it takes two parameters: the name (key) of the item, and its value.

```js
localStorage.setItem('name', 'Chris')
```

Note that we don't need to import or declare the localStorage object - it is built-in to the browser.

Unlike cookies, we can store a complex structure like an array or object by encoding it as JSON first.

```js
let person = {
    name: 'Bill',
    age: 33
}

localStorage.setItem('person', JSON.stringify(person))
```

---

## Get data from localStorage

The getItem() method allows you to retrieve a data item from storage — it takes one parameter: the name (key) of the item.

```js
localStorage.getItem('name')  // Chris
```

Assuming we stored Bill as 'person' as per the previous slide, we can retrieve the JSON and parse it to reconstruct the original object.

```js
let person = JSON.parse(localStorage.getItem('person'))
console.log(person)  // { name: 'Bill', age: 33 }
```

---

## Delete from localStorage

Unlike cookies, we can actually delete items from localStorage.

The removeItem() method takes one parameter — the name of a data item you want to remove — and removes that item out of web storage.

```js
localStorage.getItem('name')  // Chris
localStorage.removeItem('name')
localStorage.getItem('name')  // null
```

---

## sessionStorage

sessionStorage works in exactly the same way as localStorage - it has the same methods, syntax and API.

The difference with sessionStorage is that the data will only persist for the current browser session. All items related to the current origin will be deleted when the session ends.

A session ends when the browser is shut down or the user manually clears the site's storage.

---

## Resources

[Client-side storage tutorial](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Client-side_storage)

[Cookies reference](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie)

[Client-side storage reference](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)

---

# Questions?

---

## AFTERNOON CHALLENGE

### [Canvas Unit: Cookies](https://coderacademy.instructure.com/courses/144/pages/unit-cookies)

### [Canvas Unit: Web Storage](https://coderacademy.instructure.com/courses/144/pages/unit-web-storage)
