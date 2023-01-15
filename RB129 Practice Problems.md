
# 1.

```
class Person
  attr_reader :name
  
  def set_name
    @name = 'Bob'
  end
end

bob = Person.new
p bob.name
```

# What is output and why? What does this demonstrate about instance variables that differentiates them from local variables?

This code snippet will output `nil`. This is because instance variables need to be specifically initialized and assigned. 

However, this demonstrates a key difference between instance variables and local variables; that is despite not being initialized, the instance variable `@name` is accessible and will return `nil`, whereas a local variable that is not initialized will return an uninitialized local variable error!

# 2.

```
module Swimmable
  def enable_swimming
    @can_swim = true
  end
end

class Dog
  include Swimmable

  def swim
    "swimming!" if @can_swim
  end
end

teddy = Dog.new
p teddy.swim   
```

\# What is output and why? What does this demonstrate about instance variables?

The output of this code snippit is `nil`. This is because when the `swim` method is invoked on the calling object `Teddy`, it will only return the string “Swimming” if the instance variable `@can\_swim` is truthy. However, because the `enable\_swimming` method was not invoked, and therefore the `@can\_swim` instance variable was not initialized and assigned to the boolean value of `true.` Additionally, we know that instance methods that are not initialized have a value of `nil`. 

This demonstrates that instance variables require initialization in order to have value set other than `nil`. 

# 3. 

```
module Describable
  def describe_shape
    "I am a #{self.class} and have #{SIDES} sides."
  end
end

class Shape
  include Describable

  def self.sides
    self::SIDES
  end
  
  def sides
    self.class::SIDES
  end
end

class Quadrilateral < Shape
  SIDES = 4
end

class Square < Quadrilateral; end

p Square.sides 
p Square.new.sides 
p Square.new.describe_shape 
```

\# What is output and why? What does this demonstrate about constant scope? What does `self` refer to in each of the 3 methods above? 


# 4.

```
class AnimalClass
  attr_accessor :name, :animals
  
  def initialize(name)
    @name = name
    @animals = []
  end
  
  def <<(animal)
    animals << animal
  end
  
  def +(other_class)
    animals + other_class.animals
  end
end

class Animal
  attr_reader :name
  
  def initialize(name)
    @name = name
  end
end

mammals = AnimalClass.new('Mammals')
mammals << Animal.new('Human')
mammals << Animal.new('Dog')
mammals << Animal.new('Cat')

birds = AnimalClass.new('Birds')
birds << Animal.new('Eagle')
birds << Animal.new('Blue Jay')
birds << Animal.new('Penguin')

some_animal_classes = mammals + birds

p some_animal_classes 
```

\# What is output? Is this what we would expect when using `AnimalClass#+`? If not, how could we adjust the implementation of `AnimalClass#+` to be more in line with what we'd expect the method to return?


# 5.

```
class GoodDog
  attr_accessor :name, :height, :weight

  def initialize(n, h, w)
    @name = n
    @height = h
    @weight = w
  end

  def change_info(n, h, w)
    name = n
    height = h
    weight = w
  end

  def info
    "#{name} weighs #{weight} and is #{height} tall."
  end
end


sparky = GoodDog.new('Spartacus', '12 inches', '10 lbs') 
sparky.change_info('Spartacus', '24 inches', '45 lbs')
puts sparky.info 
# => Spartacus weighs 10 lbs and is 12 inches tall.
```

\# We expect the code above to output `”Spartacus weighs 45 lbs and is 24 inches tall.”` Why does our `change\_info` method not work as expected?

The `change\_info` method does not work as expected is due to the fact that on line 11, 12, and 13, the variables named `name`, `height`, and `weight` respectively, are in fact local variable initialization, and are not references to the instance variables `@name`, `@height`, and `@weight`. Additionally, they are also neither the respective setter methods as well. As a result, in order to correct this code so that it does work as intended, we must alter the local variable initialization to prepend with either an `@` symbol to reference the instance variables directly, or prepend `self.` in order to reference the setter methods for their respective instance variables! This is because we know that self inside of an instance method refers to the calling objects, and thus, permits us to call the appropriate setter method to the intended new value. 

# 6.

```
class Person
  attr_accessor :name

  def initialize(name)
    @name = name
  end
  
  def change_name
    name = name.upcase
  end
end

bob = Person.new('Bob')
p bob.name 
bob.change_name
p bob.name
```

