# SPA

---

## MODELS  

can have logic in addition to data, such as validation, default data, and custom functions  

---  

## ROUTING  

>> ![spa-1](/z-static/images/spa/clientRouter-1.png)

### Features a client-side router offers

- Match patterns in the URL with paths defined by the route  
- Allow for the execution of code in your application when a match is found  
- Allow a view to be specified that will be displayed when the route is triggered  
- Allow for parameters to be passed via the route’s path  
- Allow users to use standard navigation methods of the browser to navigate the SPA  

### Hashtag Navigation   

```javascript
window.location.hash = "hello"; // set URL to http://www.dom.com/#hello  
```  

Router listen for changes in the fragment identifier portion of the URL, via the window’s `onhashchange` event.  
When a change occurs, the pattern in the new hash string is compared to all the paths in each route from the router’s configuration. If a match exists, the router executes any process specified and then displays the view from the matching route  

```html
<a href="#routes/contact">Contact Us</a>
```  

### Html5 History API  

**Methods**

`history.pushState()` — add new history entries  
`history.replaceState()` — replace existing history entries with new ones  

**Parameters**  

- `State object` — An optional JavaScript object associated with the history entry  
- `Title` — Represents a new title for the history entry (though not implemented by most browsers)  
- `url` — The URL that should be displayed in the browser’s address bar  

```javascript
history.pushState({myObject: "hi"},"A Title", "newURL.html");
history.state // returns myObject
```

**Event**

```javascript
window.addEventListener("popstate", function(event) {
    console.log("popstate event fired");
});
```

---  

## VIEWS  

### Templates  

A template is a section of HTML that acts as a pattern for how our view is rendered.  
The template engine marries the data and template to arrive at what the user sees onscreen.  


#### **Inline Templates**  

- Included in the initial download of your SPA (inline)

-  To prevent the browser from execute the script you’ll give the SCRIPT tag’s (MIME) type something other than text/javascript or application/javascript  

```html
<script type="text/template" id="myTemplate">
     Hello, <%= firstName %>, how are you?
</script>
```  

#### **Template partials**    

- Downloaded on demand as external `partials` (or fragments).

### Bindings  

Is linking a UI element in the view (such a a user input) to something in the code (not just data, can be style, attribute, events)  

**One way data binding** Changes in the state of the source affect the target but not the other way around. This type of binding is normally associated with HTML elements that don’t require any input from the user, such as a DIV or SPAN tag. With these types of elements, you’re interested in its text, not its value. You still access the data on the JavaScript side in the same manner, but in the template you choose the attribute specifically for one-way text binding.  

**Two way data binding** changes on either end cause updates on the opposite side. This keeps the two sides in sync.)  

**One time** occurs only once at render time, from model to view.   


> When use:

> If your view receives input from the user, and you need the data and view to  stay constantly in sync, use two-way binding.  
> When you have read-only UI elements, use one-way binding. One-way will keep the view updated when the model changes but doesn’t bother with trying to monitor the view side, because the element is read-only.

---  

## MODULES  

[Module Pattern](/backend/nodejs/#modulos)  

### Submodules  

You use dot notation, without a var, to declare a submodule. What you’re really doing is adding a property called customer, which itself contains a module

```js
initialModule.subModule = (function () {
    ...
})();
```

### Inter-module interaction  

#### **Dependencies**  

Pass a module as a parameter.  

For example in a traditional module pattern

```javascript
var module1 = (function (module2alias, module3alias) {
    function doSomething(id) {
        // here we have access to the APIs of Dependencies
        // example,  module2alias.somefromtheAPI
    }
})(module2, module3)
```

#### **Pub/Sub**   

Based in the `Observer Pattern` where each observer is notified whenever something changes in the object it’s observing.  

on Pub/Sub one module broadcast messages to other modules who are subscribed

---  

## SERVER

---







---
