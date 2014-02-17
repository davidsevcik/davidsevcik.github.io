---
layout: post
title: "<small>Design Patterns in Node.js:</small> Factory Method"
date: 2014-02-12 11:24:46 +0100
comments: true
categories: [Node.js, Design Patterns]
published: false
---

In following example we simply circulate 1O instances in a round robin manner.

```javascript
// static variables
var instancesPool = [];
var maxInstances = 10;
var lastUsedInstance = 0;

exports.getInstance = function(context) {
    function createInstance() {
        // ...setup instance
        return instance;
    }

    if (instancesPool.length < maxInstances) {
        var instance = createInstance();
        instancesPool.push(instance);
        return instance;
    } else {

    }
}
```
