
CoffeeScript notes
==

Goal: I want you to never write a single line of Javascript in your life.

##Goals
- Introduce CofeeScript to write JavaScript
- Install coffee script console
- get familiar with syntax
- convert existing js program to coffee


##Intro

CoffeeScript is a language that compiles into JavaScript. It’s a bit like the love child of Ruby, JavaScript, and Python.

CoffeeScript is white-space-sensitive, just like Haml. You can embed JavaScript in CoffeeScript. In CoffeeScript, you can use string interpolation.

##Installation

CoffeeScript is build in the rails stack. It's a gem. However, we'd like to play around with CoffeScript in a console - ruby style - and also need to compile coffee to js.

- Install NodeJs and npm - [Nodejs](http://http://nodejs.org/) . Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications. We'll get to node later. [Install Options](https://gist.github.com/isaacs/579814)

- Now install Coffee: 
	`npm install -g coffee-script`

Note about Rails: coffee is a gem - no need to worry about

**Sublime autocompile on save**

- https://github.com/alexnj/SublimeOnSaveBuild
- http://stackoverflow.com/questions/15652758/install-plugins-to-sublimetext-2

- Sublime -> Pref -> Package Control -> Install Package
- SublimeOnSaveBuild
- In ~/Library/Application Support/Sublime Text 2/Packages/SublimeOnSaveBuild , put 

```
{
  "cmd": [
    "coffee",
    	"-c",
    	"-b",
    	"$file_name"
  ],
  "selector": "source.coffee"
}

```

in CoffeeScript.sublime-build

**Now when working on coffee file, hit Ctrl-B to save and build. See .js file**


##Overview


**We we use coffee script site to play:http://coffeescript.org/**


**To create a variable, it is just like Ruby:**

in you shell, type coffee to launch coffee shell. Let's go.

```
number   = 42
opposite = true
```

**Conditionals are also like Ruby, but do not require an end**

```
number = -42 if opposite
```

**Functions**

JS:

```
var square = function(x)
{
	return x * x;
}
```

Coffee:

```
square = (x) -> x * x
```

-> marks start of function body. Move it to next line, but be aware of space-indentation

```
square = (x) -> 
	x * x
```
No need to use params or def/end like construct, but you have to get indentation right

countdown = (num for num in [10..1])

alert countdown

**Arrays**

Arrays are the same in Ruby and CoffeeScript and JavaScript (OK, JS has a ;).

`list = [1, 2, 3, 4, 5]`

**Objects**

One way of looking at objects in JS is that they are simply hashes, you can create objects on the spot (without having a class). That's called object literals.

JS:


```
dog = {
	name: ‘fido’,		 
	breed: ‘lab',
	bark: (){console.log('ruff-ruff')} 
)}
```

Coffee:

```
dog =
	name: ‘fido’		 
	breed: ‘lab'
	bark: () -> console.log 'ruff-ruff'
```

Usage:

```
dog.name
dog.bark()
```


**Existance**

Coffee:

```
alert "I knew it!" if elvis?
```

JS:

```
if (typeof elvis !== "undefined" && elvis !== null) {
  alert("I knew it!");
}
```

**Array comprehension**

```
cubes = (math.cube num for num in list)
```

JS:


```
cubes = (function() {
  var _i, _len, _results;
  _results = [];
  for (_i = 0, _len = list.length; _i < _len; _i++) {
    num = list[_i];
    _results.push(math.cube(num));
  }
  return _results;
})();

```
cube is turned into an immediate function, it's defined and executed at the same time.

List is an array:

`[1, 2, 3]`

The first number in list is passed into num, which is the argument for the cube function, so the output for the variable 

cubes is :

`[1, 8, 27]`

This is example is similar to the map function. We can extend this to include filtering:

`cubes = (math.cube num for num in list when num < 4)`

List is an array:

`[1, 2, 3, 4, 5, 6, 7]`

The output is :

`[1, 8, 27]`

**String Interploation**

```
author = "Wittgenstein"
quote  = "A picture is a fact. -- #{ author }"

sentence = "#{ 22 / 7 } is a decent approximation of π"
```

**Chained Comparison**

```
cholesterol = 127

healthy = 200 > cholesterol > 60
```

**Case statement**

```
score = 76
grade = switch
  when score < 60 then 'F'
  when score < 70 then 'D'
  when score < 80 then 'C'
  when score < 90 then 'B'
  else 'A'
```

**Classes / Inheritance**

What did I say earlier: JavaScript is class-less?

JavaScript's prototypal inheritance has always been a bit of a brain-bender, with a whole family tree of libraries that provide a cleaner syntax for classical inheritance on top of JavaScript's prototypes: Base2, Prototype.js, JS.Class, etc. The libraries provide syntactic sugar, but the built-in inheritance would be completely usable if it weren't for a couple of small exceptions: it's awkward to call super (the prototype object's implementation of the current function), and it's awkward to correctly set the prototype chain.

Instead of repetitively attaching functions to a prototype, CoffeeScript provides a basic class structure that allows you to name your class, set the superclass, assign prototypal properties, and define the constructor, in a single assignable expression.

Constructor functions are named, to better support helpful stack traces. In the first class in the example below, this.constructor.name is "Animal".


```

class Animal
  constructor: (@name) ->

  move: (meters) ->
    alert @name + " moved #{meters}m."

class Snake extends Animal
  move: ->
    alert "Slithering..."
    super 5

class Horse extends Animal
  move: ->
    alert "Galloping..."
    super 45

sam = new Snake "Sammy the Python"
tom = new Horse "Tommy the Palomino"

sam.move()
tom.move()

```

**Combining Coffee and jquery and underscore**


```
releaseInventory = () ->
	if !isValid() then return
	ids = _.map orders, (i) -> i.id
	reasonText ?= ""
	releaseInventory ?= "false"
	
	postParams:
		ids: ids
		holdReason: reasonText
		releaseQuantity: releaseInventory
		
	newOrders = orderService.changeStatus 
		{ 	action: $s.action,
			postParams:	postParams, 
			newStatus: $s.newStatus.name }, ->
				err = $s.responseHasErrors(newOrders)
				if not err?
					if (newOrders.orders?.length? > 0)
					$s.items = newOrders.orders
					$s.$emit 'orderStatusChangeComplete', $s.newStatus				else
					$s.displayMessage 'Error', err.message , 'alert-error', 16000
					$('#onHoldDialog').modal 'hide'


```


  
In HTML
--

```
mkdir coffee-simple
cd !$
index.html:
<html>
<script src="simple.js" ></script>
</html>

echo alert 'hi coffee' > simple.coffee
coffee -c simple.coffee
```

In Rails
--
Simply rename .js to .coffee.js and replace js with CoffeeScript. Rails takes care of the compilation process.


LAB - Cookbook sample - version 1 or 2
--

```
$(document).ready ->
  $.ajax "/data.json",
    success: (graph_data) ->
      container = $(".chart")
      colors = ["red", "skyblue", "green", "gold"]
      container.append "<div id=\"cook_chart\" class=\"graph\"/>"
      console.log graph_data
      Morris.Bar
        element: "cook_chart"
        data: graph_data
        xkey: "y"
        ykeys: ["a"]
        labels: ["count"]
```

```
$(document).ready ->
  loadData = (callback) ->
    $.ajax "/data.json",
      success: (graph_data) ->
        callback graph_data


  drawChart = (graph_data) ->
    container = $(".chart")
    colors = ["red", "skyblue", "green", "gold"]
    container.append "<div id=\"cook_chart\" class=\"graph\"/>"
    console.log graph_data
    Morris.Bar
      element: "cook_chart"
      data: graph_data
      xkey: "y"
      ykeys: ["a"]
      labels: ["count"]


  loadData drawChart
```

Literate CoffeeScript - cool:
https://gist.github.com/jashkenas/3fc3c1a8b1009c00d9df


hw
--
Task: Convert to CofeeScript the JS that makes ajax call to get data for Morris chart.



