# JavaScript Core: in the Browser

![](https://img.shields.io/badge/status-draft-darkred.svg)

** What we will learn today?**

* [DOM](#dom)
* [Events](#browser-events)
* [AJAX](#ajax)

---

## The DOM

Your webpages are made up of a bunch of HTML elements, nested within each other
(parents and children). In JavaScript we have access to this "DOM" object
(Document Object Model) which is actually a representation of our webpage that
JavaScript can work with.

Here are two examples, HTML and then JavaScript, of how the DOM might look like:

```html
<html>
    <body>
        <h1> Welcome! </h1>
        <p> Hello world! </p>
    </body>
</html>
```

```javascript
var document = {
  body: {
    h1: "Welcome",
    p: "Hello world!"
  }
};
```

In the browser the DOM is represented by the `window.document` object, which can
also be accessed directly using `document`. We can use it to get information
about the page loaded into the browser, query the content of the page and edit
it. You can see full details of the functionality
[here](https://developer.mozilla.org/en-US/docs/Web/API/Document).

### Querying

The DOM offers a lot of useful functions we can use to find elements on the
page. Here are some we'll be using today:

```js
element = document.getElementById(id);
```

`getElementById` accepts an id string as argument and returns an `Element` from
the document with a matching id. If no matching `Element` is found the function
returns `null`.

```js
elements = document.getElementsByClassName(names); // or:
elements = rootElement.getElementsByClassName(names);
```

`getElementsByClassName` takes a string containing one or more classes and
returns an `HTMLCollection`, which is an array-like object containing all
elements whose class attributes match the string. We can pass multiple classes
as an argument to query for elements matching all classes.

```js
getElementsByClassName("green bike");
```

Above call will return elements that have both the `green` and `bike` classes.

`getElementsByClassName` can be called on individual `elements` as well as the
top-level `document` object. When calling `getElementsByClassName` on an
`element`, the method will query only the children of the `element` rather than
the entire `document`

```js
elements = document.getElementsByTagName(name); // or:
elements = rootElement.getElementsByTagName(name);
```

Much like `getElementsByClassName`, `getElementsByTagName` allows us to query
for elements using the tag name. `getElementsByTagName` also returns
`HTMLCollection` and can be called on `element`s as well as `document`.

All of the above calls return a live reference, which means that the objects
will be automatically updated with all changes since the query.

> **Exercise**:
>
> * Clone the repo from HTML and CSS class into new `bikes` directory. `git
>   clone git@github.com:CodeYourFuture/bikes-for-refugees.git bikes`
> * Create an `index.js` file in a `src` folder inside the `bikes` repo.
> * Put the following code in the `index.js` file: `alert('hello');` to check
>   the file is being loaded
> * Import the `index.js` file into `index.html` by placing `<script
>   src="src/index.js"></script>` just before the closing `body` html tag.
> * Open `index.html` in your browser.
> * In `index.js`:
> * Using `getElementById`, `getElementsByClassName` or `getElementsByTagName`
>   ...
> * ... get the element with id `donation-count-alert` and `console.log` it.
>   Look up documentation for `Element` or use a debugger find and `console.log`
>   the contents of the element.
> * ... get all elements with the class `btn`, loop over them and `console.log`
>   them individually. You may need to look up documentation for
>   `HTMLCollection`.
> * ... get all links inside the element with id `navbarSupportedContent`, loop
>   over the collection and `console.log` the text inside each link

### Query selector

The above selector functions are available in all browsers, but can be somewhat
inflexible for example in situations that require complex lookups. Modern
browsers have

```js
document.querySelector("#mainHeader");
document.querySelectorAll("p");
```

Both `.querySelector` and `querySelectorAll` accept a CSS selector as an input.
`.querySelector` selects only the first element it finds, `querySelectorAll`
selects all elements and returns a `NodeList`, which is a collection of `Nodes`
(not an array). Unlike the selectors in the previous section, these functions
return static results. That means any changes in the DOM will NOT result in
updates to the elements.

> **Exercise**:
>
> * Comment out the code from previous exercise and rewrite solutions using
>   `querySelector` and `querySelectorAll`. You may need to look up
>   documentation for `NodeList`.

### DOM manipulation

We can use the DOM to edit elements. For example the `textContent` property of
elements can used to read as well as set the text contents of an element.

```js
var x = document.querySelector(".jumbotron h1");
console.log(x.textContent); // Bikes for Refugees
x.textContent = "Something else";
```

Similarly, `innerHTML` property of elements can be used to get the HTML content
of an element as well as

```js
var x = document.querySelector(".jumbotron h1");
x.innerHTML = `<strong>${x.textContent}</strong>`;
```

We can also access the `style` property of elements and update various
properties

```js
var elements = document.querySelectorAll(".btn-primary");

elements.forEach(element => (element.style.backgroundColor = "red"));
```

Please note the use of camelCase style attribute names

We can also check it exists, read, change and remove attributes of elements
using `hasAttribute`, `getAttribute()`, `removeAttribute` and `setAttribute`

```js
var elements = document.querySelectorAll("a");

elements.forEach(element => {
  if (element.hasAttribute("href")) {
    var href = element.getAttribute("href");
    console.log(href);
    element.setAttribute("href", "https://google.com");
  }
});
```

What will above code do?

> **Exercise**:
>
> * Use above functions to
> * ... place `-` around the text in the navbar links
> * ... convert links in 'Upcoming Events' section to italic using `<i>` tag
> * ... make `Learn more` links green

### Creating and inserting elements

We can use `document.createElement(tagName)` method to create a new element and
`document.createTextNode(text)` to create new text contents. The elements
created can be manipulated just like the elements above, but the changes will
not visible until we insert the new element into the DOM.

We can insert elements into other elements using `element.appendChild` or
`element.insertBefore`. For example.

```html
<div id="parent">
    <p>some content</p>
</div>
```

```js
var spanNode = document.createElement("span");
var textNode = document.createTextNode("hello");
spanNode.appendChild(textNode);

var parentNode = document.getElementById("parent");
parentNode.appendChild(spanNode);
```

What do you think above code will do?

`insertBefore` is a bit more complicated.

```js
var insertedNode = parentNode.insertBefore(newNode, referenceNode);
```

Here `insertedNode` is the the node being inserted, that is `newNode`.
`parentNode` is the the parent of the newly inserted node. `newNode` is the node
to be inserted and `referenceNode` is the node before which newNode is inserted.

There is no `insertAfter` method. It can be emulated by combining the
`insertBefore` method with `nextSibling` property. In the line below we use this
approach to insert `nodeOne` after `nodeTwo` inside `parentNode`

```js
parentNode.insertBefore(nodeOne, nodeTwo.nextSibling);
```

> **Exercise**:
>
> * Use the inspector to examine the navbar
> * Create a new navbar item for Code Your Future which links to
>   `https://codeyourfuture.co/`
> * Insert it at the end of the navbar

## Resources

1. [DOM: Document](https://developer.mozilla.org/en-US/docs/Web/API/Document)

## Browser events

Browser events are actions that take place on our web page. They could be user
triggered such as a mouse being clicked, a key being pressed or a form
submitted. Alternatively, they could be triggered by the system such as a
resource being loaded or an error occurring.

We can subscribe to those events by registering an `eventHandler` to perform a
certain action when an event is detected. For example, we may want to update a
letter count on a text input when a user types or we may want to perform
validation on a form before submitting it.

For each event subscription we will need 3 things:

1. `target` - this is the object where we are listening for events on. For
   example, it could be a link on which we would like to listen to for clicks.
1. `eventType` - as the name suggests, this is the type of event we ar listening
   for. There are dozens of possible events (see
   [here](https://developer.mozilla.org/en-US/docs/Web/Events) for the full
   list). Common ones include . `click` - mouse click . `submit` - form submit .
   `keydown` - keyboard key being pressed . `mouseenter` - a mouse cursor being
   moved onto an element . `change` - value of a form input changing
1. `eventHandler` - A function that we want to execute when the event is
   detected

To subscribe to the actual event we use the `addEventListener` method of our
`target` element like below

```js
var myButton = document.querySelector("#myButton");
myButton.addEventListener("click", alertSomething);

function alertSomething() {
  alert("Something");
}
```

In the above code `myButton` is the target on which we listen to for `click`
events and call the event handler `alertSomething` when the click is detected.

> **Exercise**:
>
> * Set a click listener on the donate buttons and increment the donated bikes
>   counter with each click

### Event object

Just knowing that an event has happened is not particularly useful in most
cases. Usually, we will want to know more about what exactly happened such as
the coordinates of a mouse `click` or the new `value` of an input after it has
changed.

Keep in mind that different event types will have characteristics that are
specific to them. For example, `key` events will have a `event.char` property
indicating the key that was pressed. `mouse` events will have `event.clientX`
and `event.clientY` properties indicating the location of the mouse event on the
screen, where (0,0) is the top left hand corner in the browser.

Every event object has `event.preventDefault()` which when called will prevent
the default action of the event from being triggered. This is particularly
useful to intercepting events and altering the behaviour when needed. For
example, we can call `event.preventDefault()` on a `submit` event if the data in
the form is not valid.

We can find out which element the event was triggered on from the `event.target`
property.

> **Exercise**:
>
> * Prevent clicks on links from triggering a url change in the browser and
>   instead `console.log` the coordinates of the click as well as the text
>   inside the link

### Bubbling

> **Together**:
>
> * Let's place a `click` listener on the `.jumbotron` element and `console.log`
>   the `event`
> * What happens when we click on the buttons?

Most events, though not all, have a default behaviour know as `bubbling` which
results in events being propagated to the target element's parents. That means
an event listener placed on the parent will be triggered after the event
listener placed on the child. Why is this behaviour desired?

We can distinguish between the elements on which the event was triggered vs the
element on which the event was detected.

* `event.target` is the element on which received the initial event
* `event.currentTarget` is the element on which the event listener detected the
  event

In some cases we may want to prevent an event from bubbling up. We can do so by
calling the `stopPropagation` method on the event. Once it has been called, any
event handlers placed by parent elements will not be triggered.

> **Exercise**:
>
> * Place a single event listener on the `body` of the document
> * `console.log` the text inside each linked clicked, but perform no action for
>   clicks originating from inside the jumbotron.

# Resources

1. [Introduction to browser events](https://javascript.info/introduction-browser-events)
2. [Events](https://developer.mozilla.org/en-US/docs/Web/Events)

## AJAX

### What's a server

A device or program that **provides functionality to other programs or
devices**. There are database servers, mail servers, game servers, etc.

They can take the form of industrial server farms that provide a service to
millions of users (used by Facebook, Google, etc.), to personal servers for
storing your files.

The server communicates with **clients**. Clients can be a web browser, a Slack
app, your phone, etc.

Client–server systems use the **request–response model**: a client sends a
request to the server, which performs some action and sends a response back to
the client, typically with a result or acknowledgement.

> An example: We can use the Slack app (the client) to put our messages or
> pictures on Slack. The content is stored on the Slack servers and other
> clients can then also access the pictures.

### HTTP requests

A server stores the data, and the client (other programs or computers) requests
data or sends some of its own. But how do they talk to each other?

**For the client and the server to communicate they need an established language
(a protocol)**. Which is what HTTP (Hypertext Transfer Protocol) is for. It
defines the methods you can use to communicate with a server and indicate your
desired actions on the resources of the server.

There are two main types of requests: GET and POST.

> With a **GET request** you can ask for specified resource (e.g. show me that
> Slack photo).

> With a **POST request** you can send content to the server to be appended to
> the web resource (e.g. post a photo on Slack).

HTTP is the language of the internet. In our case we're using Javascript, but
you can send HTTP requests with other laguages as well.

### AJAX (= Asynchronous JavaScript And XML)

AJAX is a set of useful methods for implementing client-server communication.

![AJAX Diagram](https://www.w3schools.com/xml/ajax.gif "AJAX Diagram")

AJAX just uses a combination of:

> A browser built-in XMLHttpRequest object (to request data from a web server)
> JavaScript and HTML DOM (to display or use the data)

**Ajax works behind the scenes, helping the webpage communicate with the server
(with GET and POST requests).**

> client ----------GET request----------> server returns data to client client
> ----------POST request (with content)--------->server updates data with
> content

The server holds the data, but it only sends it to the webpage when there's a
request. The request can be sent after the page has loaded, for example when a
user clicks a button.

### Why Ajax?

There are other ways you can write HTTP requests, such as using Web sockets.
What's great about AJAX is that it makes it look like magic! The server and the
client communicate effortlessly:

> Update a web page without reloading the page Request data from a server -
> after the page has loaded Receive data from a server - after the page has
> loaded Send data to a server - in the background

### AJAX Example

The instant update: we can write code that makes the web page instantly update
its contents (without reloading the page).

Let's try sending some data from Rares’ phone to the server and see whether it
will update the webpage.

> [http://zero-point.github.io/](http://zero-point.github.io/)

### Let's Code

How does the code work? Let's break it down into parts and see what each does.

#### POST Code

```js
var request = new XMLHttpRequest(); //creating a request object

request.onreadystatechange = function() {
  if (request.readyState === 4) {
    // check if a response was sent back
    if (request.status === 200) {
      // check if request was successful
      textBox.innerHTML = request.responseText;
    } else {
      textBox.innerHTML =
        "An error occurred during your request: " +
        request.status +
        " " +
        request.statusText;
    }
  }
};
var url = "http://ajax-cyf.eu-west-1.elasticbeanstalk.com/chatroom/?id=cyf"; //server location
var params = "Here is some content"; // content we want to send
request.open("POST", url, true); // adding them to the request

request.setRequestHeader("Content-type", "application/x-www-form-urlencoded"); //header info
request.send(params); // sending the request
```

#### GET Code

```js
var request = new XMLHttpRequest(); //creating a request object

request.onreadystatechange = function() {
  if (request.readyState === 4) {
    // check if a response was sent back
    if (request.status === 200) {
      // check if request was successful
      textBox.innerHTML = request.responseText;
    } else {
      textBox.innerHTML =
        "An error occurred during your request: " +
        request.status +
        " " +
        request.statusText;
    }
  }
};
var url = "http://ajax-cyf.eu-west-1.elasticbeanstalk.com/chatroom/?id=cyf"; //server location
request.open("GET", url); // adding it to the request

request.setRequestHeader("Content-type", "application/x-www-form-urlencoded"); //header info
request.send(); // sending the request
```

### AJAX Exercise

Everyone should organise in pairs, one person writing code to **send data
(POST)** and one to **receive it (GET)**. Once you’ve finished your code,
combine it and put it into a html page. Now, try sending each other messages
this way.

Don’t forget to use a **unique id** at the end of the url (not 'cyf') and let
your partner know what it is!

## Resources

* [MDN: DOM Examples and explanation](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Examples)
* [AJAX](https://developer.mozilla.org/en-US/docs/AJAX/Getting_Started)

{% include "./homework.md" %}
