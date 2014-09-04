
Angular and Rails
--

talk about:
	- ng-class manipulating styles
	- use underscore instead of angular.forEach
	- $resources **alos talk about $http service*** http://docs.angularjs.org/api/ng.$http

**layout/application.html.erb**

```
<!DOCTYPE html>
<html ng-app="Raffler">
<head>
  <title>Raffler</title>
  <%= stylesheet_link_tag    "application", :media => "all" %>
  <%= javascript_include_tag "application" %>
  <%= csrf_meta_tags %>
</head>
<body>
<div id="container">
<%= yield %>
</div>
</body>
</html>
```

- add ng-app


**index.html.erb**


```
<h1>Raffler</h1>

<div ng-controller="RaffleCtrl">
  <form ng-submit="addEntry()">
    <input type="text" ng-model="newEntry.name">
    <input type="submit" value="Add">
  </form>

  <ul>
    <li ng-repeat="entry in entries">
      {{entry.name}}
      <span ng-show="entry.winner" ng-class="{highlight: entry == lastWinner}" class="winner">WINNER</span>
    </li>
  </ul>
  
  <button ng-click="drawWinner()">Draw Winner</button>
</div>

```

- add controller scope
- add form with ng-submit
- add ng-repeat



**javascripts/raffle.js.coffee INTERIM**


```


```


**javascripts/raffle.js.coffee FINAL**


```
app = angular.module("Raffler", ["ngResource"])

app.factory "Entry", ["$resource", ($resource) ->
  $resource("/entries/:id", {id: "@id"}, {update: {method: "PUT"}})
]

@RaffleCtrl = ["$scope", "Entry", ($scope, Entry) ->
  $scope.entries = Entry.query()
  
  $scope.addEntry = ->
    entry = Entry.save($scope.newEntry)
    $scope.entries.push(entry)
    $scope.newEntry = {}
    
  $scope.drawWinner = ->
    pool = []
    angular.forEach $scope.entries, (entry) ->
      pool.push(entry) if !entry.winner
    if pool.length > 0
      entry = pool[Math.floor(Math.random()*pool.length)]
      entry.winner = true
      entry.$update()
      $scope.lastWinner = entry
]
```

Part 2
--

Review controller scope - display vars in scope

First frist: create new dummy control

First: move factorry to factories.coffee

Looks like this

```
factoriesModule = angular.module('WDI.factories', [])

factoriesModule.factory 'Student', () ->
```

**inject too**

```
demoApp = angular.module("demoApp", ['WDI.filters', 'WDI.directives', 'WDI.factories'])

```

Now let's add star rating. 

- add rating integer to entry model
- add column to migration file
- rake db:migrate - verify in console

Want a star rating for each entry. Now that's smells like a directive. But not just simply a template, like formatted address, but we want to teach it some behavior.




###Step 1

Add the directive, and use it from the html

In our first step we will just initialize the rating directive and use it but without displaying anything.

Stub out directive:

WDI = WDI || {}
Start a new directives file!!!

… also move factory!

```
angular.module('WDI.directives', [])
  .directive 'starRating', () ->
      restrict: 'A',
      link: (scope, elem, attrs) ->
        console.log "I'm a star rating"
```  

```
  <div star-rating></div>
```

###Step 2

**Show filled stars based on bound rating value**

In our second step we display the stars as filled based on the bound value passed to the directive. Our directive would now look like this,

**add template and scope**


```
angular.module('WDI.directives', [])
  .directive 'starRating', () ->
      restrict: 'A',
      template: "<ul class='rating'>" + "<li ng-repeat='star in stars' ng-class='star' ng-click='toggle($index)'>" + "★" + "</li>" + "</ul>"
      scope:
        ratingValue: "=",
        max: "="
      link: (scope, elem, attrs) ->
        scope.stars = []
        i = 0
        while i < scope.max
          scope.stars.push {
                  filled: i < scope.ratingValue
                }
          i++
```

Scope expects a rating value - we need to define it. Let's set one in copntroller scope and pass it to directive as attribute in html

RafflCtrl
 $scope.rating = 5
 
 
in html:

```

<span star-rating rating-value="rating">

```

Step 3
--

**Set max rating, and show filled stars as per ratingValue**

Till now, we’ve covered how to use a directive to display rating stars on the screen. But these ratings just show as many stars as the ratings. What we ideally want are unfilled stars to reflect the maximum rating, and filled stars to reflect the real rating. So let us see how we would add that.

```
<span star-rating rating-value="rating" max="10"></span>

```

new link function:
```
link: (scope, elem, attrs) ->
        scope.stars = []
        i = 0
        while i < scope.max
          scope.stars.push {
                  filled: i < 5
                }
          i++
```

Step 4
--
**Change rating whenever ratingValue variable changes.** So let us see how we would go about ensuring that our directive responds to changes in the variable.


Add this to html:

```
<button ng-click="rating = 7">Change rating to 7</button>
```

Working

```
angular.module('WDI.directives', [])
  .directive 'starRating', () ->
      restrict: 'A',
      template: "<ul class='rating'>" + "<li ng-repeat='star in stars' ng-class='star' ng-click='toggle($index)'>" + "★" + "</li>" + "</ul>"
      scope:
        ratingValue: "=",
        max: "="
      link: (scope, elem, attrs) ->
        updateStars = () ->
          scope.stars = []
          i = 0
          while i < scope.max
            scope.stars.push {
                    filled: i < scope.ratingValue
                  }
            i++

       scope.$watch 'ratingValue', (oldVal, newVal) ->
           if newVal then updateStars()
```
     
Step 5
--

**Add ability to click and change a rating**

Great, so we have a rating widget that changes the rating when its binding model value changes. But we still can’t click on it to change or submit a rating. How do we do that? It’s simple, we add click event listeners to our directive. The only minor change we’ll need to make is add a function to the scope in the link function to handle the click and call it from inside the template as follows,   

**add toggle to template**

```
<li ng-repeat="star in stars" ng-class="star" ng-click="toggle($index)">
```    

Step 6
--
make readonly … skip


Step 7
--
**Add rating selected callback - Save value to DB **


We now have a fully working rating widget, that can show ratings, respond to changes and handle user clicks. This completes for the most part our AngularJS Directives Tutorial. But how about we add one last feature to it as a bonus? Let us add an event listener that gets triggered whenever the user selects a rating. This could be used to handle logic specific to a controller, like possibly saving the rating to a server, etc. Let us take a look how we would accomplish this:
     
```
scope: {
      ratingValue: '=',
      max: '=',
      readonly: '@',
      onRatingSelected: '&'
    }
```

Star rating in index it's now:
```
<span star-rating rating-value="rating" max="10" on-rating-selected="saveRating(rating,entry)">
      </span>
```
       
add this to controller

```
$scope.saveRating = (rating, entry) ->
    entry.rating = rating
    Entry.update(entry)  
```

new toggle in directive

```
      scope.toggle = (index) ->
        console.log 'toggle:' +index
        scope.onRatingSelected rating: index + 1
        scope.ratingValue = index + 1
```
         
              
        

  
    





