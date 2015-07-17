#Angular Continued

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

##Wine List Lab Part 3
- For this lab we will be refactoring our API query code to use resources.
- Your job is to remove the $http calls and instead use $resource.
- You may want to refer to web tutorials to help you:
	- [Angular guide](https://docs.angularjs.org/api/ngResource/service/$resource)
	- [CRUD app with resources](http://www.sitepoint.com/creating-crud-app-minutes-angulars-resource/)

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