\# In the code above, we hope to output `'BOB'` on `line 16`. Instead, we raise an error. Why? How could we adjust this code to output `'BOB'`? 

# 7.

```
class Vehicle
  @@wheels = 4

  def self.wheels
    @@wheels
  end
end

p Vehicle.wheels                             

class Motorcycle < Vehicle
  @@wheels = 2
end

p Motorcycle.wheels                           
p Vehicle.wheels                              

class Car < Vehicle; end

p Vehicle.wheels
p Motorcycle.wheels                           
p Car.wheels   
```

\# What does the code above output, and why? What does this demonstrate about class variables, and why we should avoid using class variables when working with inheritance?

# 8.

```
class Animal
  attr_accessor :name

  def initialize(name)
    @name = name
  end
end

class GoodDog < Animal
  def initialize(color)
    super
    @color = color
  end
end

bruno = GoodDog.new("brown")       
p bruno
```

\# What is output and why? What does this demonstrate about `super`?

The output of this code snippet is the object `bruno` with both instances variable `name` and `color` having the same value of the string object “brown”. This demonstrates that when you are using super, If it takes in a specific number of arguments and you do not specify the argument that is passed in, it will default to using the same argument that was passed in above, ie the string object “brown”. 
# 9.

```
class Animal
  def initialize
  end
end

class Bear < Animal
  def initialize(color)
    super
    @color = color
  end
end

bear = Bear.new("black")   
```

\# What is output and why? What does this demonstrate about `super`? 

This demonstrates that `super` will require specific number of arguments, depending on how it is used. Specifically, if the super that references the parent class does not take arguments, we must define super so that in the sub-class it does not take in arguments as well.
# 10.

```module Walkable
  def walk
    "I'm walking."
  end
end

module Swimmable
  def swim
    "I'm swimming."
  end
end

module Climbable
  def climb
    "I'm climbing."
  end
end

module Danceable
  def dance
    "I'm dancing."
  end
end

class Animal
  include Walkable

  def speak
    "I'm an animal, and I speak!"
  end
end

module GoodAnimals
  include Climbable

  class GoodDog < Animal
    include Swimmable
    include Danceable
  end
  
  class GoodCat < Animal; end
end

good_dog = GoodAnimals::GoodDog.new
p good_dog.walk
```

\# What is the method lookup path used when invoking `#walk` on `good\_dog`?

# 11.

```
class Animal
  def eat
    puts "I eat."
  end
end

class Fish < Animal
  def eat
    puts "I eat plankton."
  end
end

class Dog < Animal
  def eat
     puts "I eat kibble."
  end
end

def feed_animal(animal)
  animal.eat
end

array_of_animals = [Animal.new, Fish.new, Dog.new]
array_of_animals.each do |animal|
  feed_animal(animal)
end
```

\# What is output and why? How does this code demonstrate polymorphism? 

This output of this code snippet is, 3 string objects, “I eat”, “I eat plankton” and “I eat kibble.” This code demonstrates polymorphism, simply because there are 3 different data types that are able to invoked by the same method name, `eat`, which by definition, is polymorphism. 

# 12.

```
class Person
  attr_accessor :name, :pets

  def initialize(name)
    @name = name
    @pets = []
  end
end

class Pet
  def jump
    puts "I'm jumping!"
  end
end

class Cat < Pet; end

class Bulldog < Pet; end

bob = Person.new("Robert")

kitty = Cat.new
bud = Bulldog.new

bob.pets << kitty
bob.pets << bud                     

bob.pets.jump 
```

\# We raise an error in the code above. Why? What do `kitty` and `bud` represent in relation to our `Person` object?  

# 13.

```
class Animal
  def initialize(name)
    @name = name
  end
end

class Dog < Animal
  def initialize(name); end

  def dog_name
    "bark! bark! #{@name} bark! bark!"
  end
end

teddy = Dog.new("Teddy")
puts teddy.dog_name   
```

\# What is output and why?

The output of this code snippet is the string object, `”bark! Bark! Bark! Bark!”`. This is because when the method `dog\_name` is invoked on the calling object referenced by `teddy`, it is returning a string object with an instance variable `@name` interpolated inside. However, because the Dog class defines its own `initialize` method that overrides the method with the same name from the parent class `Animal`, the instance variable `@name` was never initialized, its value is `nil`by default, and therefore returns `nil` when interpolated inside of a string object!

# 14.

