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

// constructor function
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

// constructor function
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

# Characteristics
Module Pattern comprise 2 from 4 major principles of object-oriented programming - the encapsulation and abstraction.

Firstly it **encapsulate data** and provide the client of such module with just public API methods. For JavaScript this is the easiest way how to substitute the explicit public/private variables scoping. 

Secondly the Module Pattern provide a **layer of abstraction**. It means that client using such module do not have to care about the process of the instantiation itself. He needs to know parameters of called *constructor function* and api of *returned instance*. This define an *interface* between client and module. If the interface remains the same it's implementation can evolve freely without change on a client.

For example we can incorporate into Module Pattern another pattern called Factory Method. In the code example above the constructor method `createInstance` simply returns a new object instance for every call. But we can use Factory Method when we have a limited poll of instances and we would like to provide client with one from them. All this is change of the implementation but the interface for a client do not change. More about Factory Method pattern in Node.js you will find in a next blog post.

# Usage example
I try to desing Node.js apps as a net of loosely coupled services. In such scheme every service is developed according Module Pattern and benefit from a uniform interface. In such case I use slightly modified vocabulary - `init` instead of `createInstance` and `api` instead of `instance`.

```javascript
exports.init = function(context, callback) {
    var connection;

    var api = {
        list: function(query, callback) {  /* implementation */  },
        create: function(item, callback) {  /* implementation */  },
        update: function(item, callback) {  /* implementation */  },
        destroy: function(item, callback) {  /* implementation */  }
    }

    openSomeConnection(function(err, connection_) {
        if (err) return callback(err);
        connection = connection_;
        return callback(null, api);
    });
}

```


# Further Reading 
- Book [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) by Douglas Crockford who popularized this pattern in JavaScript
