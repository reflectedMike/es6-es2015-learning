# ES2015 Crash Course

## 3. To Var, Let, or Const

We already know how to decalare variables

```js
var name="Joe";
```

So why do we need **let** and **const**?

```js
function fire(bool){
	if(bool){
		var foo = "bar";
		console.log(foo);
	} else {
		console.log(foo);
	}
}

fire(false);
```

In this case the output will be **undefined** and this is very confusing because we should get **Reference error: foo is undefined** instead (logically!). This is called **hoisting**: this means in example that behind the scenes var foo gets **hoisted** to the top of the scope like this:

```js
function fire(bool){
	var foo; 

	if(bool){
		foo = "bar";
		console.log(foo);
	} else {
		console.log(foo);
	}
}
```

So now with this example it gets more clear why we get **undefined**. This makes sence that it is a good practice to declare all the variables at the top. Just because of this confusing situation for developers there are two new keywords **let** and **const**.

**let** and **const** are block level declarations (in between the "{" "}").

### 3.1 Let

Lets use **let** in our example:

```js
function fire(bool){
	if(bool){
		let foo = "bar";
		console.log(foo);
	} else {
		console.log(foo);
	}
}
```

fire(false);

Now we get what is logically correct - an error: **Reference error: foo is undefined**. foo is only visible inside of 

```js
if(bool){
	...
}
```

### 3.2 Const

**const** is not just a constant which cannot be changed. It's a bit more difficult and confusing than it appears to be:

```js
const names = ['John', 'Sandy'];
names = ['Frank', 'Susan'];
console.log(names);
```

This will return error **Assignment to a constant variable** as expected, BUT if:

```js
const names = ['John', 'Sandy'];
names.push('Susan');
console.log(names);
```

This will be ok! WTF?! This is an issue about **const**: it is immutable to reassignment. To force immutability for an example above do this:

```js
const months = Object.freeze([...]);
```

### 3.3 Final rules

1\. Use **var** at the top level if you use it like a global variable accessible anywhere.

2\. Default to using **let**.

3\. Use const for variables which won't change (e.g. months names)



## 4. Arrows

Here is an example

```js	
class TaskCollection {
	
	constructor(tasks = []){
		this.tasks = tasks;
	}

	log(){
		this.tasks.forEach(function(task){
			console.log(task);
		});
	}

}

class Task {}

new TaskCollection([
	new Task, new Task, new Task
]).log();
```

What happens when we apply **arrows**? .log() may now look like this:

```js
log(){
	this.tasks.forEach((task) => {
		console.log(task);
	});
}
```

In this case we have only one argument (task) so we can go further and simplify:

```js
log(){
	this.tasks.forEach(task => {
		console.log(task);
	});
}	
```

We are doing only one action in the block. So we can ommit { } like this:

```js
log(){
	this.tasks.forEach(task => console.log(task));
}
```

Wow! There is only 1 line now!

Things to remember:

**1. Arguments**

```js
// No arguments:
() => console.log()

// Only 1 argument:
task => console.log()

// 2 and more arguments:
(task, arg1, arg2) => console.log()
```

**2. Body**

```js
// Only 1 operator:
task => console.log();
// And "return" keyword is implicit. It will automatically happen.

// 2 and more opeartors:
task => {console.log(); console.log();}
```

Remeber that **this** in this case works differently:

```js
log(){
	this.tasks.forEach(task => {
		console.log(this);
	});
}
```

will output *> TaskCollection {tasks: Array[3]}* to browser console. So **this** is a TaskCollection object (**not a function** as we would expect!!!). In other words:

1\. **this** in *function(task){ console.log(this) }* will be changed

2\. **this** in *task => { console.log(this) }* will not be changed 

Example just to memorize:

```js
let names = ['Taylor', 'Jeffrey', 'Adam', 'Matt'];
names = names.map(name => `${name} is cool`);
console.log(names);
```

Will output

```js
['Taylor is cool', 'Jeffrey is cool', 'Adam is cool', 'Matt is cool']
```



## 5. Default parameters

ES6 has now support of default parameters. By example:

```js
function applyDiscount(cost, discount = .10){
	return cost - (cost * discount);
}

alert(applyDiscount(100));
```

