# CoffeeScript _Fly-by_

> CoffeeScript is a little language that compiles into JavaScript.  
> \- [coffeescript.org](http://coffeescript.org/)

CoffeeScript was inspired by Ruby and Python. It is not directly interpreted by browsers and can therefore be compiled into JavaScript. [NodeJS](http://nodejs.org/) accepts CoffeScript directly.

Regarding websites this can be done either on the client-side like shown below or to increase client-side performance on the server-side.

```html
<script src="//jashkenas.github.com/coffee-script/extras/coffee-script.js"></script>
<script type="text/coffeescript">
  # Some CoffeeScript
</script>
```

In general CoffeeScript provides the following benefits:

- It reduces to amount of code you write by a third to a half by taking syntactic advantage of white space and line breaks.
- It introduces high level functions like array comprehensions, prototype aliases and classes that further reduce code size.
- It hides the quirky behaving parts (as documented [here](https://bonsaiden.github.io/JavaScript-Garden/)) of JavaScript.
- It compiles into readable JavaScript code.
- CoffeeScript tends to run faster than handwritten JavaScript.  
This is because CoffeeScripts equivalents of forEach() and map() are compiled into faster for() loop constructs.

## Local Installation

```bash
sudo apt-add-repository ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install nodejs
sudo npm install -g coffee-script
```

## Usage in Bash Scripts

```coffee
#!/usr/bin/env coffee

# array: filename, absolute path, arguments
global.process.argv

# log to console
console.log 'Some text'

# include another Coffee/JS file
require './relativePath.coffee'
```

More in the [NodeJS documentation](http://nodejs.org/api/).

## CoffeeScript Syntax

### Comments

```coffee
# Single line comment

###
  Multi line comment
###
```

### Variables

Variables can not be declared explicitly. They are automatically attached to the current scope. Thus variables can not be shadowed.

Variables can be attached to the global scope in the following way: TODO
> If you'd like to create top-level variables for other scripts to use, attach them as properties on window, or on the exports object in CommonJS. The existential operator (covered below), gives you a reliable way to figure out where to add them; if you're targeting both CommonJS and the browser: exports ? this

### Functions

#### Named functions
```coffee
f1 = -> a * b
f2 = ->
  # additional code
  a * b
f3 = (a, b, c) -> # same here
```
```coffee
f1 => a * b
# binds to current scope
```

#### Splats
```coffee
f1 = (a...) ->
  # `a` is an array holding all given arguments
```

TODO: "real" array p. 15

### Destructuring Assignments
```coffee
o = {a: 1, b: 2}
a = [1, 2]
{c, d} = o
{e, f} = a
[g, h] = o
[i, j] = a
```

#### Return value
```coffee
f1 = -> 2
f2 = -> return 2
f3 = ->
  return 2
  return 3
# all return 2
```

#### Default values
```coffee
f1 = (a = 2) -> a * b
f2 = (a, b = 2) -> a * b
f3 = (a = 2, b...) -> a * b[0]
```

#### Invocation
```coffee
f1(a, b) == f1 a, b
f1(f2(a, b)) == f1 f2(a, b) == f1 f2 a, b
f1(f2(a), b) == f1 f2(a), b
```
```coffee
f1 != f1()
# f1 only returns the function
```
```coffee
c = [a, b]
f1 c... == f1 a, b
```
```coffee
f1 ->
  Math.max arguments...
f1 1, 2 # == 2
```

#### Anonymous functions
```coffee
-> a * b
() -> a * b
=> a * b
() => a * b
# same options concerning arguments, default values and splats
```

## Object Literals
```coffee
o1 = {a: 1, b: 2}
o2 = {a: 1, b: 2,}
o3 = a: 1, b: 2
o4 =
  a: 1
  b: 2
# o1 == o2 == o3 == o4
```

## Arrays
```coffee
a1 = [a, b]
a2 = [a, b,]
a3 =
  a
  b
# a1 == a2 == a3
```

### Ranges
```coffee
a = [1..3] # [1, 2, 3]
b = [1...3] # [1, 2]
```

### Slices
```coffee
[1 ,2 ,3][0..1] # [1, 2]
hello = "Hello there"[0..4] # "Hello"
"Hello there"[5..9] = "friend" # "Hello friend"
```

## Flow control
```coffee
if true
  "Yep, true"
if true then "Yep, true"
```
```coffee
unless false
  "Yep, false"
unless false then "Yep, false"
```
```coffee
if true is true
  "Yep, true"
if true is true then "Yep, true"
```
```coffee
if true is !false
  "Yep, true"
if true is not false
  "Yep, true"
if true isnt false
  "Yep, true"
```
```coffee
if 2 in [1, 2, 3] then true
```

### "Reversed" if/unless
```coffee
"Yep, true" if true
# the same for all constructs above
```

### Logical Operators
`0`, `""`, `null`, `undefined` == `false`

- `and` == `&&`
- `or` == `||`
- `is` == `==`
- `isnt` == `!=`
- `not` == `!`
- `or=` == `if not (exprLeft) then (exprLeft) = (exprRight)`

#### Existential Operator
- `(expr)?` returns `true` if `(expr)` is neither `null` nor `undefined`.
- `(obj)?.(prop)` checks if the property is available. If not does not try to access it.
- `(func)?()` checks if the function is available. If not does not try to call it.
- `?=` == `if (exprLeft) is in [undefined, null] then (exprLeft) = (exprRight)`

## String interpolation
```coffee
a = 2
b = "Two: #{a}" # "Two: 2"
c = "Two: #{if a is 2 then "Two"}" # "Two: Two"
```

## Comprehensions

### Arrays: "in"
```coffee
a = 0
for number in [1, 2, 3]
  a += number
for number, index in [1, 2, 3]
  a += number * index
```
```coffee
a = 0
for number in [1, 2, 3] when number > 2
  a += number
# same as above
```

#### reversed
```coffee
a = 0
a += number for number in [1, 2, 3]
a += number * index for number, index in [1, 2, 3]
```

### Objects: "of"
```coffee
a = 0
s = ""
o = a: 1, b: 2, b: 3
for letter of o
  s += letter
for letter, number of o
  a += number
```
```coffee
s = ""
o = a: 1, b: 2, b: 3
for letter of o when letter isnt "a"
  s += letter
```

#### reversed
```coffee
a = 0
s = ""
o = a: 1, b: 2, b: 3
s += letter for letter of o
a += number for letter, number of o
s += letter for letter of o when letter isnt "a"
```

## Loops
The only "low-level" loop that CoffeeScript provides is "while".

### "while" Loop
```coffee
a = b = 0
c = while a < 3
  a++
d = while b < 3 then b++ when b > 2
# c == d == [0, 1, 2]
```

## Aliases
```coffee
@var == this.var
User::prop == User.prototype.prop
```

## Classes

### Constructors
Constructors get invoked on instantiation. If the constructor arguments are prefixed by an `@` they will automatically get set as instance properties.

```coffee
class A
  constructor: (@instanceProp, argument) ->
    # constructor code
```

### Instance Property
```coffee
class A
  instanceProp1: 2
  instanceProp2: ->
    @instanceProp1 = 4 # value change
    @instanceProp3 = 2 # new instance property
A::instanceProp4 = 2 # new instance property
```
In order to bind the context of a instance method to the class use the fat arrow:
```coffee
class A
  instanceProp1 = 2
  instanceProp2: (a) =>
    @instanceProp1 = a
```

### Static Property
On the class level static properties can be accessed/declared using `this.`, the corresponding `@`-alias or with `ClassName.propName`. Within methods only with `ClassName.propName`.
```coffee
class A
  this.staticProp1: 2
  @staticProp2: 2
  A.staticProp3: 2

  instanceProp: ->
    A.staticProp1 = 3
```

### Inheritance and Super
When shadowing methods `super()` calls the corresponding method of the parent.

If the child does not define a `constructor` function the `constructor` of the parent gets called on instantiation.

```coffee
class A
  instanceProp1: 2
  constructor: ->
    @instanceProp1 += 2
  instanceProp2: (a) ->
    @instanceProp1 += a

class B extends A
  instanceProp2: ->
    super(2)

b = new B
b.instanceProp2()
# b.instanceProp1 ==
#  2 (initial value)
#  + 2 (parents constructor)
#  + 2 (method call)
```

#### Dynamic Inheritance
New instance properties are automatically inhertited even by existing instances of the children.

```coffee
class A
  instanceProp: 2

class B extends A

b = new B
A::newInstanceProp = 2
# b dynamically inherits `newInstanceProp`:
# b.newInstanceProp == 2
```