```
class Person
  attr_reader :name

  def initialize(name)
    @name = name
  end
end

al = Person.new('Alexander')
alex = Person.new('Alexander')
p al == alex # => true
```

\# In the code above, we want to compare whether the two objects have the same name. `Line 11` currently returns `false`. How could we return `true` on `line 11`? 

\# Further, since `al.name == alex.name` returns `true`, does this mean the `String` objects referenced by `al` and `alex`'s `@name` instance variables are the same object? How could we prove our case?


# 15.

```
class Person
  attr_reader :name

  def initialize(name)
    @name = name
  end

  def to_s
    "My name is #{name.upcase!}."
  end
end

bob = Person.new('Bob')
puts bob.name
puts bob
puts bob.name
```

\# What is output on `lines 14, 15, and 16` and why?

16.

\# Why is it generally safer to invoke a setter method (if available) vs. referencing the instance variable directly when trying to set an instance variable within the class? Give an example.

It is generally safer to invoke a setter method because if you were to reference the instance variable directly within the class, it would bypass any checks/operations performed by the setter. An example would be defining the setter method so that it ensures the new value set to the instance variable is a integer object first before assignment, by invoking the `to\_i` method on the `miles` argument passed into the method!

# 17.

\# Give an example of when it would make sense to manually write a custom getter method vs. using `attr\_reader`.

A custom getter method would make sense, when you want the return value of the instance variable to be customized to your intended output. An example of this would be if you wanted to prepend the string object “Sir” to a `name` instance variable, in order to return a concatenation of “Sir” and the instance variable `name`. 

# 18. 

```
class Shape
  @@sides = nil

  def self.sides
    @@sides
  end

  def sides
    @@sides
  end
end

class Triangle < Shape
  def initialize
    @@sides = 3
  end
end

class Quadrilateral < Shape
  def initialize
    @@sides = 4
  end
end
```

\# What can executing `Triangle.sides` return? What can executing `Triangle.new.sides` return? What does this demonstrate about class variables?

# 19.

\# What is the `attr\_accessor` method, and why wouldn’t we want to just add `attr\_accessor` methods for every instance variable in our class? Give an example.

# 20.
\# What is the difference between states and behaviors?

# 21. 
\# What is the difference between instance methods and class methods?

# 22.
\# What are collaborator objects, and what is the purpose of using them in OOP? Give an example of how we would work with one.

# 23.
\# How and why would we implement a fake operator in a custom class? Give an example.

# 24.
\# What are the use cases for `self` in Ruby, and how does `self` change based on the scope it is used in? Provide examples.

# 25.

```
class Person
  def initialize(n)
    @name = n
  end
  
  def get_name
    @name
  end
end

bob = Person.new('bob')
joe = Person.new('joe')

puts bob.inspect # => #<Person:0x000055e79be5dea8 @name="bob">
puts joe.inspect # => #<Person:0x000055e79be5de58 @name="joe">

p bob.get_name # => "bob"
```

` `# What does the above code demonstrate about how instance variables are scoped?

This code demonstrates that instance variables are scoped at the object level. Specifically, once objects from the Person class are instantiated on lines 12 and 13, the `@name` instance variable are initialized from their respective constructor, `initialize,` methods. However, despite being initialized inside of a different method, the instance variable is still accessible from the getter method `get\_name`. Therefore, this demonstrates that instances variables are scoped at the object level, and are accessible throughout the object’s instance methods, despite being initialized inside of an instance method! 

# 26.
\# How do class inheritance and mixing in modules affect instance variable scope? Give an example.

# 27.
\# How does encapsulation relate to the public interface of a class?

# 28.

```
class GoodDog
  DOG_YEARS = 7

  attr_accessor :name, :age

  def initialize(n, a)
    self.name = n
    self.age  = a * DOG_YEARS
  end
end

sparky = GoodDog.new("Sparky", 4)
puts sparky
```

\# What is output and why? How could we output a message of our choice instead?

The output for this code snippet is the name of the object class (`GoodDog` in this case) and the encoding object ID. This is because the `puts` method automatically calls the `to\_s` method on the argument. If you wanted to output a message of our choice instead, you can override this default `to\_s` method by defining your own `to\_s` method within the `GoodDog` class itself. 

\# How is the output above different than the output of the code below, and why?

