# Classes and Inheritance
Say we wanted to program a class labelled `Professor`. In pseudocode, a `Professor` class could be written like this:
```
class Professor
    properties
        name
        teaches
    methods
        grade(paper)
        introduceSelf()
```

This defines a `Professor` class with:
- two data properties: `name` and `teaches`
- two methods: `grade()` to grade a paper and `introduceSelf()` to introduce themselves.

On its own, a class doesn't do anything: it's a kind of template for creating concrete objects of that type.
* Each concrete professor we create is called an **instance** of the `Professor` class.
* The process of creating an instance is performed by a special function called a **constructor**.

Generally, the constructor is written out as part of the class definition, and it usually has the same name as the class itself:
```
class Professor
    properties
        name
        teaches
    constructor
        Professor(name, teaches)
    methods
        grade(paper)
        introduceSelf()
```

This constructor takes two parameters, so we can initialize the `name` and `teaches` properties when we create a new concrete professor:
```javascript
walsh = new Professor("Walsh", "Psychology");
lillian = new Professor("Lillian", "Poetry");

lillian.teaches; // 'Poetry'
walsh.introduceSelf(); // 'My name is Professor Walsh and I will be your Psychology professor.'
```

This creates two objects, both instances of the `Professor` class.

# Inheritance
Say we also want to represent students. They can't grade papers or teach a subject, but they can introduce themselves and have a name. Unlike professors, they belong to a particular year. We might write the definition of the class as follows:
```
class Student
    properties
        name
        year
    constructor
        Student(name, year)
    methods
        introduceSelf()
```

We can represent the fact that on some level, students and professors are the same kind of thing using inheritance.

We will create a person class and derive the student and professor classes from this:
```
class Person
    properties
        name
    constructor
        Person(name)
    methods
        introduceSelf()

class Professor : extends Person
    properties
        teaches
    constructor
        Professor(name, teaches)
    methods
        grade(paper)
        introduceSelf()

class Student : extends Person
    properties
        year
    constructor
        Student(name, year)
    methods
        introduceSelf()
```

In this case, we would say that:
* `Person` is the **superclass** or **parent class** of both `Professor` and `Student`.
* `Professor` and `Student` are **subclasses** or **child classes** of `Person`.

You might notice that `introduceSelf()` is defined in all three classes. The reason for this is that while all people want to introduce themselves, the way they do so is different:
```javascript
walsh = new Professor("Walsh", "Psychology");
walsh.introduceSelf(); // 'My name is Professor Walsh and I will be your Psychology professor.'

summers = new Student("Summers", 1);
summers.introduceSelf(); // 'My name is Summers and I'm in the first year.'

pratt = new Person("Pratt");
pratt.introduceSelf(); // 'My name is Pratt.'
```

* When a method has the same name but a different implementation in different classes it is called **polymorphism**.
* When a method in a subclass replaces the superclass's implementation, we say that the subclass **overrides** the version in the superclass.

# Encapsulation
An object's internal state is kept **private**, meaning that it can only be accessed by the object's own methods, not from other objects. Keeping an object's internal state private, and generally making a clear division between its public interface and its private internal state, is called **encapsulation**.

For example, suppose students are allowed to study archery if they are in the second year or above. We could implement this just by exposing the student's `year` property, and other code could examine that to decide whether the student can take the course:
```javascript
if (student.year > 1) {
  // allow the student into the class
}
```

The problem is if we decide to change the criteria for the class then we we would have to change every place in our system that uses this code. It would be better to have a `canStudyArchery()` method on `Student` objects, that implements the logic in one place:
```
class Student : extends Person
    properties
       year
    constructor
       Student(name, year)
    methods
       introduceSelf()
       canStudyArchery() { return this.year > 1 }
```

That way, if we want to change the rules about studying archery, we only have to update the `Student` class, and all the code using it will still work.

In many OOP languages, we can prevent other code from accessing an object's internal state by marking some properties as `private`. This will generate an error if code outside the object tries to access them:
```
class Student : extends Person
    properties
       private year
    constructor
        Student(name, year)
    methods
       introduceSelf()
       canStudyArchery() { return this.year > 1 }

student = new Student('Weber', 1)
student.year // error: 'year' is a private property of Student
```

# OOP and JavaScript
