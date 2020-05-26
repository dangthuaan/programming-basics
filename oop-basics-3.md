# OOP Basics \#3

## OOP \#3: Magic methods and constants

Object Oriented PHP offers several **magic methods and constants** that enable a lot of programming with very little code, but which suffer from a few disadvantages like a slight reduction in the performance time as well as in the level of code clarity.

> The names of magic methods always start with two underscores.

**Magic methos** trong php là những phương thức rất đặc biệc trong PHP vì nhiệm vụ của nó là bắt một sự kiện \(event\) nào đó khi chúng ta thao tác tới đối tượng

### 1. \_\_construct\(\)

The PHP constructor is the first method that is automatically called after the object is created. Each class has a constructor. If you do not explicitly declare it, then there will be a default constructor with no parameters and empty content in the class.

1\) the use of the method

Constructors are usually used to perform some initialization tasks, such as setting initial values ​​for member variables when creating objects.

2\) the declaration format of the constructor in a class

```php
function __constrct([parameter list]){

    Method body //It's usually used to set initial values ​​for member variables

}
```

> Note: Only one constructor can be declared in the same class, because PHP does not support constructor overloading.

Here is a complete example:

```php
<?php
    class Person
    {                                                                     
            public $name;       
            public $age;       
            public $sex;       

        /**
         * explicitly declare a constructor with parameters
         */                                                                                       
        public function __construct($name="", $sex="Male", $age=22)
        {     
            $this->name = $name;
            $this->sex = $sex;
            $this->age = $age;
        }

        /**
         * say method
         */
        public function say()
        {
            echo "Name：" . $this->name . ",Sex：" . $this->sex . ",Age：" . $this->age;
        }   

    }
```

Create object $Person1 without any parameters.

```php
$Person1 = new Person();
echo $Person1->say(); //print:Name：,Sex：Male,Age：22
```

Create object $Person2 with parameter "Jams".

```php
$Person2 = new Person("Jams");
echo $Person2->say(); // print: Name: Jams, Sex: Male, Age: 22
```

Create object $Person3 with three parameters.

```php
$Person3 = new Person ("Jack", "Male", 25);
echo $Person3->say(); // print: Name: Jack, Sex: Male, Age: 25
```

### 2. \_\_destruct\(\)

Now that we have already known what the constructor is, the destructor is the opposite.

Destructor allows you to perform some operations before destroying an object, such as closing a file, emptying a result set, and so on.

Destructor is a new feature introduced by PHP5.

The declaration format of the destructor is similar to that of the constructor  \_\_construct\(\), which means that \_\_destruct\(\) is also started with two underscores, and the name of the destructor is also fixed.

1\) the declaration format of destructor

```php
function __destruct()
{
    //method body
}
```

> Note: Destructor cannot have any parameters.

2\) the use of the destructor

In general, the destructor is not very common in PHP. It's an optional part of a class, usually used to complete some cleanup tasks before the object is destroyed.

Here is an example of using the destructor:

```php
<?php
class Person{     

    public $name;         
    public $age;         
    public $sex;         

    public function __construct($name="", $sex="Male", $age=22)
    {   
        $this->name = $name;
        $this->sex  = $sex;
        $this->age  = $age;
    }

    /**
     * say method
     */
    public function say()
    {
        echo "Name：".$this->name.",Sex：".$this->sex.",Age：".$this->age;
    }   

    /**
     * declare a destructor method
     */
    public function __destruct()
    {
            echo "Well, my name is ".$this->name;
    }
}

$Person = new Person("John");
unset($Person); //destroy the object of $Person created above
```

The output of the program above is:

```php
Well, my name is John
```

### 3. \_\_get\(\)

When you try to access a private property of an external object in a program, the program will throw an exception and end execution. We can use the magic method \_\_get\(\) to solve this problem. It can get the value of the private property of the object outside the object. Here is an example:

