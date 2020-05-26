# OOP Basics \#4

## OOP \#4: Inheritance

The idea of inheritance is allows you to create a new [class ](https://zentut.com//php-tutorial/php-objects-and-classes/)that reuses the properties and methods from an existing class. 

A class that inherits from another class is called _subclass_ \(also a _child class_ or a _derived class\)._ 

The class from which the _subclass_ inherits is known as _parent_ class \(also a superclass or a _based class_\). 

Besides inheriting properties and methods from the _parent_ class, a _subclass_ class can have additional properties and methods

> In inheritance, we have a parent class with its own methods and properties, and a child class \(or classes\) that can use properties and methods from the parent.

> The inheritance is a way to form new classes using classes that have already been defined. The newly formed classes are called _derived_ classes, the classes that we derive from are called _base_ classes

```php
//The parent class
class Car {
  // Private property inside the class
  private $model;
 
  //Public setter method
  public function setModel($model)
  {
    $this->model = $model;
  }
 
  public function hello()
  {
    return "beep! I am a <i>" . $this->model . "</i><br />";
  }
}
 
 
//The child class inherits the code from the parent class
class SportsCar extends Car {
  //No code in the child class
}
 
 
//Create an instance from the child class
$sportsCar1 = new SportsCar();
  
// Set the value of the class’ property.
// For this aim, we use a method that we created in the parent
$sportsCar1->setModel('Mercedes Benz');
  
//Use another method that the child class inherited from the parent class
echo $sportsCar1->hello();
```

> However, while a child class can use the code it inherited from the parent, the parent class is not allowed to use the child class’s code

### Override the parent’s properties and methods in the child class

The inherited methods can be overridden by **redefining the method with the same name**.

```php
// The parent class has hello method that returns "beep".
class Car {
  public function hello()
  {
    return "beep";
  }
}
 
//The child class has hello method that returns "Halllo"
class SportsCar extends Car {
  public function hello()
  {
    return "Hallo";
  }
}
    
//Create a new object
$sportsCar1 = new SportsCar();
  
//Get the result of the hello method
echo $sportsCar1->hello();
```

### Calling the Parent's Methods <a id="overriding-parent-call"></a>

From the child class, we can call parent's methods using the parent keyword and :: \(Scope Resolution Operator\).

```php
class Person {
	public $name;
	public $age;
	public function __construct($name, $age) {
		$this->name = $name;
		$this->age = $age;
	}
	public function introduce() {
		echo "My name is {$this -> name}. My age is {$this -> age}";
	}
}
class Tom extends Person {
	public $school;
	public function __construct($name, $age, $school) {
		# $this -> name and $this -> age will be set by the parent's constructor
		parent::__construct($name, $age);
		$this->school = $school;
	}
	public function introduce() {
		echo "My name is {$this -> name}. My age is {$this -> age}. My school is {$this -> school}";
	}
}
$tom = new Tom('Tom', 29, 'Foothill School');
$tom->introduce();
```

### Prevent the child class from overriding the parent’s methods

In order to prevent the method in the child class from overriding the parent’s methods, we can prefix the method in the parent with the **final** keyword.

There are two uses of the `final` keyword.

#### 1. Preventing Class Inheritance

```php
final class NonParent {
	// class code
}

// will throw an error
class Child extends NonParent {...}

```

#### 2. Preventing Method Overriding

```php
class ParentClass {
	final public function myMethod() {

	}
}

class Child extends ParentClass {
	// Fatal Error: Overriding final methods
	public function myMethod() {

	}
}
```