Default value doesn't have to be a primitive value. E.g.

```js
function defaultDiscountRate(){
	return .10;
}

function applyDiscount(cost, discount = defaultDiscountRate()){
	return cost - (cost * discount);
}

alert(applyDiscount(100));
```



## 6. Rest and spread

This is about arguments and it is very simple. We take all the rest arguments passed and turn them into array. 

### 6.1 Rest agruments (... operator in function declaration)

```js
function sum(...numbers){
	// the old way function
	return numbers.reduce(function(prev, current)){
		return prev + current;
	}
}

// now we may pass any number of parameters
sum(1, 2, 3);
```

or with ES6 arrows

```js
function sum(...numbers){
	// new arrows way
	return numbers.reduce((prev, current) => prev + current)
}

// now we may pass any number of parameters
sum(1, 2, 3);
```

Just to mention about other arguments - those should be in the beggining

```js
function sum(foo, ...numbers){

}

sum('bar', 1, 2, 3);
```

### 6.2 Spread argumnets (... operator in function call)

This one is diametrically opposite to rest. E.g.

```js
// imagine the function which only accepts separated agruments
function sum(x, y){
	return x + y;
}

// but we have got an array
let numbers = [1, 2];

// now we can spread array to separate values for passing arguments separatelly (not within an array)
sum(...numbers);
```

Just like with **rest** - other parameters should go first

```js
function sum(foo, x, y){

}

let numbers = [1, 2];

sum('bar', ...numbers);
```



## 7. Template strings

With only use of \`STRING_CONTENTS_HERE\` (backticks) we can easily deal with strings:

```js
let template = `
<div class="foo">
	<p class="bar">Hello here!</p>
</div>
`

console.log(template);
```

In case we want to reference a value:

```js
let name = "Michael";

let template = `
<div class="foo">
	<p class="bar">Hello, ${name}!</p>
</div>
`

console.log(template);
```

In case we want to get rid of the white-space in front of template string we can use **trim()**;

```js
let template = `
<div class="foo">
	<p class="bar">Hello, ${name}!</p>
</div>
`.trim();
```



## 8. Awesome object enhancements 

### 8.1 Object shorthand

Lets imagine the following

```js
function getPerson(){

	let name = 'John';
	let age = 25;

	// if we want to return as an object, we must hardcode it like this:
	return {
		name: name,
		age: age
	}

}
```

This **return** hardcoding is kind of annoyng - a bit of repetition there. With ES6 we can do following: In the situation when your property name is the same as the variable then we can remove "**: name**" or/and "**: age**" entirely like below:

```js
function getPerson(){

	let name = 'John';
	let age = 25;
	
	// remove ": name" and ": age" because this property name is the same as variable name.
	// Now we can make it even nicer with one line 
	return { name, age };

}

// call of the property is the same as before
console.log(getPerson().name);
```

This one is in a lot of use. E.g. with Vue:

```js

import HomeView from './components/home-view.vue';
import Alert from './components/alert.vue';
import Notification from './components/notification.vue';

new Vue({
	components: { HomeView, Alert, Notification }

	// earlier we had to code like:
	// components: { HomeView: HomeView, Alert: Alert, Notification: Noification }
	// just a useless repetition of code 
}); 
``` 

### 8.2 Method shorthand

The following example with old style method declaration:

```js
function getPerson(){

	let name = 'John';
	let age = 25;

	return {
		name: name,
		age: age,
		greet: function(){ // ES5 way to write method
			return 'Hello, ' + this.name + '!';
		}
	}

}

console.log(getPerson().greet()); // Hello, John!
``` 

With ES6 we can shorthand method like this: get rid of "**: function**" part + we can apply template strings:

```js
function getPerson(){

	let name = 'John';
	let age = 25;

	return {
		name: name,
		age: age,
		greet(){ // ES6 way
			return `Hello, ${this.name} !`;
		}
	}

}

console.log(getPerson().greet()); // Hello, John!
```

### 8.3 Object destructuring

```js
let person = {
	name: 'Karen',
	age: 32
}

// object destructuring is like this:

let {name, age} = person;

