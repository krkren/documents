# Type Specification Syntax

In KiriKiri Z, typed TJS can be executed.
Specifying types does not cause errors, and the script's behavior remains exactly the same as when types are not specified. It is primarily used for features like editor IntelliSense.

The syntax is as shown in the following examples.

## Variable Declaration
A colon can be added after the variable name to specify the type.
```
var i:int;
var win:Window = new Window();
```

## Function Definition
A colon can be added after the argument name to specify the type.
The return value can be specified after the argument list.
```
function add(param1:int, param2:int) : int {
    return param1 + param2;
}
```

## Function Expressions
Similar to function definitions, argument and return value types can be specified.
```
var f = function(param1:string) : void {};
```

## Property Definition
The types for the setter argument and the getter return value can be specified respectively.
```
property prop {
    setter(value:int) {}
    getter:int { return 0; }
}
```