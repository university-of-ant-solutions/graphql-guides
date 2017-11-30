---
description: What is interface and how to design it ?
keywords: graphql, interface, design
title: What is interface and how to design it ?
---

## Interface

Like many type systems, GraphQL supports interfaces. An Interface is an abstract type that includes a certain set of fields that a type must include to implement the interface. - From [graphql.org](http://graphql.org/learn/schema/#interfaces)

### Interface in OOP

[http://www.cs.utah.edu/~germain/PPS/Topics/interfaces.html](http://www.cs.utah.edu/~germain/PPS/Topics/interfaces.html)


An interface is a programming structure/syntax that allows the computer to enforce certain properties on an object (class).

#### What is the power of the interface?

The power is that once we have a number of classes which implement the interface, from some point of view, they are `equivalent`.

Without Interface:

```
// Main Actions
var car : Car = new Car();
var truck : Truck = new Truck();
car.start_engine();
truck.start_engine();
car.drive();
truck.drive();
```

With Interface:

```
// Main Actions
var vehicle1 : Vehicle = new Car(); // here the only difference is the TYPE of each variable is the same!!!!
var vehicle2 : Vehicle = new Truck();
vehicle1.start_engine();
vehicle2.start_engine();
vehicle1.drive();
vehicle2.drive();
```

#### Using an array and polymorphism

```
// Main Actions
var vehicles : Array = new Array();
vehicles.push( new Car() );
// Assume our program is modeling a game or traffic simulation
vehicles.push( new Car() );
// The Cars and Trucks that we are modeling can be created in
vehicles.push( new Truck() );
// an arbitrary matter.  Normally we would have to create separate
vehicles.push( new Car() );
// arrays for Cars, Trucks, etc.  But because the are all vehicles
vehicles.push( new Truck() );
// we can put them in a single array of Vehicle.
vehicles.push( new Truck() );
vehicles.push( new Truck() );
vehicles.push( new Car() );
vehicles.push( new Truck() );
vehicles.push( new Car() );
vehicles.push( new Bicycle() );
// assume Bicycle is also a Vehicle...
vehicles.push( new Car() );
// Later on in the program, we want to start all the cars...
// We can use GENERIC programming (we don't care if they are cars, trucks, bicycle, etc, just that they are vehicles).
for each (var item : Vehicle in vehicles )
{
		item.start_engine();
		item.drive();
}
```

The final example above shows the concept of Polymorphism. Polymorphism is the idea that an compile time (coding time) we don't know (and often can't know) what the actual type of object inside a variable will be. In the vehicles array above, we don't know if vehicles[i] is a car, truck, bicycle, etc.In computer languages without polymorphism, we wouldn't be able to do anything with these objects.

With polymorphism, the computer remembers what each is and when we say: "item.start_engine();" the computer decides, if this item is a truck then call "truck.start_engine()", if this item is a car, call "car.start_engine()", if this object is an XYZZY, call "XYZZY.start_engine();"

Polymorphism save the programmer a lot of time and effort in coding up "exceptional" conditions. The computer does the work for us, a) remembering what each object really is, and then b) at run time, invoking the actual function associated with the current object.

### Reference

- http://graphql.org/learn/schema/#interfaces

- http://www.cs.utah.edu/~germain/PPS/Topics/interfaces.html