alert(name); // 'Karen'
alert(age); // 32
```

Its like assigning object properties to a coresponding variables. Like destructure object into variables. It is simillar to php's **extract()**:

```php
extract(['name'=>'John']); // now you have variable $name
var_dump($name); // 'John'
```

This is very usefull feature. Imagine that there is an ajax request with returned data:

```js
let data = {
	name: 'Karen',
	age: 32,
	results: ['foo', 'bar'],
	count: 30
}

// The ES5 old way to fetch the results:
// let results = data.results; 
// let count = data.count;

// With ES6 we can simply:
let {results, count} = data;
// and results and count will be extracted or  in other words
// data will be destructured to results and count variables

console.log(results, count);
// > ['foo', 'bar'] 30
```

### 8.4 Object destructuring as a function agrument

We can also use object destructuring as a function argument:

```js
// The ES5 old way to fetch the results:

// simulate ajax request
function getData(data){
	var results = data.results; 
	var count = data.count;	

	console.log(results, count);
}

getData({
	name: 'Karen',
	age: 32,
	results: ['foo', 'bar'],
	count: 30
});

// > ['foo', 'bar'] 30
```

The new ES6 way:

```js
// The new ES6 way:

// simulate ajax request
function getData({results, count}){
	console.log(results, count);
}

getData({
	name: 'Karen',
	age: 32,
	results: ['foo', 'bar'],
	count: 30
});

// > ['foo', 'bar'] 30
```

How cool is that? One more example to memorize:

```js
// ES5
function greet(person){
	var name = person.name;
	var age = person.age;
	
	console.log("Hello, " + name + "! You are " + age +".");
}

greet({
	name: 'Luke',
	age: 24
})

// > 'Hello, Luke! You are 24. 
```

Now ES6 way:

```js
// ES6
function greet({name, age}){
	console.log(`Hello, ${name}! You are ${age}.`);
}

greet({
	name: 'Luke',
	age: 24
})

// > 'Hello, Luke! You are 24. 
```



## 9. Classes

Earlier if we needed objects to be a class siblings we ended up doing this:

```js
function User(username, email){
	// a constructor
	this.username = username;
	this.email = email;
	
	// this one is bad, because each object user will cause new instance of the function
	// this.changeEmail = function(newEmail){
	// }

} 

// it's better to have prototype for a method. In this case all the objects of User 
// will have this method (not like above!)
User.prototype.changeEmail = function(newEmail) {
	this.email = newEmail;
}

var user = new User('Michael', 'support@mail.com');

user.changeEmail('foo@mail.com');
``` 

### 9.1 Class & its constructor

Now with ES6 we can have a new object syntax:

```js
class User {

	// now a normal constructor!
	constructor(username, email){
		this.username = username;
		this.email = email;
	}
	
	// short method syntax from 8.2
	changeEmail(newEmail){
		this.email = newEmail
	}

}

let user = new User('Michael', 'support@mail.com');

user.changeEmail('foo@mail.com');
```

This up above is a syntax **sugar**, because **behind the scenes** changeEmail() is converted to **prototype**, but it saves us a lot of time.

### 9.2 Static methods

Whats about **static** methods? ES6 has it!

```js
class User {

	constructor(username, email){
		this.username = username;
		this.email = email;
	}

	// this method is only callable directly from a constructor
	// we can't reference it from other method like: this.register()
	static register(username, email){
		return new User(username, email);
	}
	
	changeEmail(newEmail){
		this.email = newEmail
	}

}

let user = User.register('Michael', 'support@mail.com');

user.changeEmail('foo@mail.com');
```

Now with **rest** operator:

```js
class User {

	constructor(username, email){
		this.username = username;
		this.email = email;
	}

	static register(...args){
		return new User(...args);
	}
	
	changeEmail(newEmail){
		this.email = newEmail
	}

}

let user = User.register('Michael', 'support@mail.com');

user.changeEmail('foo@mail.com');
```

### 9.3 Getters and setters

Some more features: **getters**, **setters**:

```js
class User {

	constructor(username, email){
		this._username = username;
		this._email = email;
	}

	static register(...args){
		return new User(...args);
	}
	
	changeEmail(newEmail){
		this._email = newEmail
	}
	
	// getter
	get foo(){
		return 'foo';
	}

