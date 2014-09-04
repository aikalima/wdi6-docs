
## Object Oriented Programming

###Quick review:

- What's an object, what's a class?

Classes are a little bit like templates. Usually, you create an instance of a class - an object. Object methods get the job done, for example blend a smoothie. 

Sometimes classes have methods too, that means classes can do things without the need to create an object. But for sake of simplicity, let's say we always want to create an instance of a class, an object.

Over the course of the week, we learned a lot about the mechanics of programming: syntax, constructs, statements etc. Today we explore the process of designing an object-oriented (OO) application.

###OO Analysis and Design

OO Analysis and Design aims to create a model of the system's functional requirements that is independent of implementation. Requirements are organized around objects. Objects integrate both behavior [methods] and state (data) [instance variables]. They are modeled after the real world objects that the system interacts with. 

Sometimes you work with a domain expert. For example, before modeling a reservation system for restaurants, you probably want to talk to a restaurant owner first.

What you do is:

- Find the objects
- Organize the objects - what are their relationships (**is_a vs has_a**)?
- Describe how the objects interact
- Define the behavior of the objects
- Define the internals of the objects


###Let's go an do that

Let's assume you are asked to write a rental management application. Let's keep it very simple. 

**Features** 
Think like: The user should be able to ... 

- create a tenant
- create a unit
- move tenant into a unit
- see a list of all vacant/occupied units
- see annual income / sqft rented

That's a good start.

Go through the narrative and highlight nouns (person, place, thing), as candidate classes and verbs (actions), as methods / behaviors.

Ask yourself how things are related. Look for common scenarios / patterns.

Create a class diagram that represents you model.

Link to rental app repository solution for reference (contains modeel diagram):

https://github.com/aikalima/wdi-ruby-rental-app



