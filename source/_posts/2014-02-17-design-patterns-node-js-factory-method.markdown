---
layout: post
title: "<small>Design Patterns in Node.js:</small> Factory Method"
date: 2014-02-17 11:24:46 +0100
comments: true
categories: [Node.js, Design Patterns]
published: false
---

In the last post I have wrote about [Module Pattern in Node.js](/2014-02-12-design-patterns-node-js-module-pattern/). As I already mentioned this pattern provide base for another pattern called Factory Method. 

# Usage

In Module Pattern the constructor function simple return new instance after each call.  But we can use it also in case when we have limited poll of instances and we would like to provide client of the module with one from them. More about Factory Method pattern in Node.js you will find in a next blog post.

Factory Method provide the **data encapsulation** in the same was as Module Pattern.

In following example we simply circulate 1O instances in a round robin manner.

```javascript
// static variables
var instancesPool = [];
var maxInstances = 10;
var instanceToken = -1;

exports.getInstance = function(context) {
    function createInstance() {
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

    if (instancesPool.length < maxInstances) {
        var instance = createInstance();
        instancesPool.push(instance);
        instanceToken++;
        return instance;
    } else {

    }
}
```
