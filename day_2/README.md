#Angular Continued

##ngAnimate
- ngAnimate is a module that allows us to perform animations on elements selectively.
- This functionality requires the ngAnimate module from the Angular extras.
- Let's take a look at the documentation [here](https://docs.angularjs.org/api/ngAnimate/).
- Animations work by applying the `.ng-enter`, `.ng-enter-active`, `.ng-leave`, or `.ng-leave-active` classes on any of your existing classes.
- Your classes should implement some sort of CSS3 animation.
- Let's try this out with some pre-built css classes to help us out: https://github.com/Augus/ngAnimate.

##Wine List Lab Part 3
- In this part of the lab we will implement some animations between views.
- Use your existing application and create an animation between the wine list and the edit view.
- Experiment using different CSS3 properties.

##Factories
- Factories allow us to create Angular code that is modular and reusable.
- We can create methods within factories that can be injected into controllers throughout a module.
- This is often used to streamline work with API calls and other functionality that has to be replicated over and over.
- Let's see a simple example:

```javascript
app.factory("Helpers", function() {
	var factory = {};
	
	factory.addTo = function(array, item) {
		return array.push(item);
	}
	
	factory.removeFrom = function(array, index) {
		return array.splice(index, 1);
	}
	
	return factory;
});
```

- Now in our controller we have:

```javascript
app.controller("wineCtrl", function($scope, Helpers) {
	$scope.wines = Helpers.addTo($scope.wines, "New Wine");
});
```

##$resource
- $resource is a higher-level abstraction on $http.
- It gives us a few pre-defined routes that we can use to easily query a server backend.
- $resource assumes a classical RESTful architecture of our backend service.
- First thing we need to do to use it is include it as a dependency in our module:

```javascript
var app = angular.module("wineApp", ["ngResource"]);
```

- We can then set up our resource in our controller:

```javascript
app.controller("wineCtrl", function($scope, $http, $resource) {
	var Wine = $resource("http://daretodiscover.herokuapp.com/wines/:id", {
		id: "@id"
	}, {
		update: {
			method: "PUT"
		}
	});
});
```

- This sets up Wine as a resource with the following methods:
	- `query()` - GET /wines
	- `get()` - GET /wines/:id
	- `save()` - POST /wines
	- `update()` - PUT /wines/:id
	- `remove()` - DELETE /wines/:id
	- `delete()` - DELETE /wines/:id

##Wine List Lab Part 4
- For this lab we will be refactoring our API query code to use resources.
- Your job is to remove the $http calls and instead use $resource.
- You will also be breaking out the logic for these calls into factories.
- You may want to refer to web tutorials to help you:
	- [Angular guide](https://docs.angularjs.org/api/ngResource/service/$resource)
	- [CRUD app with resources](http://www.sitepoint.com/creating-crud-app-minutes-angulars-resource/)

##Filters
- Filters allow you to alter $scope data to transform it in some way.
- Think of it like we are passing the data through some filter before it is shown to the user or used in the JavaScript.
- Let's look at an example that will take numbers entered into an input box and multiply them by 2:

HTML

```html
<input type="text" ng-model="numbers" />
{{numbers | multiply}}
```

JS

```javascript
app.filter("multiply", function() {
	return function(input) {
		if (input) {
			var num = parseInt(input);

			return num * 2;
		} else {
			return "";
		}
	}
});
```

##Custom Directives
- We have already seen how directives enhance the functionality of the page.
- If we want to create custom functionality that can be reused throughout the app we can register our own custom directive.
- Directives can be declared via element, attribute, or class.
- The Angular team recommends using either the element or attribute form for intuitiveness.

HTML

```html
<my-todos></my-todos>
```

JS

```javascript
app.directive("myTodos", function() {
	return {
		templateUrl: "templates/todos.html",
		controller: "todoCtrl"
	}
});
```

- You can split your templates into multiple files, essentially creating a partial.
- templateUrl can be a string as in the above, but it can also be a function that gets access to the element and any related attributes.
- Imagine we had an attributes we wanted to use:

HTML

```html
<my-todos type="awesome"></my-todos>
```

JS

```javascript
app.directive("myTodos", function() {
	return {
		templateUrl: function(elem, attr) {
			console.log(elem);
			console.log(attr.type);
			return "templates/todos.html"
		},
		controller: "todoCtrl"
	}
});
```

#####Restricting directive types
- By default Angular directives are restricted to element and attribute types.
- If you want to use a directive that is triggered by a class name you will have to use the `restrict` option.
- Restrict supports three types - A, E, and C for attribute, element, and class name respectively.
- Let's see an example where we want the directive to only be available as a class name:

HTML

```html
<div class="my-todos" type="awesome"></div>
```

JS

```javascript
app.directive("myTodos", function() {
	return {
		restrict: "C",
		templateUrl: function(elem, attr) {
			console.log(elem);
			console.log(attr.type);
			return "templates/todos.html"
		},
		controller: "todoCtrl"
	}
});
```

##Movie Search Lab
- For this lab we will be building an application that can search for movies in a database and display the results.
- The frontend is already done for you [here](movie_starter_app/).
- For this project we will imagine that we want to build a widget that can pull this movie information, and that we want to use this functionality throughout our app.
- Your job is to create a directive for the search function of this app and have it render on the page.
- To hide and show the search box you will have to look into [ngShow](https://docs.angularjs.org/api/ng/directive/ngShow).