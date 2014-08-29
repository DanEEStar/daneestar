# Google I/O 2014

### Componentize the Web (Addy Osmani)

[Youtube](https://www.youtube.com/watch?v=2toYLLcoY14)

The `shadow` tag in a polymer element injects the HTML-from the children. Somewhat like the `transclude` thing in AngularJS.

A polymer tag can extend from a builtin element like [core-selector](http://www.polymer-project.org/docs/elements/core-elements.html#core-selector).

Here is the full source code for a simple `tab` tag:

    <link rel="import" href="bower_components/polymer/polymer.html">
    <link rel="import" href="bower_components/polymer/layout.html">
    <link rel="import" href="bower_components/core-selector/core-selector.html">

    <polymer-element name="my-tabs" extends="core-selector" noscript>
      <template>
        <link rel="stylesheet" href="my-tabs.css" />
        <div horizontal layout>
          <shadow></shadow>
        </div>
      </template>
    </polymer-element>
    
And here how you can use it:

    <my-tabs selected="0">
      <span>One</span>
      <span>Two</span>
      <span>Three</span>
    </my-tabs>

### Google I/O 2014 - Making the mobile web fast, feature-rich, and beautiful (Paul Irish)

[Youtube](https://www.youtube.com/watch?v=EXjPsvwIDwU)


### Touch in a Web App? No Problem

[Youtube](https://www.youtube.com/watch?v=Rwc4fHUnGuU)

* Bind Low, bind to the element you want the user to touch
* Bind Late
* Use requestAnimationFrame to update the UI

### Responsive Images Today

[Youtube](https://www.youtube.com/watch?v=vpRsLPI400U)

* Media queries for different background-images depending on viewport size and/or resolution.
* `image-set()`
* `srcset`-attribute in `ing`-tags