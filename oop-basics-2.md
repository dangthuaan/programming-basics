# OOP Basics \#2

## OOP \#2: Encapsulation

#### 1.1. Encapsulation

> It states the concept of binding data \(attributes\) and methods \(functions\) into a single unit \(Class\). This term is also referred to as Information Hiding.

> With Encapsulation you have a control to access and set class parameters and methods. You have a control to say which part is visible to outsiders and how one can set your objects parameters.

**So finally the concept of Encapsulation in PHP is hiding internal information of object to protect from the other object**

Depending on the methods that we implement, we can decide what permissions have to be given. We can give read-only, write-only, full permissions or no permissions at all. To achieve this we [use access specifiers](https://www.educba.com/access-specifiers-in-c-plus-plus/) such as public, private and protected.

| **Access Specifiers** | **Private** | **Protected** | **Public** |
| :--- | :--- | :--- | :--- |
| Access in same class | **√** | **√** | **√** |
| Access in extended class | × | **√** | **√** |
| Access outside the class | × | × | **√** |

Using getter and setter methods, you can access the variables and methods outside the class as well.

> `get` method for get the class parameters form outside, `set` method use for validate and set valid value to protected variable.



#### 1.2. The meaning of **Encapsulation**, is to make sure that "sensitive" data is hidden from users. To achieve this, you must:

* declare class variables/attributes as `private`
* provide public **get** and **set** methods to access and update the value of a `private` variable

The `get` method returns the variable value, and the `set` method sets the value.

```php
class Car {
 
  //the private access modifier denies access to the method from outside the class’s scope
  private $model;
 
  //the public access modifier allows the access to the method from outside the class
  public function setModel($model)
  {
    $this->model = $model;
  }
  
  public function getModel()
  {
    return "The car model is " . $this->model;
  }
}
 
$mercedes = new Car();
//Sets the car’s model
$mercedes->setModel("Mercedes benz");
//Gets the car’s model
echo $mercedes->getModel();
```



#### 1.3. Why we need Access Modifiers

We need **access modifiers** in order to limit the modifications that code from outside the classes can do to the classes' methods and properties. Once we define a property or method as private, only methods that are within the class are allowed to approach it. So, in order to interact with private methods and properties, we need to provide public methods. Inside these methods, we can put logic that can validate and restrict data that comes from outside the class.



#### 1.4. Examples and notes

```php
class User
{
    private $gender;

    public function getGender()
    {
        return $this->gender;
    }

    public function setGender($gender)
    {
        if ('male' !== $gender and 'female' !== $gender) {
            throw new \Exception('Set male or female for gender');
        }

        $this->gender = $gender;
    }
}
```

Now you can create an object from your User class and you can safely set gender parameters. If you set anything that is wrong for your class then it will throw and exception. You may think it is unnecessary but when your code grows you would love to see a meaningful exception message rather than awkward logical issue in the system with no exception.

```php
$user = new User();
$user->setGender('male');

// An exception will throw and you can not set 'Y' to user gender
$user->setGender('Y');
```

\(Bad Way\)

If you do not follow Encapsulation roles then your code would be something like this. Very hard to maintain. Notice that we can set anything to the user gender property.

```php
<?php

class User
{
    public $gender;
}

$user = new User();
$user->gender = 'male';

// No exception will throw and you can set 'Y' to user gender however 
// eventually you will face some logical issue in your system that is 
// very hard to detect
$user->gender = 'Y';
```

**Access class methods**

\(Good Way\)

```php
<?php

class User
{
    public function doSomethingComplex()
    {
        $this->doThis(...);
        ...
        $this->doThat(...);
        ...
        $this->doThisExtra(...);
    }

    private function doThis(...some Parameters...)
    {
      ...
    }

    private function doThat(...some Parameters...)
    {
      ...
    }

    private function doThisExtra(...some Parameters...)
    {
      ...
    }
}
```

We all know that we should not make a function with 200 line of code instead we should break it to some individual function that breaks the code and improve the readability of code. Now with encapsulation you can get these functions to be private it means it is not accessible by outsiders and later when you want to modify a function you would be sooo happy when you see the private keyword.

\(Bad Way\)

```php
class User
{
    public function doSomethingComplex()
    {
        // do everything here
        ...
        ...
        ...
        ...
    }
}
```

