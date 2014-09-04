
Lineup
--

Sharif & Lior

*lunch*



Interviews
--



### Warmup / History

- How are you doing today?
- What makes you want to be a software developer?
- Tell me about project X on your resume:
	- What was your role?
	- How big was the team
	- What were the biggest challenges?
	- 


###Behavioral

- Have you ever been in a situation where you got stuck? What did you do?
- Tell me about how you worked effectively under pressure.
- How do you handle a challenge? Give an example.
- Have you ever made a mistake? How did you handle it?
- Give an example of a goal you reached and tell me how you achieved it.
- Describe a decision you made that wasn't popular and how you handled implementing it.
- Give an example of how you set goals and achieve them.
- Give an example of how you worked on team.
- What do you do if you disagree with someone at work?
- Share an example of how you were able to motivate 

###Skill

- 3 Bubble gum machines. One with only red gums, one with only blue gums and a third one with blue/red mixed. Each machine has a lable red/blue/both - there are all wrong. You have on quarter - how do you find out wich machine contains which gums?

- Create a simple ruby class with two properties and one method. Simply make up one, your choice.

```

class Test
	attr_accessor :a, :b
	
	def initialize c,d
		@a = c
		@b = d
	end
	
	def do_something 
		@a + 2
	end
end

t = Test.new 1,2

puts t.do_something

```
		
		
Stuff
--

**Architecture**


Define the Rails MVC implementation using an example.

Stated Simply:

a) Model — is where the data is — the database
b) Controller — is where the logic is for the application
c) View — is where the data is used to display to the user


**Angular**

What is AngularJS

How do you bind a controller to a view

how do you bind a controller varibale to a view

what is scope?

What is a directive

What is a module

What is a factory

What are the steps of creating and using filter in Angular

**Rails**

What is Ruby on Rails

http://anilpunjabi.tumblr.com/post/25948339235/ruby-and-rails-interview-questions-and-answers

What is the convention for using ‘!’ at the end of a method name?
What is a module?
What is the purpose of environment.rb and application.rb file?

What is a filter in rails

What is ActiveRecord

Name some of the model relationships in Rails


**Ruby**

https://github.com/rhettc/ruby-interview-questions

Symbol vs String

How do define a class in Ruby

Why would you use classes / Object Oriented Design?


**Javascript**

How do define a class in JS


### General Questions

#### Ruby

- What is object oriented programming?
- What is the difference between an array and a hash?
- What is the difference between a string and symbol?

#### Rails

- What is MVC?
- How does routing work?
- How do you implement user authentication?

#### Javascript

- Where do you include your Javascript includes? (Top? Bottom? Spread out?)
- How do you debug Javascript code?
- Do you like Coffeescript? Why or why not?
- What is AJAX? Why/when would you use it?

#### CSS

- What's the difference between an ID and a class?
- How do you organize your styles?
- What's a challenge in implementing a design you've faced? How did you overcome it?
- Explain the box model.
- Explain positioning.

#### Other

- Why Rails, instead of - for example - Sinatra?
- What's next on your list of new technologies to learn?

---

### Programming Puzzles

#### Estimated Arrival Date

Scenario: An order comes in and the ship method is UPS ground which typically takes 5 business days. We want to tell the
customer what date to expect their package on. Orders made before noon, ship the same day. All others ship the next day.

Complete a function that would figure this out for us for any shipping method. The `order_created_at` variable is a standard unix timestamp. `day_count` is the number of days we expect the package to take to arrive.

```ruby
def get_arrival_date(order_created_at, day_count)
  # Your magic happens here

  # Return as a DateTime object
  return estimated_date_of_arrival
end
```

#### Recursively reverse an array.

Write a function that reverses an array. Use recursion.

#### Calculate Price

Some products in our store sell for a flat rate no matter the quantity.

Example, Monsterpods go for $20.00/each no matter how many you buy.

Other products offer discounts for buying multiple quantities.

Example, a Bottle Cap Tripod might be 1 for $5.00 or 2 for $7.00 or 10 for $30.00.

If a customer orders a quantity in-between one of the tiers, it's priced according to the last tier. So, if I ordered 4 Bottle Cap Tripods it would look at the closest tier which is 2 for $7.00, or $3.50 per tripod.

Each product stores the price tiers as a string that looks like this:

Monsterpod: 1_20  
Bottle Cap Tripod: 1_5,2_7,10_30

Given this information, your task is to fill in this function:

```ruby
def calculate_price(quantity, price_string)
  # Your magic happens here

  # Make sure the price is pretty $x,xxx.xx
  return price
end
```

Example results:

`calculate_price(1, "1_20")` would return $20.00  
`calculate_price(1, "1_5,2_7,10_30")` would return $5.00  
`calculate_price(4, "1_5,2_7,10_30")` would return $14.00

#### Search Sorted Array

Write a method to search a sorted array for a specific number.
 
What is Big O of the function?


---
		






