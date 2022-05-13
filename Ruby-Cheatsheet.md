[back to overview](/../..)  
Looking for [Rails](../master/Ruby-on-Rails-Cheatsheet.md)?

# Ruby Cheatsheet<!-- omit in toc -->

- [Basics](#basics)
- [Vars, Contants, Arrays, Hashes & Symbols](#vars-contants-arrays-hashes--symbols)
  - [Constants](#constants)
  - [Arrays](#arrays)
  - [Hashes](#hashes)
  - [Symbols](#symbols)
    - [Functions to create Arrays](#functions-to-create-arrays)
- [Methods](#methods)
- [Classes](#classes)
- [Singleton Methods](#singleton-methods)
  - [Inheritance](#inheritance)
- [Modules](#modules)
- [Blocks & Procs](#blocks--procs)
  - [Code Blocks](#code-blocks)
  - [Proc](#proc)
- [Lambdas](#lambdas)
- [Calculation](#calculation)
- [Commenting](#commenting)
- [Conditions](#conditions)
  - [If](#if)
  - [Unless](#unless)
  - [Case](#case)
- [Printing & Putting](#printing--putting)
- [String Methods](#string-methods)
- [User Input](#user-input)
- [Loops](#loops)
  - [While loop](#while-loop)
  - [Until loop](#until-loop)
  - [For loop](#for-loop)
  - [Loop iterator](#loop-iterator)
  - [Next](#next)
  - [.each](#each)
  - [.times](#times)
  - [.upto / .downto](#upto--downto)
- [Sorting & Comparing](#sorting--comparing)
- [Useful Methods](#useful-methods)
- [Tips](#tips)

## Basics

- `$ irb`: to write ruby in the terminal
- don't use `'` in ruby, use `"` instead
- you can replace most `{}` with `do end` and vice versa –– not true for hashes or `#{}` escapings
- Best Practice: end names that produce booleans with question mark
- CRUD: create, read, update, delete
- `[1,2].map(&:to_i)`
- `integer`: number without decimal
- `float`: number with decimal
- tag your variables:
- - `$`: global variable
- - `@`: instance variable
- - `@@`: class variable
- - starts with capital: constant
- `1_000_000`: 1000000 –– just easier to read
- `nil`: null

## Vars, Contants, Arrays, Hashes & Symbols

```Ruby
my_variable = “Hello”
my_variable.capitalize! # ! methods modify (and return) the object - same as my_variable = my_variable.capitalize
my_variable ||= "Hi" # ||= is a conditional assignment - only set the variable if it was not set before.
my_variable.object_id # unique refid for this object
```

### Constants

```Ruby
MY_CONSTANT = "something" # starts with capital
```

### Arrays

```Ruby
my_array = [a,b,c,d,e]
my_array[1] # b
my_array[2..-1] # c , d , e
multi_d = [[0,1],[0,1]]
[1, 2, 3] << 4 # [1, 2, 3, 4] same as [1, 2, 3].push(4)
```

### Hashes

```Ruby
hash = { "key1" => "value1", "key2" => "value2" } # same as objects in JavaScript
hash = { key1: "value1", key2: "value2" } # the same hash using symbols instead of strings
my_hash = Hash.new # same as my_hash = {} – set a new key like so: pets["Stevie"] = "cat"
pets["key1"] # value1
pets["Stevie"] # cat
my_hash = Hash.new("default value")
hash.select{ |key, value| value > 3 } # selects all keys in hash that have a value greater than 3
hash.each_key { |k| print k, " " } # ==> key1 key2
hash.each_value { |v| print v } # ==> value1value2

my_hash.each_value { |v| print v, " " }
# ==> 1 2 3
```

### Symbols

```Ruby
:symbol # use whenever you need internal identifiers in code (i.e. hash keys, reference method name) 
# :symbol != "symbol" Immutable. Faster.
# Once you have used a Symbol once, any Symbol with the same characters references the same Object in memory.
:test.to_s # converts to "test"
"test".to_sym # converts to :test
"test".intern # :test
# Symbols can be used like this as well:
my_hash = { key: "value", key2: "value" } # is equal to { :key => "value", :key2 => "value" }
```

#### Functions to create Arrays

```Ruby
"bla,bla".split(“,”) # takes string and returns an array (here  ["bla","bla"])
```

## Methods

**Methods**

```Ruby
def greeting(hello, *names) # *names is a split argument, takes several parameters passed in an array 
  return "#{hello}, #{names}"
end

start = greeting("Hi", "Justin", "Maria", "Herbert") # call a method by name

def name(variable=default)
  ### The last line in here gets returned by default
end
```

## Classes

_custom objects_

```Ruby
class Person # class names are rather written in PascalCase (It is similar to camelCase, but the first letter is capitalized)
  @@count = 0
  attr_reader :name # make it readable
  attr_writer :name # make it writable
  attr_accessor :name # makes it readable and writable

  def Methodname(parameter)
    @classVariable = parameter
    @@count += 1
  end

  def self.show_classVariable
    @classVariable
  end

  def Person.get_counts # is a class method
    return @@count
  end

  private

  def private_method; end # Private methods go here
  def private_method_2; end # note this is still private (until end of block or next access modifier)
end

matz = Person.new("Yukihiro")
matz.show_name # Yukihiro
```

## Singleton Methods

_Per-object methods, only available on the Object you defined it on_

```Ruby
class Car
  def inspect
    "Cheap car"
  end
end

porsche = Car.new
porsche.inspect # => Cheap car
def porsche.inspect
  "Expensive car"
end

porsche.inspect # => Expensive car

# Other objects are not affected
other_car = Car.new
other_car.inspect # => Cheap car
```

### Inheritance

```Ruby
class DerivedClass < BaseClass; end # if you want to end a Ruby statement without going to a new line, you can just type a semicolon.

class DerivedClass < Base
  def some_method
    super(optional args) # When you call super from inside a method, that tells Ruby to look in the superclass of the current class and find a method with the same name as the one from which super is called. If it finds it, Ruby will use the superclass' version of the method.
      # Some stuff
    end
  end
end

# Any given Ruby class can have only one superclass. Use mixins if you want to incorporate data or behavior from several classes into a single class.
```

## Modules

```Ruby
module ModuleName # module names are rather written in PascalCase
  # variables in modules doesn't make much sence since modules do not change. Use constants.
end

Math::PI # using PI constant from Math module. Double colon = scope resolution operator = tells Ruby where you're looking for a specific bit of code.

require 'date' # to use external modules.
puts Date.today # 2016-03-18

module Action
  def jump
    @distance = rand(4) + 2
    puts "I jumped forward #{@distance} feet!"
  end
end

class Rabbit
  include Action # Any class that includes a certain module can use those module's methods! This now is called a Mixin.
  extend Action # extend keyword mixes a module's methods at the class level. This means that class itself can use the methods, as opposed to instances of the class.
  attr_reader :name
  def initialize(name)
    @name = name
  end
end

peter = Rabbit.new("Peter")
peter.jump # include
Rabbit.jump # extend
```

## Blocks & Procs

### Code Blocks

_Blocks are not objects._ A block is just a bit of code between do..end or {}. It's not an object on its own, but it can be passed to methods like .each or .select.

```Ruby
def yield_name(name)
  yield("Kim") # print "My name is Kim. "
  yield(name) # print "My name is Eric. "
end

yield_name("Eric") { |n| print "My name is #{n}. " } # My name is Kim. My name is Eric.
yield_name("Peter") { |n| print "My name is #{n}. " } # My name is Kim. My name is Eric. My name is Kim. My name is Peter.
```

### Proc

_Saves blocks and are objects._ A proc is a saved block we can use over and over.

```Ruby
cube = Proc.new { |x| x ** 3 }
[1, 2, 3].collect!(&cube) # [1, 8, 27] # the & is needed to transform the Proc to a block.
cube.call # call procs directly
```

## Lambdas

```Ruby
lambda { |param| block }
multiply = lambda { |x| x * 3 }
y = [1, 2].collect(&multiply) # 3 , 6
```

Diff between procs and lambdas:

- a lambda checks the number of arguments passed to it, while a proc does not (This means that a lambda will throw an error if you pass it the wrong number of arguments, whereas a proc will ignore unexpected arguments and assign nil to any that are missing.)
- when a lambda returns, it passes control back to the calling method; when a proc returns, it does so immediately, without going back to the calling method.
  So: A lambda is just like a proc, only it cares about the number of arguments it gets and it returns to its calling method rather than returning immediately.

## Calculation

- Addition (+)
- Subtraction (-)
- Multiplication (\*)
- Division (/)
- Exponentiation (\*\*)
- Modulo (%)
- The concatenation operator (<<)
- you can do 1 += 1 –– which gives you 2 but 1++ and 1-- does not exist in ruby
- `"A " << "B"` => `"A B"` but `"A " + "B"` would work as well but `"A " + 4 + " B"` not. So rather use string interpolation (`#{4}`)
- `"A #{4} B"` => `"A 4 B"`

## Commenting

```Ruby
=begin
Bla
Multyline comment
=end
```

```Ruby
# single line comment
```

## Conditions

Note everything except `nil` and `false` are true (including `0`).

### If

```Ruby
if 1 < 2
puts “one smaller than two”
elsif 1 > 2 # *careful not to mistake with else if. In ruby you write elsif*
puts “elsif”
else
puts “false”
end
# or
puts "be printed" if true
puts 3 > 4 ? "if true" : "else" # else will be putted
```

### Unless

```Ruby
unless false # unless checks if the statement is false (opposite to if).
puts “I’m here”
else
puts “not here”
end
# or
puts "not printed" unless true
```

### Case

```Ruby
case my_var
  when "some value"
    ###
  when "some other value"
    ###
  else
    ###
end
# or
case my_var
  when "some value" then ###
  when "some other value" then ###
  else ###
end
```

- `&&`: and
- `||`: or
- `!`: not
- `(x && (y || w)) && z`: use parenthesis to combine arguments
- problem = false
- print "Good to go!" unless problem –– prints out because problem != true

## Printing & Putting

```Ruby
print "bla"
puts "test" # puts the text in a separate line
```

## String Methods

```Ruby
"Hello".length # 5
"Hello".reverse # “olleH”
"Hello".upcase # “HELLO”
"Hello".downcase # “hello”
"hello".capitalize # “Hello”
"Hello".include? "i" # equals to false because there is no i in Hello
"Hello".gsub!(/e/, "o") # Hollo
"1".to_i # transform string to integer –– 1
"test".to_sym # converts to :test
"test".intern # :test
:test.to_s # converts to "test"
```

## User Input

```Ruby
gets # is the Ruby equivalent to prompt in javascript (method that gets input from the user)
gets.chomp # removes extra line created after gets (usually used like this)
```

## Loops

### While loop

```Ruby
i = 1
while i < 11
  puts i
  i = i + 1
end
```

### Until loop

```Ruby
i = 0
until i == 6
  puts i
  i += 1
end
```

### For loop

```Ruby
for i in 1...10 # ... tells ruby to exclude the last number (here 10 if we .. only then it includes the last num)
  puts i
end
```

### Loop iterator

```Ruby
i = 0
loop do
  i += 1
  print "I'm currently number #{i}” # a way to have ruby code in a string
  break if i > 5
end
```

### Next

```Ruby
for i in 1..5
  next if i % 2 == 0 # If the remainder of i / 2 is zero, we go to the next iteration of the loop.
  print i
end
```

### .each

```Ruby
things.each do |item| # foreach(item in things)
  print “#{item}"
end
```

on hashes like so:

```Ruby
hashes.each do |x,y| # foreach(x,y in hashes)
  print "#{x}: #{y}"
end
```

### .times

```Ruby
10.times do
  print “this text will appear 10 times”
end
```

### .upto / .downto

```Ruby
10.upto(15) { |x| print x, " " } # 10 11 12 13 14 15
"a".upto("c") { |x| print x, " " } # a b c
```

## Sorting & Comparing

```Ruby
array = [5,4,1,3,2]
array.sort! # = [1,2,3,4,5] – works with text and other as well.
"b" <=> "a" # = 1 – combined comparison operator. Returns 0 if first = second, 1 if first > second, -1 if first < second
array.sort! { |a, b| b <=> a } # to sort from Z to A instead of A to Z
```

## Useful Methods

```Ruby
1.is_a? Integer # returns true
:1.is_a? Symbol # returns true
"1".is_a? String # returns true
[1,2,3].collect!() # does something to every element (overwrites original with ! mark)
.map() # is the same as .collect
1.2.floor # 1 # rounds a float (a number with a decimal) down to the nearest integer.
cube.call # implying that cube is a proc, call calls procs directly
Time.now # displays the actual time
```

## Tips

Everything has a value
```Ruby
x = 10
y = 11
z = if x < y
      true
    else
      false
    end
z # => true
```

Everything is an object
```Ruby
MyClass = Class.new do
  attr_accessor :instance_var
end
# This is the same as
# class MyClass
#   attr_accessor :instance_var
# end
```