	// setter (or a mutator)
	set email(newEmail){
		this._email = newEmail;
	}

}

let user = User.register('Michael', 'support@mail.com');

// getter call:
user.foo;
// > 'foo'

// setter call
user.email = 'mail@mail.com';
// setter is used in the background
```

### 9.4 Passing object to a function as an argument

One cool thing about classes in ES6 is that they are like a first class citizens. This means that they can be passed around just about anywhere.

```js
function log(strategy){
	strategy.handle();
}

// now we can pass strategy as an object to log() function
log(new class {
	handle() {
		alert("Using the alert strategy to log");
	}
});

// another strategy that works as well
log(new class {
	handle() {
		console.log("Using the console.log strategy to log");
	}
});
```

Or something more reusable:

```js
function log(strategy){
	strategy.handle();
}

class AlertLogger {

	handle(){
		alert("Using the alert strategy to log");
	}

}

class ConsoleLogger {

	handle(){
		console.log("Using the console.log strategy to log");
	}

}

log(new AlertLogger());

log(new ConsoleLogger());
```

## 10. ES6 Modules

Think of modules just as of simple files. Lets create a separate file TaskCollection.js.

Before days to insert some siblings of something we ended up doing this:

```html
<script src="out/main.js"></script>
<script src="src/TaskCollection.js"></script>
<script src="src/OtherThing.js"></script>
```

So in this case it was very inconvinient to maintain what one files shares from the other. The solution for this is a module format in ES6. So now, rather than doing it this way, we will define modules, export behaviour and we will import that behaviour anywhere where it's neccessary.

```js
// TaskCollection.js

class TaskCollection {

	constructor(tasks = []){
		this.tasks = tasks;
	}

	dump() {
		console.log(this.tasks);
	}

}
```

So we created TaskCollection, but we don't want this to be in a global space. We need to export or expose this behaviour outside of this file. In the past, people were trying to solve this task in a different ways, e.g. CommonJS, AMD, UMD and so on. But now we are lucky - we can immidately use **EcmaScript6 Modiles** and be happy.

Just a retrospec:

```js
// ES5
// CommonJS
module.exports = {
	foo: 'bar'
}

// AMD
define('TaskCollection', ['_'], function(){
	// module here
});
```

### 10.1 Export module

Now we don't have to worry about those to much because of ES6. And this is simply done by **export** keyword!!!

```js
// ES6
// TaskCollection.js

export class TaskCollection {

	constructor(tasks = []){
		this.tasks = tasks;
	}

	dump() {
		console.log(this.tasks);
	}

}
```

So you can create a file and anything (meaning **anything**) you want to expose to the outerworld you just prefix by **export**. So now lets import our module:

```js
// main.js
// CommonJS way
var TaskCollection = require('./TaskCollection')
```

### 10.2 Import module

Now the ES6 way to **import** module

```js
// main.js
// ES6 way
import { TaskCollection } from './TaskCollection';

new TaskCollection([
	'Go to the store',
	'Finish screencast',
	'Eat cake'
]).dump();
```

But next, at least for the time of the video (May 2016) not so many browsers accept such an syntax and cound not work with modules right ahead.

So, for now lets use a **rollup**

```
>  rollup main.js --output.format es --name "myBundle" --output.file bundle.js
```

This will compile `main.js` into `bundle.js`. Now in `main.html` we run our new file:

```html
	<script src="bundle.js"></script>
```

So if we open browser console we see:

```js
["Go to the store", "Finish screencast", "Eat cake"]
 ``` 

### 10.3 Multiple exports

This works so far, but we can go and clean the code even more! Each **module can have any number of exports**. E.g.

```js
// ES6
// TaskCollection.js

export class TaskCollection {

	constructor(tasks = []){
		this.tasks = tasks;
	}

	dump() {
		console.log(this.tasks);
	}

}

export let foo = 'bar';

export myFunc = function(){
	console.log('myFunc() call');
}
```

The **KEY THING TO UNDERSTAND HERE**: The value that we are **EXPORTING** is what we would **IMPORT**. E.g.

```js
// main.js
// ES6
import { TaskCollection, foo } from './TaskCollection';

