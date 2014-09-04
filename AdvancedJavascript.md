Advanced Javascript
--

### Objects

Objects in Javascript are class less. No 'class def' like in ruby or most other OO languages.

When you think about objects in JavaScript, simply think about hash tables of key-value pairs (similar to what are called “associative arrays” in other languages). **A bag of properties**. The values can be primitives or other objects; in both cases they are called properties. The values can also be functions, in which case they are called methods.

You can start with a blank object and add functionality to it as you go. The object literal notation is ideal for this type of


**Object Literal notation**

```
// Object Literal notation - Object is a bag of properties. Fucxntions can be properties - they
// become object methods
// If you need to make an object on the fly (see tic tac toe)
var dog = {
  name: 'Loui',
  breed: 'Lab',
  bark: function(){
    console.log('ruff-ruff');
  }
}

console.log(dog.name)

var p = Object.create(dog);
p.bark();
// You can also delete property
delete(dog.bark);
p.bark();


```

**Constructor notion**

Creators of JavaScript felt like that they have to have more traditonla notion of OO. Slapped that one. But **new** suggest classes, but there are no classes.

In addition to the object literal pattern and the built-in constructor functions, you can create objects using your own custom constructor functions, as the following example demonstrates: 

**notice use of this**

```
// Constructor functions and prototypes
var Dog = function (name,breed){
  this.name = name,
  this.breed = breed
  this.bark = function() {
  	console.log('ruff ruff');
  	}
}

kellysDog = new Dog('sam','richback');
laurensDog = new Dog('max','mix');

```

This new pattern looks very much like creating an object in Ruby using a class called Dog. The syntax is similar, but actually in JavaScript there are no classes and Dog is just a function.

**Object Prototyping**

For simplicity in this example, the bark() method was added to this. The result is that any time you call new Dog() a new function is created in memory. This is obviously inefficient, because the bark() method doesn’t change from one instance to the next. 

The better option is to add the method to the prototype of Dog: 

```
Dog.prototype.bark = function() {
  return console.log('riff-raff');
}

kellysDog.bark()

laurensDog.bark()

// Great Feature: Extending Array
a = [1,2,3,4]

Array.prototype.first = function(){
  return a[0];
}
console.log(a.first())

```
Just remember that reusable members, such as methods, should go to the prototype. Note that prototypes can be added/changed anytime and affect existing instances.


**Inheritance**

```
//Shape - superclass
function Shape() {
  this.x = 0;
  this.y = 0;
}

//superclass method
Shape.prototype.move = function(x, y) {
    this.x += x;
    this.y += y;
    console.info("Shape moved.");
};

// Rectangle - subclass
function Rectangle() {
  Shape.call(this); //call super constructor.
}

//subclass extends superclass
Rectangle.prototype = Shape.prototype;
Rectangle.prototype.constructor = Rectangle;

Rectangle.prototype.circumference = function() {
    return 2 * this.x + 2 * this.y;
};

var rect = new Rectangle();
rect.x = 3;
rect.y = 4;

console.log(rect instanceof Rectangle) //true.
console.log(rect instanceof Shape) //true.

rect.move(1, 1); //Outputs, "Shape moved."
console.log(rect.circumference());

```

###Synchronizing functions

Ajax calls are asynchronous, i.e. you don't know in advance when they are done. Ajax signals completion by calling success() function. This function has different scope from the rest of the program, i.e this != this. 

**Problem:** how to pass results of ajax call back to main program scope. Answer: callbacks. Let's look at an example.

look at callbackfromhell website - pyramid of doom

```
loadData = (callback) ->
 alert 'Loading la data'
 #pretend to make ajax call, we don't know when it's done
 countdown = (num for num in [10000000..1])
 callback()
 

renderUI = () ->
 alert 'Rendering le UI!'

loadData(renderUI)
```

**CookBook example**

```
$(document).ready(function(){

  var loadData = function(callback){
    $.ajax(
      '/data.json',
      {
        success: function(graph_data) {
          callback(graph_data);
        }
      }
    );
  }

  var drawChart = function (graph_data){
      var container = $('.chart');
      var colors = ['red','skyblue','green','gold'];
      container.append('<div id="cook_chart" class="graph"/>');
      console.log(graph_data);
      Morris.Bar({
        element: 'cook_chart',
        data: graph_data,
        xkey: 'y',
        ykeys: ['a'],
        labels: ['count']
      });
  }

  loadData(drawChart);

});
```

###Namespaces

As the complexity of a program grows and some parts of code get split into different files and included conditionally, it becomes unsafe to just assume that your code is the first to define a certain namespace or a property inside of it. 
 
Some of the properties you’re adding to the namespace may already exist, and you could be overwriting them. 

3rd partly libs usually define their own namespace, examples:

- underscore -> _
- LeafLet -> L
- jQuery -> $


**Best practice** (for larger js projects, libs): Define a namespace!

Example:
*it’s best to check first that it doesn’t already exist*
```
var MYAPP = MYAPP || {};
```

Example usage:

```
WDI.Person = (name) -> 
  @name = name
  @say = () -> 
   return "I am " + @name
  @

stud1 = new WDI.Person('Karl')
stud2 = new WDI.Person('Lauren')

alert stud1.say()
alert stud2.say()
```









