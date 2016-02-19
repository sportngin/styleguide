---
layout: page
title: JavaScript
header: JavaScript Style Guide
group: navigation
---
{% include JB/setup %}

If you can't find an answer in here, use your best judgement and keep things readable. Strive for terse, readable code.

## Table of Contents

* [Semicolons](#semicolons)
* [Code Layout](#code-layout)
* [Naming](#naming)
* [Variable Declarations](#variable-declarations)
* [Block Style](#block-style)
* [Comments](#comments)
* [Whitespace](#whitespace)
* [Object Literals](#object-literals)
* [Array Literals](#array-literals)

## Semicolons
* **Don't use them.** ASI takes care of 99.9995% of cases where a semicolon is required.

  The only edge case is when a new line starts with a left (open) parenthesis '('. And the only reason to start a new
  line with an open paren is to wrap a file in a self executing function to create a closure. If you have to start a
  line with an open paren for any other reason, you're doing it wrong.

## Code Layout

* Use two **spaces** per indentation level. No hard tabs.

* **What to indent**
  - Contents of a block
  - Contents of a multiline object literal
  - Contents of a multiline array literal

* **Line length:** Try and stay under 80 characters per line. Don't break up expressions within an if statement
  whenever possible.

## Naming
* Don't use single letter variable names if you can avoid it. It's not necessary to be overly descriptive either. A
single letter name in the scope of a very small function probably isn't a big deal. Nor is using i for an iterator.
*Just don't be a bad programmer and everything will be fine.*

* Variables and functions use lower camel case: `thisIsLowerCamelCase`
* Constructors use upper camel case: `ThisIsUpperCamelCase`
* **Do not** use underscores whenever possible.

## Variable Declarations
* Use mulitple var statements:

  ```javascript
  var pointX = 2
  var pointY = 4
  var pointZ = 0
  ```

## Block style
* Conditional blocks should be formatted with an *'uncuddled'* `else` in the case of larger blocks of code:

  ```javascript
  if (x == y) {
      doSomthing(x)
      callMethod()
      x += y
      y = 0
      doSomethingElse(y)
  }
  else {
      callMethod2()
      x++
      someMethod()
  }
  ```
* In the case of smaller blocks containing only one or two lines, use a *'cuddled'* `else`:

  ```javascript
  if (x == y) {
      return x
  } else {
      return y
  }
  ```

* You may also omit the braces in the case of single line conditions. This is mostly a grandfathered style,
  as remember: If someone adds a statement to the block, it will break without braces.

  ```javascript
  if (x == y)
      return x
  else
      return y
  ```

* All `switch` statement cases must end with `break`, `throw`, `return`, or a comment indicating a fall-through.

  ```javascript
  //bad
  switch(value) {
    case 1:
        doSomething();

    case 2:
        doSomethingElse();
        break;

    default:
        doDefaultThing();
  }

  //good
  switch(value) {
      case 1:
          doSomething();
          //falls through

      case 2:
          doSomethingElse();
          break;

      default:
          doDefaultThing();
  }
  ```

* In addition to the previous rule on `switch` format, be mindful in the use of `switch` at all. `switch` statements in
  JavaScript, C, and other languages (excluding Ruby) exhibit behavior akin to [goto statements](http://en.wikipedia.org/wiki/Goto)
  due to their fallthrough nature. This goes against principles of [structured programming](http://en.wikipedia.org/wiki/Structured_programming).
  There is always a different control structure which will do the job. Unless it is blatantly obvious to use `switch`, don't use it. 
  Read [Dijkstra's paper](https://files.ifi.uzh.ch/rerg/arvo/courses/kvse/uebungen/Dijkstra_Goto.pdf) on goto and structured programming
  if you're curious about this topic.

## Comments
* Block comments should only be used as a banner for a file, or as documentation of a function.

* Function documentation should be well formatted for automatic parsing and generation of documentation. One blank space
  should be inserted between the block comment and the prior function, as well as between the block comment and the
  function it is describing.

  ```javascript
  /**
   * Description of the method
   *
   * @param {Type} paramName
   * @param {Type} paramName2
   * @return {Type}
   * @api public|private
   */

  function doStuff(paramName, paramName2) {
      ...
      return something
  }
  ```

  **Examples**

  ```javascript
  /**
   * Squares the given number
   *
   * @param {Number} number
   * @return {Number}
   * @api public
   */

  function square(number) {
      return number * number
  }

  /**
   * Cleans up any long lived resources
   *
   * @api private
   */

  function cleanUp() {
      ...
  }
  ```

* Single line comments should appear as documentation within methods and around larger blocks of code.

  ```javascript
  function square(number) {
      // multiply the number by itself
      return number * number
  }
  ```

### Whitespace
* **Named functions:** Space after `function`. No space between the method name and opening paren. One space after
  each comma in the signature. One space after the closing paren.

  ```javascript
  function methodName(param1, param2) {
      ...
  }
  ```

* **Anonymous functions:** No spacebetween function and opening paren. One space after each comma in the signature.
  One space after the closing paren.

  ```javascript
  function(param1, param2) {
      ...
  }
  ```

* **Operators:** One space on either side of the operator except for increment, decrement, bitwise not and logical not.

  ```javascript
  // operators wrapped in spaces
  x = 1
  x == 1
  x > 1
  x + 1
  x % 2
  x + 3
  x && y
  x || y
  x | 0

  // operators without spaces
  x++
  x--
  ~x
  !x

  // example
  x == 4 ? x++ : x--
  ```

  **Exceptions:** When converting a type (Date to String, for example) the addition operator is often used as a
  shortcut. It is permissible to omit the space between the operator and operand in this case. Example:

  ```javascript
  var msSinceEpoch = +new Date
  ```

## Object Literals
* New empty object literals should be instantiated with `{}` instead of `new Object()`

* With data, place a space after the opening brace, before the ending brace, and after each comma. Spaces around
  the colon can be omitted for brevity and still remain quite readable.

  ```javascript
  var myObj = { name:'John', city:'Minneapolis' }
  ```

* If the declaration runs over 80 characters, or contains an anonymous function declaration, it is best to break
  the statement up into multiple lines. Line breaks should appear after the opening brace, after each comma and
  before the closing brace. The content of the object literal should be indented two spaces and the closing brace
  should not be indented.

  ```javascript
  var myObj = {
      firstname: 'John',
      lastname: 'Doe',
      city: 'Minneapolis',
      url: 'http://tstmedia.com'
  }
  ```

* A blank space should precede each function property if the object is a definition for a class prototype or
  contains several function properties.

  ```javascript
  var Person.prototype = {

      walk: function(speed) {
          ...
      },

      talk: function(phrase) {
          ...
      }
  }
  ```
## Array Literals
* New empty array literals should be instantiated with `[]` instead of `new Array()`

* Place a space after the opening bracket, before the closing bracket, and after each comma.

  ```javascript
  var myArr = [ 5, 4, 5, 6, 7, 1 ]
  ```

* If the declaration runs over 80 characters or contains anonymous functions, it is best to break the statement
  up into multiple lines. Line breaks should appear after the opening bracket, after each comma, and before the
  closing bracket. The array content should be indented two spaces and the closing bracket should not be indented.

  ```javascript
  var whitelist = [
      'teamID',
      'name',
      'abbreviation',
      'color1',
      'color2'
  ]
  ```
  
## Revealing Module Pattern
  
* Revealing module pattern provides a great way of explicitly defining an api for an object. The enclosing function
  provides a closure for state, but the object return provides interface. This pattern is frequently used in our
  Angular applications.  This is a good practice when developing your javascript objects. 
    
  ```javascript
  function revealMeAnObject() { 
      var somePrivateThingy = 'private'
      var somePublicThingy = 'public'
      
      return {
        somePublicThingy: somePublicThingy,
        somePublicMethod: function(){
          console.log("called a public method")
        }
      }
  }
  ```
  
* While the above example is revealing module pattern, it can suffer from a bloated return object where there is
  too much implementation logic within the object and it makes your API hard to discern. You can refactor this method
  using named functions instead of anonymous functions. What you end up with is a tight object block that defines
  your interface.
     
  ```javascript
  function revealMeAnObject() { 
    var somePrivateThingy = 'private'
    var somePublicThingy = 'public'
      
    function myInternalMethod(){
      console.log("called a public method")
    }
      
    return {
      somePublicThingy: somePublicThingy,
      somePublicMethod: myInternalMethod
    }
  }
  ```



