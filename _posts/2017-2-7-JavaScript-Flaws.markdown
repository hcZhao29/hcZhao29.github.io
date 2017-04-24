---
layout: post
title:  "Javascript Wired Parts"
date:   2017-2-7 22:13:11
categories:
tags:
image: /assets/article_images/2017-2-7-JavaScript-Flaws/centralPark.jpg
image2: /assets/article_images/2017-2-7-JavaScript-Flaws/centralPark.jpg
---

# Javascript design flaws and wired parts

# JavaScript flaws

JavaScript is the most dominant programming language for the web. Powerful as it is, JavaScript has many design flaws that may cause confusions for both newbie and seasoned developers . So it's worth making a summary here.

### Data type and variables

* ```NaN``` can not be compared with any number, like ``` NaN === NaN; // false```, the only way to check is by using ```isNaN()```: {% highlight js %}isNaN(NaN); // true {% endhighlight %}

* ```null``` and ```undefined```: The undefined value is a primitive value used when a variable has not been assigned a value. The null value is a primitive value that represents the null, empty, or non-existent reference.

* If a variable is defined without ```var```, it is set to be a global variable, which may cause some errors that are hard to debug. This problem can be solved by adding ```'use strict';``` in the first line


### Strings
Strings are imutable, like
{% highlight js %}
var s = 'Test';
s[0] = 'X';     
alert(s); // s is still 'Test'    
{% endhighlight %}

### Conditions
JavaScript uses ```if () { ... } else { ... }``` to set conditions. But if there is only one statement in a block, ```{}``` can be removed. like
{% highlight js %}
var age = 20;
if (age >= 18)
    alert('adult');
else
    alert('teenager');
{% endhighlight %}
However, there are risks doing this:
{% highlight js %}
var age = 20;
if (age >= 18)
    alert('adult');
else
    console.log('age < 18');
    alert('teenager'); // this line is actually not inside else
{% endhighlight %}
So it is better to always include ```{}```

### Loops

- "foreach" vs "for of" vs "for in"
    - ```foreach``` is an method that is available only in Array objects. It allows you to iterate through elements of an array. When invoked it takes a callback function and invokes the callback once for every array element. The callback can access both index and value of the array elements. foreach is available only for looping arrays.
    -  ```for of``` is a new way for iterating collections. Its introduced in ES6. Earlier you had to use for or while loop to iterate through elements of an collection. For for of to work on an collection, the collection must have an [Symbol.iterator] property.
    - ```for in``` is used to loop through properties of an object. It can be any object. for in allows you to access the keys of the object but doesn’t provide reference to the values. In JavaScript object properties themselves have internal properties. One of the internal properties is [[Enumerable]]. for in will only walkthrough a property if it has [[Enumerbale]] set to true. It not used to iterate elements of an collection rather used to iterate properties of objects. For example:
        {% highlight js %}
        var a = ['A', 'B', 'C'];
        for (var i in a) {
            alert(i); // '0', '1', '2'
            alert(a[i]); // 'A', 'B', 'C'
        }
        
        {% endhighlight %}

### Map and Set
In JavaScript, Map can be inplemented with Object```{}```, buy the key has to be a string. So ES6 introduced a new type ```Map```.
[Map Reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)


### Objects
The property name has to be a valid name in order to let ```Objects.property``` to access. For example,
{% highlight js %}
var me = {
    name: 'henry'
    "university": "UD"
}
{% endhighlight %}
```"university"``` is not a valid property name here, so it has to be accessed by
```me['university']```. But name can be accessed by ```me.name```

- "this" key word:
    The this object is bound at runtime based on the context in which a function is executed:
    - when used inside global functions,this is equal to window in nostrict mode and undefinedin strict mode.
    - whereas this is equal to the object when called as an object method.
    - as a constructor
    - call and apply
    - bound functions
    - as dom event handler

this will point to global object:
{% highlight js %}
function a() {
    console.log(this);
    this.newvariable = 'hello';
}

var b = function() {
    console.log(this);   
}

a();
console.log(newvariable);
{% endhighlight %}
When a global function is called, ```this```will point to the window. Likewise, 'this' inside a function which is inside another function will be attached to the global object. 

In order to solve this problem, "self" or "that" is introduced:
{% highlight js %}
var c = {
    name: 'The c object',
    log: function() {
        var self = this;
        
        self.name = 'Updated c object';
        console.log(self);
        
        var setname = function(newname) {
            self.name = newname;   
        }
        setname('Updated again! The c object');
        console.log(self);
    }
}

c.log();
{% endhighlight %}
Self points to the object c. When ‘setname’ is called, the js will look at the scope chain and find c.



