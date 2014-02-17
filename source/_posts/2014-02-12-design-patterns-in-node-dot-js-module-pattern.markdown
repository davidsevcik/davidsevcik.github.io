---
layout: post
title: "<small>Design Patterns in Node.js:</small> Module Pattern"
date: 2014-02-12 10:17:20 +0100
comments: true
categories: [Node.js, Design Patterns]
---

Today I start the series of blog spots dedicated to design patterns used in Node.js development. As developers you are probably familiar with the [GoF design patterns](http://sourcemaking.com/design_patterns). These work best with the programming languages using class inheritance but JavaScript belongs to the [prototype-based](http://en.wikipedia.org/wiki/Prototype-based_programming) category of languages. Moreover the Node.js is specific for an asynchronism. Because of this I see useful to revise already known patterns from GoF and add some more specific to JavaScript or Node.js. 

There is no order of particular patterns in this series. I plan to simply pick those which I use the most in my current projects. For start I have chose a Module Pattern.

# Code first

```javascript Synchronous version
var privateStaticVariable = 'sample';

exports.createInstance = function(context) {
    var privateVariable = 10;

    var privateFunction = function() {
        return privateVariable * context.multiplier;           
    }

    var instance = {
        publicVariable: 7,
        publicFunction: function(x) {
            return privateVariable + x;
        },
        chainableFunction: function(sum) {
            privateVariable = instance.publicFunction(sum);
            return instance;
        }
    }

    return instance;
}
```


```javascript Asynchronous version with callback
var privateStaticVariable = 'sample';

exports.createInstance = function(context, callback) {
    var privateVariable = 10;

    var privateFunction = function() {
        return privateVariable * context.multiplier;           
    }

    var instance = {
        publicVariable: 7,
        publicFunction: function(x) {
            return privateVariable + x;
        },
        chainableFunction: function(sum) {
            privateVariable = instance.publicFunction(sum);
            return instance;
        }
    }

    someAsyncCall(function(err) {
        if (err) return callback(err);
        return callback(null, instance);
    });
}
```

# Usage
I use Module Pattern as a basic building block in my Node.js apps. Firstly it **encapsulate data** and provide the client of such module with just public API methods. For JavaScript this is the easiest way how to substitute the explicit public/private variables scoping. 

Secondly the Module Pattern **incorporate the [Factory Method](http://sourcemaking.com/design_patterns/factory_method) pattern**. In the code example above the factory method `createInstance` plays role of simple constructor, for every call returns a new object instance. But we can use it also in case when we have limited poll of instances and we would like to provide client of the module with one from them. More about Factory Method pattern in Node.js you will find in a next blog post.


# Further Reading 
- Book [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) by Douglas Crockford who popularized this pattern in JavaScript