The output of the code below is not different from the output above, however, the mechanism to set the value of the instance variables `@name` and `@age` are different. Specifically, in the example above it is invoking the setter methods of the instance variable within the `initialize` method to set their values to their respective parameters, `n`, and `a`, whereas in the example below it is directly referencing the instance variables themselves in order to set their values. 

```
class GoodDog
  DOG_YEARS = 7

  attr_accessor :name, :age

  def initialize(n, a)
    @name = n
    @age  = a * DOG_YEARS
  end
end

sparky = GoodDog.new("Sparky", 4)
p sparky
```

# 29.
\# When does accidental method overriding occur, and why? Give an example.

# 30.
\# How is Method Access Control implemented in Ruby? Provide examples of when we would use public, protected, and private access modifiers.

# 31.
\# Describe the distinction between modules and classes.

# 32.
\# What is polymorphism and how can we implement polymorphism in Ruby? Provide examples.

# 33.
\# What is encapsulation, and why is it important in Ruby? Give an example.

# 34.

```
module Walkable
  def walk
    "#{name} #{gait} forward"
  end
end

class Person
  include Walkable

  attr_reader :name

  def initialize(name)
    @name = name
  end

  private

  def gait
    "strolls"
  end
end

class Cat
  include Walkable

  attr_reader :name

  def initialize(name)
    @name = name
  end

  private

  def gait
    "saunters"
  end
end

mike = Person.new("Mike")
p mike.walk

kitty = Cat.new("Kitty")
p kitty.walk
```

\# What is returned/output in the code? Why did it make more sense to use a module as a mixin vs. defining a parent class and using class inheritance?

# 35.
\# What is Object Oriented Programming, and why was it created? What are the benefits of OOP, and examples of problems it solves?

Object oriented programming is a programming paradigm that uses classes and objects to take advantage of inheritance, encapsulation, and polymorphism in order to divide a program into different parts that interact with one another, making our code more flexible and reusable.

Starting with classes and objects, classes are like blueprints that objects are based on. We define state and behavior within our classes and then instantiate objects from those classes.

# 36.
\# What is the relationship between classes and objects in Ruby?

# 37.
\# When should we use class inheritance vs. interface inheritance?

Subclass has a ‘is-a’ relationship, versus having a ‘has-a’ relationship

# 38.

```
class Cat
end

whiskers = Cat.new
ginger = Cat.new
paws = Cat.new
```

\# If we use `==` to compare the individual `Cat` objects in the code above, will the return value be `true`? Why or why not? What does this demonstrate about classes and objects in Ruby, as well as the `==` method?

In the code snippet, the return value will not be true. This is because when you are defining a Class without defining `==` specifically, it will default to utilizing the `==` method from the BasicObject class. By default, this `==` method will compare whether or not two objects are the same object, and not whether the VALUES of the objects are the same. What this demonstrates about classes and objects in Ruby is that even if objects are instantiated from the same class, they are not the same object. Additionally, this also demonstrates that the default `==` method from the `BasicObject` class will automatically compare if two objects are the same object. 

# 39.

```
class Thing
end

class AnotherThing < Thing
end

class SomethingElse < AnotherThing
end
```

\# Describe the inheritance structure in the code above, and identify all the superclasses.

# 40.

```
module Flight
  def fly; end
end

module Aquatic
  def swim; end
end

module Migratory
  def migrate; end
end

class Animal
end

class Bird < Animal
end

class Penguin < Bird
  include Aquatic
  include Migratory
end

pingu = Penguin.new
pingu.fly
```

What is the method lookup path that Ruby will use as a result of the call to the `fly` method? Explain how we can verify this.

# 41.

```
class Animal
  def initialize(name)
    @name = name
  end

  def speak
    puts sound
  end

  def sound
    "#{@name} says "
  end
end

class Cow < Animal
  def sound
    super + "moooooooooooo!"
  end
end

daisy = Cow.new("Daisy")
daisy.speak
```

\# What does this code output and why?

This code snippet will output, “Daisy says moooooooooo!” This is because after a cow object has been instantiated from the Cow Class on line 21 and passes in “Daisy” string object as an argument, is it referenced by the local variable initialization of `daisy`. Then on line 22, the `speak` method is invoked on the calling object, `daisy`. To reconcile this search, first it will look for the `speak` method inside the cow class definition which is not found, and then search up the inheritance chain to the `Animal` class where it finds the `speak` method definition. The `puts` method is then invoked, and passing in the return value of the `sound` method as an argument. The `sound` method is then searched for and found within the cow class definition, then returns a string object, with the instance variable `@name` interpolated into a string, plus another string object “moooooo!”. Therefore this returns the output as described above. 

