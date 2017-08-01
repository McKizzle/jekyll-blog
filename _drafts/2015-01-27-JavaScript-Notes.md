---
layout: pure
title: JavaScript Notes
tags: [Javascript, notes]
category: RO-Blog
date: 2015-01-27 00:00:00
---

# JavaScript Notes

## Objects 

## Functions

All functions in JavaScript are objects that are linked to `Function.prototype` which is in turned linked to `Object.prototype`. In conjunction all functions have the `prototype` property, which is not to be confused with the `prototype` it is linked to. 

### Function Syntax

Key:

  - `[optional]`
  - `<automaitic/required>`

{% highlight javascript %}

var [function name] = function(<this> arguments) {
  ...
}

{% endhighlight %}

`[function name]` is an optional value that can be left out in order to create anonymous functions, `function` is a reserved keyword, `this` will always get passed in by the interpreter, and `arguments` is the list of arguments that were passed into the function. The value of `this` is dependent on how the function is invoked.  

#### Function Invocation Patterns

##### Method Invocation Pattern

When a function is stored as a property of an object then it is called a method. 
Invoking a method will result in the value of `<this>` being the object the function belongs to. 

##### Function Invocation Pattern

When the function is called by its name directly then we have function invocation. 
`this` is bound to the global object `window`. 
Now suppose we perform a function invocation inside of a function invocation what would `this` point to? 
It would point to the global object `window`. 
There is a simple workaround. Simply store `this` into a variable called that. Simple example:

{% highlight javascript %}
myObject.double = function() {
    var that = this;

    var helper = function() {
        that.value = add(that.value, that.value);
    };

    helper();
}
{% endhighlight %}

##### Constructor Invocation Pattern

Javascript uses prototypal inheritance. 
The book will cover in more depth about the proper way to create a constructor function.

##### Apply Invocation

In JavaScript all functions are objects. 
Therefore, it is possible for functions to have methods. 
The `apply` method allows you to control the value of `this` and the arguments when they are passed into the function. 

#### Arguments

JavaScript's function arugments are the same as Perl arguments. They are passed in as an array. 

#### Return Value

A function can either return a value explicitly with `return` or implicitly by excluding a return statement. The default return value of a function is `undefined`. 

#### Augmenting Types

JavaScript allows the addition of properties and functions to the basic types. 
Augmenting types can prove to be advantageous and disadvantageous. 
First new functionality can be added, second adding new features can override other features or libraries may override features that were added. 
Augmenting a type will cause all objects to get updated, even if they were created before the Augmentation happened. 

#### Scope

Unlike most languages JavaScript _scopes_ behave differently. There exists no _block_ scope, only _function_ scopes. Hence, if defining a function with multiple loops then it is better to define the variables at the beginning.  

#### Closure

All functions are able to call the variables contained within the invoking function. This allows an object to be initialized with a function that returns a literal instead of using an object literal directly. Since the function scope is hidden it effectively hides any variables within the function. Here is an example provided from the textbook. 

{% highlight javascript %}
  var myObject = (function() { 
    var value = 0;

    return {
      increment: function(inc) {
        value += typeof inc === 'number' ? inc : 1;
      },
      getValue: function() {
        return value;
      }
    };
  }());
{% endhighlight %}

In the example above `value` is only visible to the function that represents the object. Syntactically the extra set of parenthesis do not cause a difference in how myObject is created, but it is a good practice to use the parenthesis in case the `new` keyword is used. 

#### 


