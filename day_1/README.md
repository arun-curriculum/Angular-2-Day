#Introduction to Angular JS
- Angular is a great framework that allows you to write front end code in a sort of MVC pattern.
- The library wraps in many utility functions such as AJAX and data binding that are a part of the core functionality.
- Angular also gives us a clean format for writing single-page applications (SPAs).

##Install Software
- [Install Python](https://www.python.org/downloads/)

##App Setup
- Angular apps have some basic configuration that we need to do before they can be run.
- We will need the main script located [here](https://angularjs.org/).
- Because Angular uses AJAX to load in its views, it's best to run any Angular app via a server.
- We will be using simplehttpserver that we downloaded earlier to accomplish this.

##### Mac

```
python -m SimpleHTTPServer 3000
```

##### Windows

```
python -m http.server 3000
```

##Data Binding
- One of the core aspects of Angular is data binding.
- Angular is a two-way binding system. This means that data that is inputted on the HTML side is also immediately available to JavaScript.
- For example, form fields that are filled out are accessible without any additional code. How did we used to do it?
- Data binding is accomplished through the `$scope` variable, which is a common theme throughout any Angular application.
- Let's see an example:

```html
<input type="text" ng-model="userEntry" />

<div>
	{{ userEntry }}
</div>
```

- Because of this we don't have to do tons of extra work grabbing things from the DOM and adding event listeners and such.
- All data within views is accessible via `$scope`.

##Angular Modules
- Modules are like a container for different parts of the application.
- They are like the main hub that links up controllers, services, filters, directives, and more.
- This allows your code to be reusable throughout your application.
- Modules can be loaded in any order, or even in parallel, because the modules delay execution.
- They are declared on the HTML side by the `ng-app` directive.

#####Modules can be attached to elements, even the `body` or `html` tags

HTML

```html
<div ng-app="myApp">
</div>
```

JS

```javascript
var myAppModule = angular.module("myApp", []);
```

- Now this functionality is hooked up to Angular, and can be manipulated using controllers, services, factories, etc.

##Controllers
- Controllers are sort of the brain of your application.
- They are responsible for the "business logic" of the app.
- These are what interact with that `$scope` variable we saw earlier.
- Let's see an example:

```javascript
app.controller("userListController", function($scope) {
	$scope.users = [
		{
			firstname: "Arun",
			lastname: "Sood",
			age: 28,
			username: "arsood"
		},
		
		{
			firstname: "John",
			lastname: "Doe",
			age: 34,
			username: "jdoe"
		}
	];
});
```
- To sync this with the HTML we simply can add this to the `body` tag:

```html
<body ng-controller="userListController">
```

- You can attach controllers to individual elements, but bear in mind that the `$scope` variable would only pertain to functionality inside of these elements and nothing else.

##Built-In Directives
- Angular comes packed with great features that allow us to extend the functionality of our applications.
- The first directive we will look at is `ng-repeat`.
- This allows us to loop through data and display it on the page.
- Here is an example using a table row:

```html
<tr ng-repeat="todo in todos">{{todo.todoText}}</tr>
```

##Todo List Lab
- We will be building a todo list using Angular.
- The front end is already done for you [here](todo_html/).
- Use each of the concepts we learned to make your app work.
- **Bonus:** When each todo is clicked, create a strikethrough on the todo text.
- **Bonus:** Persist your data using localStorage.

##Using $http for AJAX Calls
- So far we have seen how we can use `$scope` variables to bring data into the HTML view.
- Normally though, these values aren't hard-coded. We usually use AJAX to pull dynamic values.
- This can be accomplished through the `$http` service built into Angular.
- To use the $http service we will have to "inject" this dependency into the controller:

```javascript
app.controller("userListController", function($scope, $http) {
	$http.get("url here")
		.success(function(users) {
			$scope.users = users;
		})
		
		.error(function() { //Error here });
});
```

##Wine List Lab Part 1
- In this lab we will be using Angular to send a GET request to pull a list of wines.
- Set up a new Angular project and try to make a GET request to `http://daretodiscover.herokuapp.com/wines`.
- Use the front end provided [here](wine_manager_html/) to display the wine data on the page.
- Make a POST request to the same url with the data from the add wine modal window form.

##$routeProvider
- Routes allow you to add multiple page functionality to your app.
- Views are triggered via the location hash.
- Routes are set up on the application configuration:

```javascript
app.config(["$routeProvider", function($routeProvider) {
	$routeProvider
		.when("/wines", {
			templateUrl: "templates/wine-list.html",
			controller: "wineCtrl"
		})
		.otherwise({
			redirectTo: "/wines"
		});
}]);
```

- In order to use this new ngRoute module however it has to be one of our dependencies in our module setup:

```javascript
var app = angular.module("wineApp", ["ngRoute"]);
```

#####Accessing route parameters

- In order to access parameters that are passed into a URL, you will need to inject $routeParams into your controller:

```javascript
app.controller("wineCtrl", function($scope, $http, $routeParams) {
	console.log($routeParams.id);
});
```

##Wine List Lab Part 2
- In this part we will use routing to full data about a specific wine and render the edit page.
- Here are the steps you will need to follow:
	- Step 1: Create a route to `/edit/:id`.
	- Step 2: Use the route parameter to make a GET request to `http://daretodiscover.herokuapp.com/wines/:id`.
	- Step 3: Display wine data in the form on edit.html.
	- Step 4: On submit of the form create a PUT request to the same URL as above to update the wine record.