```php
<?php
class Person
{
    private $name;
    private $age;

    function __construct($name="", $age=1)
    {
        $this->name = $name;
        $this->age = $age;
    }

    public function __get($propertyName)
    {   
        if ($propertyName == "age") {
            if ($this->age > 30) {
                return $this->age - 10;
            } else {
                return $this->$propertyName;
            }
        } else {
            return $this->$propertyName;
        }
    }
}

$Person = new Person("John", 60);   // Instantiate the object with the Person class and assign initial values to the properties with the constructor.
echo "Name：" . $Person->name . "<br>";   // When the private property is accessed, the __get() method will be called automatically,so we can get the property value indirectly.
echo "Age：" . $Person->age . "<br>";    // The __get() method is called automatically，and it returns different values according to the object itself.
```

The results are as follows:

```text
Name: John
Age: 50
```

### 4. \_\_set\(\)

The \_\_set\( $property, $value\) method is used to set the private property of the object. When an undefined property is assigned, the \_\_set\(\) method will be triggered and the passed parameters are the property name and value that are set.

Here is the demo code:

```php
<?php
class Person
{
    private $name;
    private $age;

    public function __construct($name="",  $age=25)
    {
        $this->name = $name;
        $this->age  = $age;
    }

    public function __set($property, $value) {
        if ($property=="age")
        {
            if ($value > 150 || $value < 0) {
                return;
            }
        }
        $this->$property = $value;
    }

    public function say(){
        echo "My name is ".$this->name.",I'm ".$this->age." years old";
    }
}

$Person=new Person("John", 25); //Note that the initial value will be changed by the code below.
$Person->name = "Lili";     //The "name" property is assigned successfully. If there is no __set() method, then the program will throw an exception.
$Person->age = 16; //The "age" property is assigned successfully.
$Person->age = 160; //160 is an invalid value, so it fails to be assigned.
$Person->say();  //print：My name is Lili, I'm 16 years old.
```

Here is the result:

```text
My name is Lili, I'm 16 years old
```

### 7. \_\_isset\(\)

Before using the \_\_isset \(\) method, let me explain the use of the isset \(\) method first. The isset \(\) method is mainly used to determine whether the variable is set.

If you use the isset\(\) method outside the object, there are two cases:

1. If the parameter is a public property, you can use the isset\(\) method to determine whether the property is set or not.
2. If the parameter is a private property , the isset\(\) method will not work.

So for the private property, is there any way to know if it is set? Of course, as long as we define a \_\_isset\(\) method in a class, we can use the isset\(\) method outside the class to determine whether the private property is set or not.

When the isset\(\) or empty\(\) is called on an undefined or inaccessible property, the \_\_isset\(\) method will be called. Here is an example:

```php
<?php
class Person
{
    public $sex;
    private $name;
    private $age;

    public function __construct($name="",  $age=25, $sex='Male')
    {
        $this->name = $name;
        $this->age  = $age;
        $this->sex  = $sex;
    }

    /**
     * @param $content
     *
     * @return bool
     */
    public function __isset($content) {
        echo "The {$content} property is private，the __isset() method is called automatically.<br>";
        echo  isset($this->$content);
    }
}

$person = new Person("John", 25); // Initially assigned.
echo isset($person->sex),"<br>";
echo isset($person->name),"<br>";
echo isset($person->age),"<br>";
```

The results are as follows:

```text
1
The name property is private，the __isset() method is called automatically.
1
The age property is private，the __isset() method is called automatically.
1
```

### 8. \_\_unset\(\)

Similar to the \_\_isset\(\) method, the \_\_unset\(\) method is called when the unset\(\) method is called on an undefined or inaccessible property. Here is an example:

```php
<?php
class Person
{
    public $sex;
    private $name;
    private $age;

    public function __construct($name="",  $age=25, $sex='Male')
    {
        $this->name = $name;
        $this->age  = $age;
        $this->sex  = $sex;
    }

    /**
     * @param $content
     *
     * @return bool
     */
    public function __unset($content) {
        echo "It is called automatically when we use the unset() method outside the class.<br>";
        echo  isset($this->$content);
    }
}

$person = new Person("John", 25); // Initially assigned.
unset($person->sex),"<br>";
unset($person->name),"<br>";
unset($person->age),"<br>";
```

The results are as follows:

```text
It is called automatically when we use the unset() method outside the class.
1
It is called automatically when we use the unset() method outside the class.
1
```

####  

