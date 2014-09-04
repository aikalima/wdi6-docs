

Angular and Rails
--

####Custom directive

http://www.befundoo.com/university/tutorials/angularjs-directives-tutorial/


####Cafe Townsend
git clone


- use of ng in application.html.erb
	- {{ user.name }} 
	- ng-class
	- ng-animate
	
- ApplicationController
	- called every single time
	- see before_filer
	- always render our one and only page
	
- Look at routes:
	- Rails routes
	- Angular routes. Cool: use of asset_path. Notice that file is coffee.rb file. Rails is the server, it servesa ll files, including all assets. If it sees .erb, it'll process it, here it turns it into coffee, then js . Very cool.
	
- Login & Session
	- inspect login.html
	- ng-show="myForm.list.$error.REQUIRED"
	- ng-disabled="{{loginForm.$invalid}}
	
- Look up services
	- $log -> **try** $log.info('logging in:' + $scope.user.name)	
	- $location - >     $log.info 'Current path' + $location.path()
	- loginResultHandler and errorhandler
		- Explore SessionService - Go over factory premise again, see how it returns factory object. See use of success, error callbacks (http://docs.angularjs.org/api/ngResource.$resource). Before calling callback, it's good practice to check if callback is actually a function.
		
- OK, we're logged in now - how about control flow. What do we do next? loginResultHandler -> $location.path '/employees'
- employees.html - add this to list to demo ng-class:     <li>Eval {{!selectedEmployee && 'disabled'}}</li>
- also nice: ng-dblclick="editEmployee()"
- ONe more ng-class, after each name: {{employee.id == selectedEmployee.id && 'active'}}
- Hover/Transition is burbon feature

- Pattern: SelectedEmployee - Share data between controllers: Create a service as a bucket holding object that you'd like to share. create/edit and employee controller all share the same SelectedEmployee.

- editEmploye: $scope.browseToOverview = ->
    $location.path '/employees' Example of how to redirect


Concept: Nexted controller:
http://www.ng-newsletter.com/posts/beginner2expert-how_to_start.html



		



