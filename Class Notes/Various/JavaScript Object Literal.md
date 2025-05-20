## What is JavaScript Object Literal?

JavaScript Object Literal is a data type used to define objects in JavaScript. It is a syntax for creating an object in JavaScript that is composed of **key-value pairs**. It is a lightweight and efficient way to create and store data.
## Why should you use it?

## Object Literal

Object literals are one of the most commonly used data structures in JavaScript. They are used to store collections of data, and can be used to represent complex data structures such as objects, arrays, functions, and even regular expressions. Object literals are also used to store information about a particular instance of an object, such as its state or behavior.

Object literals are written in the form of **key-value pairs**, where each key is a string and each value can be any valid JavaScript data type. For example, the following object literal stores information about a person:

```javascript
const person = {
  name: 'John Doe',
  age: 30,
  address: '123 Main Street'
};
```

Object literals can also contain other object literals, as well as functions. For example, the following object literal stores information about a person, and also contains a function that prints out a greeting message:

```javascript
const person = {
  name: 'John Doe',
  age: 30,
  address: '123 Main Street',
  greet: function() {
    console.log('Hello, my name is ' + this.name);
  }
};
```

Object literals can be used to create objects in JavaScript. To create an object, you use the `new` keyword, followed by the constructor function of the object you want to create. For example, to create an instance of the `Person` object defined above, you would use the following code:

```javascript
const person = new Person();
```

Once an object has been created, you can access its properties and methods using the `dot` notation. For example, to access the `name` property of the `person` object, you would use the following code:

```javascript
console.log(person.name);
```

To call the `greet` method of the `person` object, you would use the following code:

```javascript
person.greet();
```