# 42.

```
class Cat
  def initialize(name, coloring)
    @name = name
    @coloring = coloring
  end

  def purr; end

  def jump; end

  def sleep; end

  def eat; end
end

max = Cat.new("Max", "tabby")
molly = Cat.new("Molly", "gray")
```

\# Do `molly` and `max` have the same states and behaviors in the code above? Explain why or why not, and what this demonstrates about objects in Ruby.

`Molly` and `max` have the same behaviors (also known as instance methods) in the code snippet above, but do not share the same states, also known as instance variables. Specifically, `max` will be referencing a cat object with instance variables `@name` and `@color` referencing the string objects “max” and “tabby” respectively. In comparison, `molly` the cat object’s instance variables will be referencing ‘molly’ and ‘gray’ respectively. This is because OOP in Ruby, objects of the same class will share behaviors, but will not share states. This demonstrates that objects in ruby will inherit behaviors, but will not inherit instance variables and their respective states!

# 43.

```
class Student
  attr_accessor :name, :grade

  def initialize(name)
    @name = name
    @grade = nil
  end
  
  def change_grade(new_grade)
    grade = new_grade
  end
end

priya = Student.new("Priya")
priya.change_grade('A')
priya.grade 
```

\# In the above code snippet, we want to return `”A”`. What is actually returned and why? How could we adjust the code to produce the desired result?

This code snippet will return `nil`. This is because on line 10, rather than referencing the setter method for the instance variable `@grade`, it is a local variable initialization of the variable `grade` that is set to the argument (string object `‘A’`) that is passed into the method `change\_grade` on line 15. As a result, the instance variable `@grade` was never set to the string object `’A’` and is why it returns `nil` instead. Thus, we must prepend `self` to `grade` on line 10 in order to properly reference the setter method for `@grade` and reassign the instance variable to the value that we intended. 

# 44.

```
class MeMyselfAndI
  self

  def self.me
    self
  end

  def myself
    self
  end
end

i = MeMyselfAndI.new
```

\# What does each `self` refer to in the above code snippet?

`self` on line 9, references the calling object itself, that is stored as a value for the local variable `i`, because it is inside of an instance method definition. In comparison, the other `self` from line 2, line 4 and line 5, all refer to the class `MeMyselfAndI` itself!

# 45.

```
class Student
  attr_accessor :grade

  def initialize(name, grade=nil)
    @name = name
  end 
end

ade = Student.new('Adewale')
p ade # => #<Student:0x00000002a88ef8 @grade=nil, @name="Adewale">
```

\# Running the following code will not produce the output shown on the last line. Why not? What would we need to change, and what does this demonstrate about instance variables?

The reason why running the code snippet above as such will not produce the output shown on the last line, is because in order to add the instance variable `@grade` as part of the initialized object’s state, it must be specifically initialized. Despite having established the instance variable and its respective getter/setter methods from the `attr\_accessor` from line 2, we must change the `initialize` method to include setting `@grade` to reference the value that argument that parameter `grade` passed in on line 4. This demonstrates that instance variables must be specifically initialized, in order to considered part of the object’s state! 
# 46.

```
class Character
  attr_accessor :name

  def initialize(name)
    @name = name
  end

  def speak
    "#{@name} is speaking."
  end
end

class Knight < Character
  def name
    "Sir " + super
  end
end

sir_gallant = Knight.new("Gallant")
p sir_gallant.name 
p sir_gallant.speak 
```

\# What is output and returned, and why? What would we need to change so that the last line outputs `”Sir Gallant is speaking.”`? 

The output of this code snippet from the last 2 lines is “Sir Gallant” and “Gallant is speaking.” Respectively. In order to change this final output to reflect “Sir Gallant is speaking.” We must change the method definition of `speak.` Specifically, inside the body of the method definition, it is performing string interpolation of the instance variable `@name` directly. Therefore, if we change it to the getter method `name` instead, it will reference the custom name getter, and prepend ‘Sir ‘ to the instance variable `@name` to be interpolated inside of the string

# 47. 

```
class FarmAnimal
  def speak
    "#{self} says "
  end
end

class Sheep < FarmAnimal
  def speak
    super + "baa!"
  end
end

class Lamb < Sheep
  def speak
    super + "baaaaaaa!"
  end
end

class Cow < FarmAnimal
  def speak
    super + "mooooooo!"
  end
end

p Sheep.new.speak
p Lamb.new.speak
p Cow.new.speak 
```

