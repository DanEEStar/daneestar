# The Principles of Object-Oriented JavaScript

[Amazon](http://www.amazon.com/Principles-Object-Oriented-JavaScript-Nicholas-Zakas-ebook/dp/B00I87B1H8)

## Chapter 1: Primitive and Reference Types

*Primitive Types*

* Boolean
* Number
* String
* Null
* Undefined

The `typeof` operator works well with strings, numbers, Booleans, and undefined.

```
console.log(typeof "Nicholas");    // "string"console.log(typeof 10);            // "number"console.log(typeof 5.1);           // "number"console.log(typeof true);	         // "boolean"console.log(typeof undefined);    // "undefined"
```

Achtung `typeof(null) == "object"`

*Reference types*

* Array `Array.isArray`
* Date
* Error
* Function
* Object
* RegExp

### Primitive Wrapper Types

The primitive wrapper types are reference types that are automatically created behind the scenes whenever strings, numbers, or Booleans are read.

```
var name = "Nicholas";name.last = "Zakas";console.log(name.last);                 // undefined
```

What the JavaScript does behind the scenes:

```
var name = "Nicholas";var temp = new String(name);temp.last = "Zakas";temp = null;							// temporary object destroyed
var temp = new String(name);console.log(temp.last);			// undefinedtemp = null;
```

## Chapter 2: Functions

With `call` and `apply` can the `this` value be changed for a function call.

`apply` expects the arguments as an array.