new TaskCollection([
	'Go to the store',
	'Finish screencast',
	'Eat cake'
]).dump();
```

### 10.4 Export default

But the truth is that most of the time you're only **exporting one thing**, so in those situiations we can add keyword **default** to export.

```js
// ES6
// TaskCollection.js

export default class TaskCollection {

	constructor(tasks = []){
		this.tasks = tasks;
	}

	dump() {
		console.log(this.tasks);
	}

}
```

So, here, the only thing we are exporting is the class TaskCollection. And when we do **export default** we must clean `main.js` for default exports. We **remove** `{ }` and finally will look like this:

```js
// main.js
import TaskCollection from './TaskCollection';

new TaskCollection([
	'Go to the store',
	'Finish screencast',
	'Eat cake'
]).dump();
```

```js
// TaskCollection.js
export default class TaskCollection {

	constructor(tasks = []){
		this.tasks = tasks;
	}

	dump() {
		console.log(this.tasks);
	}

}
```

### 10.5 Combined exports

But we still can have **more exports** from the same file.

```js
// main.js
import TaskCollection, { foo } from './TaskCollection';

new TaskCollection([
	'Go to the store',
	'Finish screencast',
	'Eat cake'
]).dump();
```

```js
// TaskCollection.js
export default class TaskCollection {

	constructor(tasks = []){
		this.tasks = tasks;
	}

	dump() {
		console.log(this.tasks);
	}

}

export let foo = 'bar';
```

But, to mention again, mostly we will have just only **export default** in one file. We can write TaskCollection export in the end of file.

```js
// TaskCollection.js
class TaskCollection {

	constructor(tasks = []){
		this.tasks = tasks;
	}

	dump() {
		console.log(this.tasks);
	}

}

export default TaskCollection;
```

### 10.6 Conclusion

Finally remember:

1. 	Module is just a file which exposes or exports the behaviour.

2. 	We can import that behaviour by:<br />
	`import { EXPORT_PROP } from './file'` or<br />
	`import EXPORT_DEFAULT_PROP from './file'` or<br>
	combined `import EXPORT_DEFAULT_PROP, { EXPORT_PROP } from './file'`



## 11. Module Bundling With Rollup.js

We start module fetching (compiling) with rollup.js. Very fast, very clean output, treeshaking. Let's install rollup globally:

```bash
sudo npm install rollup --global
```

Or locally (goes to /node_modules)

```bash
npm install rollup
```

If we use `> rollup main.js --output.format esm` it will simply output the result to stdout. So we better run:

```bash
rollup --format=esm --file=main.dist.js main.js
```

So now we get single file - a **bundle**. Module bundler takes all the modules and bundles them into single file. But don't forget that it is a **module bundler** and it will not take ES6 and convert it to ES5! For this purpose we should use babel or boble. Boble is simplier than babel. But we can go further and use buble with rollup, so we search for the plugin `rollup-plugin-buble`

```bash
npm install --save-dev rollup-plugin-buble
```

Next we need to reference it in rollup config file. Create `rollup.config.js`

```js
import buble from 'rollup-plugin-buble';

export default {
	// entry: 'src/main.js',
	input: 'main.js',
	output: {
		format: 'esm'
	},
	plugins: [buble()]
}

// and run > rollup -c
```

So there is the difference.

Result **without** buble:

```js
class TaskCollection {

	constructor(tasks = []){
		this.tasks = tasks;
	}

	dump() {
		console.log(this.tasks);
	}

}

new TaskCollection([
	'Go to the store',
	'Finish screencast',
	'Eat cake'
]).dump();
```

Result with **buble**:
```js
var TaskCollection = function TaskCollection(tasks){
    if ( tasks === void 0 ) tasks = [];

    this.tasks = tasks;
};

TaskCollection.prototype.dump = function dump () {
    console.log(this.tasks);
};

new TaskCollection([
    'Go to the store',
    'Finish screencast',
    'Eat cake'
]).dump();
```

So final `rollup.config.js` will look like this:

```js
import buble from 'rollup-plugin-buble';

export default {
	// entry: 'src/main.js',
	input: 'main.js',
	output: {
		file: 'main.dist.js',
		format: 'esm'
	},
	plugins: [buble()]
}
```