\# What is output and why? 

The output for the last three lines will be the class object itself, interpolated into the string object. This is because `self` inside of an instance method on line 3 refers to the calling object. In order to correct the output, we must call the `class` method on `self` in order to return the class of the calling object. In this scenario, sheep, lamb, and cow, respectively. 

# 48.

```
class Person
  def initialize(name)
    @name = name
  end
end

class Cat
  def initialize(name, owner)
    @name = name
    @owner = owner
  end
end

sara = Person.new("Sara")
fluffy = Cat.new("Fluffy", sara)
```

\# What are the collaborator objects in the above code snippet, and what makes them collaborator objects?

The `Person` object referenced by the variable `Sara` is stored as a collaborator object for the `Fluffly` cat object’s instance variable, `@owner`. 

# 49.

```
number = 42

case number
  when 1          then 'first'
  when 10, 20, 30 then 'second'
  when 40..49     then 'third'
end
```

\# What methods does this `case` statement use to determine which `when` clause is executed?

The `case` method uses `===` instance method implicitly to determine which `when` clause is executed. For each `when` clause, it is asking if the VALUE that is being presented, does it belong to a specific group? Therefore, specifically it uses #Range===  when comparing a range to an integer such as on line 6 and #Integer=== when comparing 2 instances of integers. 

# 50.

```
class Person
  TITLES = ['Mr', 'Mrs', 'Ms', 'Dr']

  @@total_people = 0

  def initialize(name)
    @name = name
  end

  def age
    @age
  end
end
```

\# What are the scopes of each of the different variables in the above code?

`@@total\_people’ is a class variable, who’s scope is limited to the CLASS!

Whereas both `@name` and `@age` are instance variables whose scope is limited to the OBJECT. 

Titles is a CONSTANT and has lexical scope.

# 51.
\# The following is a short description of an application that lets a customer place an order for a meal:

\# - A meal always has three meal items: a burger, a side, and drink.

\# - For each meal item, the customer must choose an option.

\# - The application must compute the total cost of the order.

\# 1. Identify the nouns and verbs we need in order to model our classes and methods.

\# 2. Create an outline in code (a spike) of the structure of this application.

\# 3. Place methods in the appropriate classes to correspond with various verbs.

# 52. 

```
class Cat
  attr_accessor :type, :age

  def initialize(type)
    @type = type
    @age  = 0
  end

  def make_one_year_older
    self.age += 1
  end
end
```

\# In the `make\_one\_year\_older` method we have used `self`. What is another way we could write this method so we don't have to use the `self` prefix? Which use case would be preferred according to best practices in Ruby, and why?

Another way to write this method in order to not use the `self` prefix would be replace the `self.` with `@` in order to reference the instance variable `@age` directly. However, in these scenarios, it is preferred to use the getter metter with `self` prepended, simply because it would bypass any checks/operations performed by the setter, and in addition, allows for easier code maintenance in the future due to it being isolated to one area, rather than all over the program

# 53.

```
module Drivable
  def self.drive
  end
end

class Car
  include Drivable
end

bobs_car = Car.new
bobs_car.drive
```

\# What is output and why? What does this demonstrate about how methods need to be defined in modules, and why?

The output of this code snippet is a no method error. This demonstrates that method definition within modules must not be prepended with `self`, if the intention is for it to be accessible by the calling object;s class included the original module in the first place. `Self` prepended to a instance method definition inside of a module refers to the Module itself, and therefore in order to invoke that method, would require `ModuleName::Methodname`, or in this scenario, `Driveable::drive’. 

# 54.

```
class House
  attr_reader :price

  def initialize(price)
    @price = price
  end
end

home1 = House.new(100_000)
home2 = House.new(150_000)
puts "Home 1 is cheaper" if home1 < home2 # => Home 1 is cheaper
puts "Home 2 is more expensive" if home2 > home1 # => Home 2 is more expensive
```

\# What module/method could we add to the above code snippet to output the desired output on the last 2 lines, and why?

We could add two instance method definitions, one that permits the greater comparison of the instance variable `@price` with another object, and another that permits the lesser comparison with another object respectively. This is because the class `House` definition does not have defined methods for `<` and `>` respectively, and need to be explicitly defined within the class definition itself.  
#



