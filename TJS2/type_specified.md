---
layout: default
title: Type Specification Syntax
---

KiriKiri Z can execute type-specified TJS.
Specifying types does not cause errors, and the script behavior is exactly the same as when types are not specified. It is used for purposes such as utilizing editor IntelliSense.

The syntax is as shown in the following examples.

## Variable Declaration
You can add a colon after the variable name to specify the type.
{% highlight javascript %}
var i:int;
var win:Window = new Window();
{% endhighlight %}

## Function Definition
You can add a colon after the argument name to specify the type.
The return value can be specified after the argument list.
{% highlight javascript %}
function add(param1:int, param2:int) : int {
    return param1 + param2;
}
{% endhighlight %}

## Function Expressions
Similar to function definitions, you can specify the types for arguments and the return value.
{% highlight javascript %}
var f = function(param1:string) : void {};
{% endhighlight %}

## Property Definition
You can specify the types for the setter argument and the getter return value respectively.
{% highlight javascript %}
property prop {
    setter(value:int) {}
    getter:int { return 0; }
}
{% endhighlight %}