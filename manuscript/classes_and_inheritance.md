# classes and inheritance

**OO** keywords is probably the most awaited features in ES6. **Classes** are something like another syntactic sugar over the prototype-based OO pattern. We now have one, concise way to make class patterns easier to use.

**Over the prototype-based OO pattern to ensure backwards compatibility.**

## Overview — an example of ES6 class syntax and ES5 equivalent

```javascript
class Vehicle {

  constructor (name, type) {
    this.name = name;
    this.type = type;
  }

  getName () {
    return this.name;
  }

  getType () {
    return this.type;
  }

}

let car = new Vehicle('Tesla', 'car');
console.log(car.getName()); // Tesla
console.log(car.getType()); // car
```

It's naive example, but we can see a new keywords as class and constructor.

ES5 equivalent could be something like this:

```javascript
function Vehicle (name, type) {
  this.name = name;
  this.type = type;
};

Vehicle.prototype.getName = function getName () {
  return this.name;
};

Vehicle.prototype.getType = function getType () {
  return this.type;
};

var car = new Vehicle('Tesla', 'car');
console.log(car.getName()); // Tesla
console.log(car.getType()); // car
```

**Classes support prototype-based inheritance, super calls, instance and static methods and constructors.**

It's simple. We instantiate our classes the same way, but let's add some..

## inheritance

..to it and start from ES5 example:

```javascript
function Vehicle (name, type) {
  this.name = name;
  this.type = type;
};

Vehicle.prototype.getName = function getName () {
  return this.name;
};

Vehicle.prototype.getType = function getType () {
  return this.type;
};

function Car (name) {
  Vehicle.call(this, name, 'car');
}

Car.prototype = Object.create(Vehicle.prototype);
Car.prototype.constructor = Car;
Car.parent = Vehicle.prototype;
Car.prototype.getName = function () {
  return 'It is a car: '+ this.name;
};

var car = new Car('Tesla');
console.log(car.getName()); // It is a car: Tesla
console.log(car.getType()); // car
```

And now look at the ES6 version:

```javascript
class Vehicle {

  constructor (name, type) {
    this.name = name;
    this.type = type;
  }

  getName () {
    return this.name;
  }

  getType () {
    return this.type;
  }

}

class Car extends Vehicle {

  constructor (name) {
    super(name, 'car');
  }

  getName () {
    return 'It is a car: ' + super.getName();
  }

}

let car = new Car('Tesla');
console.log(car.getName()); // It is a car: Tesla
console.log(car.getType()); // car
```

We see how easy is to implement inheritance with ES6. It's finally looking like in other OO programming languages. We use **extends** to inherit from another class and the **super** keyword to call the parent class (function). Moreover, **getName()** method was overridden in subclass **Car**.

**super — previously to achieve such functionality in Javascript required the use of call or apply**

## static

```javascript
class Vehicle {

  constructor (name, type) {
    this.name = name;
    this.type = type;
  }

  getName () {
    return this.name;
  }

  getType () {
    return this.type;
  }

  static create (name, type) {
    return new Vehicle(name, type);
  }

}

let car = Vehicle.create('Tesla', 'car');
console.log(car.getName()); // Tesla
console.log(car.getType()); // car
```

Classes give us an opportunity to create static members. We don't have to use the **new** keyword later to instantiate a class.

static methods (properties) are also inherited
and could be called by super

## get / set

Other great things in upcoming ES6 are **getters** and **setters** for object properties. They allow us to run the code on the reading or writing of a property.

```javascript
class Car {

  constructor (name) {
    this._name = name;
  }

  set name (name) {
    this._name = name;
  }

  get name () {
    return this._name;
  }

}

let car = new Car('Tesla');
console.log(car.name); // Tesla

car.name = 'BMW';
console.log(car.name); // BMW
```

I use '_' prefix to create a (**tmp**) field to store name property.

## Enhanced Object Properties

The last thing I have to mention is **property shorthand**, **computed property names** and **method properties**.

ES6 gives us shorter syntax for common **object property** definition:

```javascript
// ES6
let x = 1,
    y = 2,
    obj = { x, y };

console.log(obj); // Object { x: 1, y: 2 }

// ES5
var x = 1,
    y = 2,
    obj = {
      x: x,
      y: y
    };

console.log(obj); // Object { x: 1, y: 2 }
```

As you can see, this works because the property value has the same name as the property identifier.

Another thing is ES6 support for **computed names** in object property definitions:

```javascript
// ES6
let getKey = () => '123',
    obj = {
      foo: 'bar',
      ['key_' + getKey()]: 123
    };

console.log(obj); // Object { foo: 'bar', key_123: 123 }

// ES5
var getKey = function () {
      return '123';
    },
    obj = {
      foo: 'bar'
    };
obj['key_' + getKey()] = 123;

console.log(obj); // Object { foo: 'bar', key_123: 123 }
```

The one last thing is **method properties** seen in classes above. We can even use it in object definitions:

```javascript
// ES6
let obj = {
  name: 'object name',
  toString () { // 'function' keyword is omitted here
    return this.name;
  }
};


console.log(obj.toString()); // object name

// ES5
var obj = {
  name: 'object name',
  toString: function () {
    return this.name;
  }
};

console.log(obj.toString()); // object name
```
