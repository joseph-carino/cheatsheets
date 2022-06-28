[back to overview](/../..)
Looking for [Rails](../master/Ruby-on-Rails-Cheatsheet.md)?

# Ruby Cheatsheet<!-- omit in toc -->

- [Basics](#basics)
- [Vars, Contants, & Symbols](#vars-contants--symbols)
  - [Vars](#vars)
  - [Constants](#constants)
  - [Symbols](#symbols)
- [Operators](#operators)
  - [Comparison](#comparison)
  - [Assignment](#assignment)
  - [Calculation](#calculation)
  - [Other](#other)
- [Enumerables](#enumerables)
  - [Arrays](#arrays)
  - [Hashes (Dictionaries)](#hashes-dictionaries)
  - [Ranges](#ranges)
  - [Working with Enumerables](#working-with-enumerables)
  - [OpenStruct](#openstruct)
  - [Environment Variables](#environment-variables)
- [Regular Expressions (Regex)](#regular-expressions-regex)
- [Blocks & Procs & Lambdas](#blocks--procs--lambdas)
  - [Code Blocks](#code-blocks)
  - [Proc](#proc)
  - [Lambdas](#lambdas)
- [Methods](#methods)
- [Classes](#classes)
- [Eigenclass](#eigenclass)
- [Singleton Methods](#singleton-methods)
- [Inheritance](#inheritance)
- [Patterns](#patterns)
  - [Delegation](#delegation)
  - [Observer](#observer)
  - [Singleton](#singleton)
- [Modules](#modules)
  - [Include/Extend](#includeextend)
- [Require/Load](#requireload)
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
- [Exceptions](#exceptions)
- [Useful Methods](#useful-methods)
- [Gems](#gems)
- [Tips](#tips)
- [Debugging](#debugging)
  - [In General](#in-general)
    - [Introspection](#introspection)
    - [ObjectSpace](#objectspace)
  - [Byebug](#byebug)
    - [Generic](#generic)
    - [Control](#control)
    - [Breaking](#breaking)
    - [Frames](#frames)
    - [Info](#info)
    - [Threads](#threads)
    - [Expressions](#expressions)
    - [Options](#options)
    - [Source](#source)
  - [Pry](#pry)
- [Testing](#testing)
  - [RSpec](#rspec)
    - [Basics](#basics-1)
    - [Matchers](#matchers)
    - [Mocks](#mocks)
      - [Create Mocks](#create-mocks)
      - [Method Stubs/Expectations](#method-stubsexpectations)
      - [Spies](#spies)
    - [Extras](#extras)
  - [Fixtures - factory_bot](#fixtures---factory_bot)
    - [Create vs Build](#create-vs-build)
  - [Running Tests](#running-tests)
- [Links](#links)

# Basics

- `irb`: to write ruby in the terminal
- 'duck typing': Objects in Ruby are defined by the methods on them - "If it walks like a duck, and quacks like a duck, then it must be a duck"
- `'` strings are literal, `"` strings allow interpolation and escapes
- you can replace most `{}` with `do end` and vice versa –– not true for hashes or `#{}` escapings
- Best Practice: end names that produce booleans with ?, update the Object with !
- CRUD: create, read, update, delete
- `integer`: number without decimal
- `float`: number with decimal
- tag your variables:
- - `$`: global variable
- - `@`: instance variable
- - `@@`: class variable
- - starts with capital: constant
- `1_000_000`: 1000000 –– just easier to read
- `nil`: null
- `;`: statement separator - like newline
- `::`: namespace resolution operator - for accessing other namespaces
- Method calls are really messages:
  ```Ruby
  # This
  1 + 2
  # Is the same as this ...
  1.+(2)
  # Which is the same as this:
  1.send "+", 2
  ```
- `PORO` == POJO

# Vars, Contants, & Symbols

## Vars

```Ruby
my_variable = “Hello”
my_variable.capitalize!       # ! methods modify (and return) the object - same as my_variable = my_variable.capitalize
my_variable ||= "Hi"          # ||= is a conditional assignment - only set the variable if it was not set before.
my_variable.object_id         # unique refid for this object
```

## Constants

```Ruby
MY_CONSTANT = "something"     # starts with capital, unenforced, can't define in method
Class.constants               # returns constants defined in class Class
const_get(:name)              # return value of constant "name"
const_set(:name, value)       # set value of constant "name" to value
const_missing(:symbol)        # invoked when reference made to undefined constant, accepting constant name
const_defined?(:name)         # returns true if constant is defined
remove_const(:name)           # remove constant "name"
private_constant(:name)       # makes constant private
# NOTE: Classes are constants!
module Food
  class Bacon; end
end
Food.constants                #=> [:Bacon]
```
Note: constant lookup starts with class containing the method called, NOT the class called on. [See More](https://hoffm.medium.com/a-puzzle-about-ruby-constants-e958d15dbada)

## Symbols

```Ruby
:symbol                       # use: internal identifiers in code (i.e. hash keys, reference method name)
                              # :symbol != "symbol" Immutable. Faster.
                              # Once used, any Symbol with the same name references the same memory Object
:test.to_s                    # converts to "test"
"test".to_sym                 # converts to :test
"test".intern                 # :test
```

# Operators

## Comparison
- `==`, `!=`, `>`, `<`, `>=`, `<=` as usual
- `===`: used for case statements - exact comparison
- `==`: compare values and not types
- `<=>`: comparison, 0 if equal, 1 if a>b, -1 if a<b
- `.eql?`: used for hash key comparisons, compare hash (value)
- `.equal?`: true if same object id (exact same object)

## Assignment
- Assignment operators: `+=`, `-=`, `*=`, `/=`, `%=`, `**=` but NOT 1++ or 1--
- `a, b, c = 10, 20, 30` is the same as `a = 10` `b = 10` `c = 10`

## Calculation
- Normal: `+`, `-`, `*`, `/`, `%`, `**` (exponent)
- The concatenation operator (<<) `"a" << "b"   #=> "ab"`

## Other
- `condition ? "true" : "false"`: ternary
- `1..9`: inclusive range (1..9)
- `1...9`: exclusive range (1..8)
- `defined? x`: true if x is (variable) initialized or (method) defined
- `.`: "message" operator -> reference class variables/methods
- `::`: "scope" operator -> reference class variables/methods and constants/namespaced things
- `&.`: safe navigation, null coalesce
- `&<object>`: if object is a `Proc` ? convert to block : call `<object>.to_proc`, then convert to block

# Enumerables

## Arrays

```Ruby
my_array = [a,b,c,d,e]
my_array[1] # b
my_array[2..-1] # c , d , e
multi_d = [[0,1],[0,1]]
[1, 2, 3] << 4 # [1, 2, 3, 4] same as [1, 2, 3].push(4)
```

## Hashes (Dictionaries)

```Ruby
hash = { "key1" => "value1", "key2" => "value2" } # same as objects in JavaScript
hash = { key1: "value1", key2: "value2" }         # same hash, keys are turned into symbols
key1 = "value1", key2 = "value2"
hash = { key1:, key2: }                           # same hash, value fetched by name of key
Hash.new                                          #=> {}
Hash([])                                          #=> {}
Hash(key: value)                                  #=> {:key => :value}

h.default                                         #=> nil - return on key not found lookups
h.default = 0                                     #
Hash.new("val")                                   # default = "val"
h.default_proc                                    #=> nil - if set, used instead of default
h.default_proc = proc { |hash, key| "#{key}" }    # key not found lookups call proc
Hash.new{ |hash, key| "#{key}" }                  # default_proc set to block
h[:nokey]                                         #=> "nokey" - note this doesn't add the key to hash
h.include?(:nokey)                                #=> false

pets["Stevie"] = "cat"                            # set key to value
pets["Stevie"]                                    #=> cat
hash.select { |key, value| value > 3 }            # selects all keys in hash that have a value greater than 3
hash.filter { |key, value| value > 3 }            # alias to select
hash.each_key { |k| print k, " " }                #=> key1 key2
hash.each_value { |v| print v, " " }              #=> 1 2 3
```

## Ranges

```Ruby
# technically can use anything that implements <=>
(-1..-5).to_a      #=> []
(-5..-1).to_a      #=> [-5, -4, -3, -2, -1]
('a'..'e').to_a    #=> ["a", "b", "c", "d", "e"]
('a'...'e').to_a   #=> ["a", "b", "c", "d"]
```

## Working with Enumerables

```Ruby
"bla,bla".split(“,”)                #=> ["bla","bla"] takes string and returns an array
["1", "2"].each { |i| puts i }      #=> 12 - do block on each element
enum.all?(&:valid?)                 # returns true/false if all elements return true for block

[1,2,3].collect {|i| i.to_str}      #=> ["1", "2", "3"] runs block on every element and returns new array
[1,2,3].map(&:to_str)               #=> ["1", "2", "3"] alias collect. NOTE: symbol to block with &

enum.reduce(symbol)                 # for each e in enum, memo.symbol(e) => memo
(5..10).reduce(:+)                  #=> 45
(5..10).reduce(1, :*)               #=> 151200 - arg is initial value of memo, otherwise use first element
enum.inject {|memo, e| memo + e}    # for each e in enum, run block producing memo
(5..10).inject{ |sum, n| sum + n }  #=> 45 - note reduce == inject
(5..10).inject(1) { |p, n| p * n }  #=> 151200 - initial value used
H = {"Key1" => 1, "Key2" => 2}
H.values.reduce(:+)                 #=> 3 - below are also 3, but note will return 0 if H is nil
H.values.reduce(0) { |sum,x| sum + x }
H.reduce(0) { |sum,(key,val)| sum + val }

object.freeze                       # now immutable, modifications will throw runtime error

enum.group_by { |x| y }             # return hash: keys = y, values = array of x that correspond to y
(1..6).group_by { |i| i%3 }         #=> {0=>[3, 6], 1=>[1, 4], 2=>[2, 5]}
```

## OpenStruct
[See more](https://ruby-doc.org/stdlib-2.3.1/libdoc/ostruct/rdoc/OpenStruct.html)
```Ruby
OpenStruct.new          # create new arbitrary object
aus = OpenStruct.new(:country => "Australia", :countryCode => "AUS")
p aus                   # => <OpenStruct country="Australia" countryCode=""AUS">
```

## Environment Variables
[See more](https://www.rubyguides.com/2019/01/ruby-environment-variables/)
`ENV` object behaves like a hash and gives access to env vars defined outside ruby
```Ruby
ENV.keys                              # returns all env var keys
ENV["GEM_HOME"]                       #=> "/Users/josephcarino/.gem/ruby/3.1.2"
```

# Regular Expressions (Regex)
- [Tester](https://rubular.com/)
- Wrap regex in '/'
```Ruby
text = "A regular expression pleases me"
text.match(/expression/)              #=> MatchData or nil
text.scan(/\b[aeiou][a-z]*\b/)        #=> ["expression"] - note case sensitive
```

# Blocks & Procs & Lambdas

## Code Blocks

_Blocks are not objects._ Enclosed in do..end or {}.
`yield`: run block

```Ruby
def yield_name(name)
  return "No block given" unless block_given?
  yield("Kim")                                          # print "My name is Kim. "
  yield(name)                                           # print "My name is <name>. "
end

yield_name("Eric") # => "No block given"
yield_name("Eric") { |n| print "My name is #{n}. " }    # My name is Kim. My name is Eric.
yield_name("Peter") { |n| print "My name is #{n}. " }   # My name is Kim. My name is Peter.

def explicit_block(&block)
  block.call # same as yield
end
```

## Proc

_Function object._ A proc is a saved block object.

```Ruby
cube = Proc.new { |x| x ** 3 }
[1, 2, 3].collect(&cube)        #=> [1, 8, 27]
cube.call(3)                    #=> 27 call procs directly

def block(&the_block)           # take block and turn it into a proc
  the_block                     # return the proc
end
adder = block { |a, b| a + b }  # adder is now a Proc object
adder.class                     #=> Proc

# any method that responds to #to_proc can be coerced to proc with & [see more](https://maximomussini.com/posts/ruby-to_proc)
class Doubler
  def double(n)
    n * 2
  end

  def to_proc
    method(:double).to_proc
  end
end

doubler = Doubler.new
[1, 2, 3].map(&doubler)           #=> [2, 4, 6]

# Symbol.to_proc sends message of same name as symbol to object
[1,2,3].map(&:to_s)               # => ["1", "2", "3"]
```

## Lambdas

_What is a lambda?_ #todo
```Ruby
lambda { |param| block }
multiply = ->(x) { x * 3 }        #
multiply = lambda { |x| x * 3 }   #same as prev line
multiply.call(4) # => 12
y = [1, 2].collect(&multiply) # 3 , 6
```

Diff between procs and lambdas:

- a lambda checks the number of arguments passed to it, while a proc does not (This means that a lambda will throw an error if you pass it the wrong number of arguments, whereas a proc will ignore unexpected arguments and assign nil to any that are missing.)
- when a lambda `return`s, it passes control back to the calling method; when a proc `return`s, it does so immediately, without going back to the calling method.

# Methods

```Ruby
# Argument types are used in this order: required -> optional -> variable -> keyword
def testing(a, b = 1, *c, d: 1, **x)      # required, optional, variable, keyword optional, double splat
  p a,b,c,d,x
end
testing('a', 'b', 'c', 'd', 'e', d: 2, x1: 1, x2: 2)
=begin
"a"
"b"
["c", "d", "e"]
2
{:x1=>1, :x2=>2}
=> ["a", "b", ["c", "d", "e"], 2, {:x1=>1, :x2=>2}]
=end

def greet(hello, name, exclaim = false)   # 2 args in order plus default value
  return "#{hello}, #{name}#{exclaim ? '!' : ''}"
end
greet("Hi", "Joe")                        #=> "Hi, Joe"

def greet2(hello:, name:, exclaim: false) # 2 args by name plus default value
  return "#{hello}, #{name}#{exclaim ? '!' : ''}"
end
greet2(hello:"Hi", name:"Joe")            #=> "Hi, Joe"

def greet3(hello, *names)                 # *names is variable argument; other arguments combined into array
  return "#{hello}, #{names}"
end
greet3("Hi", "Joe", "Justin")             # => "Hi, [\"Joe\", \"Justin\"]"

object.method(:method_name)               #=> like object.method_name, reflection

def greet4(opener, **exclaims)            #=> create hash of named arguments that don't match
  exclaims.reduce(opener) { |all, (key, val)| all + " #{key} says #{val}!"}
end
greet4(opener: "Greetings!", joe: "Hi", john: "Hello")          #=> "Greetings! joe says Hi! john says Hello!"
```

# Classes

_custom objects_

```Ruby
class Person                    # class names are PascalCase
  @@count = 0
  attr_reader :name             # make it readable
  attr_writer :name             # make it writable
  attr_accessor :name           # makes it readable and writable

  def Methodname(parameter)
    @classVariable = parameter
    @@count += 1
  end

  def self.show_classVariable   # class method - static method, self = Person
    @classVariable
  end

  def Person.get_counts         # class method - static method
    return @@count
  end

  private

  def private_method; end       # Private methods go here
  def private_method_2; end     # note this is still private (until end of block or next access modifier)
end

matz = Person.new("Yukihiro")
matz.show_name                  # Yukihiro
```

# Eigenclass
Everything has a metaclass called the eigenclass - copy of original class owned by instance
`Eigenclass`: Class that follows the Singleton pattern
```Ruby
matz = Object.new
def matz.speak                            # define a method on the matz's object's metaclass
  "Place your burden to machine's shoulders"
end

matz.class                                #=> Object - metaclass is invisible

class << matz                             # retarget self, add method to matz eigenclass
  def speak2
    'speak2'
  end
end

metaclass = class << matz; self; end      # assign metaclass to matz's class
metaclass.instance_methods.grep(/speak/)  #=> ["speak", "speak2"] - defined on this instance, not on Object
```

# Singleton Methods

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

# Inheritance

```Ruby
class DerivedClass < BaseClass; end

class DerivedClass < Base
  def some_method
    super(optional args) # call super.some_method
      # Some stuff
    end
  end
end

# Single inheritance only. Use mixins for multiple inheritance/extension
```

# Patterns
## Delegation
'Evaluating a member of a receiving object in the context of a sending object.'
```Ruby
# SimpleDelegator
require 'delegate'
User = Struct.new(:first_name, :last_name)
class UserDecorator < SimpleDelegator
  def full_name
    "#{first_name} #{last_name}"
  end
  def first_name
    "#{super[0]}."                               # call corresponding method on wrapped object
  end
end
john_user = User.new("John", "Doe")
decorated_user = UserDecorator.new(john_user)    #=> create delegator wrapping User object
decorated_user.first_name                        #=> "J."
decorated_user.full_name                         #=> "J. Doe"

# DelegateClass(ClassToDelegateTo)
class Logfile < DelegateClass(File)
  MODE = File::WRONLY|File::CREAT|File::APPEND

  def initialize(basename, logdir = '/var/log')
    path = File.join(logdir, basename)           # Create logfile in logdir location
    logfile = File.open(path, MODE, 0644)

    super(logfile)                               # call Delegator.initialize. Logfile now wraps File
  end
end

# Forwardable
require 'forwardable'
User = Struct.new(:first_name, :last_name)
class UserDecorator
  extend Forwardable
  def_delegators :@user, :first_name             # explicitly declare for each method to forward
  def_delegator :@user, :last_name, :family_name # explicitly declare last_name and rename to family_name
  def initialize(user)
    @user = user
  end
  def full_name
    "#{first_name} #{family_name}"
  end
end

john_user = User.new("John", "Doe")
decorated_user = UserDecorator.new(john_user)
decorated_user.full_name                         #=> "John Doe"
decorated_user.last_name                         #=> NoMethodError

# delegate
User = Struct.new(:first_name, :last_name)
class UserDecorator
  attr_reader :user
  delegate :first_name, to: :user
  delegate :last_name, to: :user, prefix: true  # alternate: prefix: account to add account_last_name
                                                # also: :allow_nil (return null on nil user)
  def initialize(user)
    @user = user
  end
  def full_name
    "#{first_name} #{user_last_name}"
  end
end

decorated_user = UserDecorator.new(User.new("John", "Doe"))
decorated_user.full_name                         #=> "John Doe"

# casting
require 'casting'
User = Struct.new(:first_name, :last_name)
module UserDecorator
  def full_name
    "#{first_name} #{last_name}"
  end
end

user = User.new("John", "Doe")
user.extend(Casting::Client)                      # alternate: include Casting::Client inside User class
user.delegate(:full_name, UserDecorator)
```

## Observer
Object (subject) maintains list of dependents (observers), and automatically notifies them of any state changes
```Ruby
require "observer"

class Moderator
  include Observable
  def initialize(name)
    @name = name
  end
  def write
    message = "Computer says: No"
    changed                             # assert subject has changed
    notify_observers(message)           # notify observers, call their :update method
  end
end

class Subscriber
  def initialize(moderator, limit)
    @limit = limit
    moderator.add_observer(self)        # add to subject's observers list
  end
  def update(message)                   # required to be observer
    puts "#{message}"
  end
end

moderator = Moderator.new("Rupert")
Subscriber.new(moderator, 1)
moderator.write
moderator.write
# prints "Computer says: No" twice
```

## Singleton

```Ruby
require 'singleton'

class Logger
  include Singleton

  def initialize
    puts 'Created'
  end
end

Logger.new                #=> NoMethodError - constructor is private

first = Logger.instance   # prints 'Created'
second = Logger.instance  # prints nothing
first == second           #=> true
```

# Modules

```Ruby
module ModuleName # module names are rather written in PascalCase
  # variables in modules doesn't make much sence since modules do not change. Use constants.
end

Math::PI # using PI constant from Math module. Double colon = scope resolution operator

require 'date' # to use external modules.
puts Date.today # 2016-03-18

module Action
  def jump
    @distance = rand(4) + 2
    puts "I jumped forward #{@distance} feet!"
  end
end
```

## Include/Extend
```Ruby
# If you *include* a class with a module, you're "bringing in" the module's methods as instance methods. See Mixin
# If you *extend* a class with a module, you're "bringing in" the module's methods as class methods.
module A
  def say
    puts "this is module A"
  end
end
class B
  include A
end
class C
  extend A
end

B.say #=> undefined method 'say' for B:Class
B.new.say #=> this is module A
C.say #=> this is module A
C.new.say #=> undefined method 'say' for C:Class

# Note: include/extend methods have scope of original module, not class called from
```

# Require/Load

Similar to import/using. Returns true if found.
```Ruby
load `example_module.rb`            # Load file into code when code executed

require './app/example_file.rb'     # Load file into code when code executed, caches file
require 'example_file'              # shortened name - depends on the listed directories in $LOAD_PATH

require_relative 'example_file.rb'  # use for files in your directory

require_dependency
```

# Commenting

```Ruby
=begin
Bla
Multyline comment
=end
```

```Ruby
# single line comment
```

# Conditions

Note everything except `nil` and `false` are true (including `0`).

## If

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

## Unless

```Ruby
unless false # unless checks if the statement is false (opposite to if).
puts “I’m here”
else
puts “not here”
end
# or
puts "not printed" unless true
```

## Case

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

# Printing & Putting

```Ruby
print "bla"
puts "test" # puts the text in a separate line
```

# String Methods

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

# User Input

```Ruby
gets # is the Ruby equivalent to prompt in javascript (method that gets input from the user)
gets.chomp # removes extra line created after gets (usually used like this)
```

# Loops

## While loop

```Ruby
i = 1
while i < 11
  puts i
  i = i + 1
end
```

## Until loop

```Ruby
i = 0
until i == 6
  puts i
  i += 1
end
```

## For loop

```Ruby
for i in 1...10 # ... tells ruby to exclude the last number (here 10 if we .. only then it includes the last num)
  puts i
end
```

## Loop iterator

```Ruby
i = 0
loop do
  i += 1
  print "I'm currently number #{i}” # a way to have ruby code in a string
  break if i > 5
end
```

## Next

```Ruby
for i in 1..5
  next if i % 2 == 0 # If the remainder of i / 2 is zero, we go to the next iteration of the loop.
  print i
end
```

## .each

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

## .times

```Ruby
10.times do
  print “this text will appear 10 times”
end
```

## .upto / .downto

```Ruby
10.upto(15) { |x| print x, " " } # 10 11 12 13 14 15
"a".upto("c") { |x| print x, " " } # a b c
```

# Sorting & Comparing

```Ruby
array = [5,4,1,3,2]
array.sort! # = [1,2,3,4,5] – works with text and other as well.
"b" <=> "a" # = 1 – combined comparison operator. Returns 0 if first = second, 1 if first > second, -1 if first < second
array.sort! { |a, b| b <=> a } # to sort from Z to A instead of A to Z
```

# Exceptions
- Exception > StandardError > errors. Do not rescue Exception, used for system level events/signals.
- If raised but not yet handled, $! contains exception and $@ contains its backtrace
- Put custom errors into module, inherit from a base error (StandardError > MyLib::Error > MyLib::CustomError)
```Ruby
begin
  raise ArgumentError.new("arg")
  raise RuntimeError, "You messed up!"
  raise "You messed up!"                # raises RuntimeError if unspecified
rescue ArgumentError, RuntimeError => error
  #... error handler for ArgumentError or RuntimeError
  retry                                 # optional - retry begin block
rescue => error                         # StandardError
  #... error handler for any other errors
else
  #... executes when no error
ensure
  #... always executed - 'finally'
end

grade = 100 / nil rescue 0.0    # => 0.0

for x in expression do
    #... loop body
end rescue ...error handler...  # note loop will exit when rescued. use next in handler to coninue

.each {} rescue ...error handler...
```

# Useful Methods

```Ruby
1.is_a? Integer   # returns true
:1.is_a? Symbol   # returns true
"1".is_a? String  # returns true
1.2.floor         #=> 1 - rounds a float (a number with a decimal) down to the nearest integer.
cube.call         # where 'cube' is a proc, calls procs directly
Time.now          # displays the actual time
```

# Gems
External packages
```Bash
$ gem search ^rails
$ gem install drip
$ gem list
```
Require a gem places its /lib directory on the load path
```Ruby
require 'ap'        #=> true
pp $LOAD_PATH.first # ".../gems/awesome_print-1.0.2/lib"
```

# Tips

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

`self` - basically 'this' but changes in context
```Ruby
module Reflection
  def reflect
    self
  end
end

class Ghost
  include Reflection #or 'extend Reflection', same outcome - Ghost.reflect == Ghost is true
end

g = Ghost.new
g.reflect == g # => true
```
And you can retarget self:
```Ruby
class << "test"
  puts self.inspect
end

# => #<Class:#<String:0x007f8de283bd88> - this is the metaclass
```
You can use the above to easily create class methods:
```Ruby
class Person1
  def self.species
    "Homo Sapien"
  end

  self.name #=> "Person1"
end
Person1.name #=> "Person1"
Person1.species #=> "Homo Sapien"

class Person2
  class << self
    def species
      "Homo Sapien"
    end

    self.name #=> ""
  end
end
Person2.name #=> "Person2"
Person2.species #=> "Homo Sapien"

class << Person3
  def species
    "Homo Sapien"
  end

  self.name #=> ""
end
Person3.name #=> "Person3"
Person3.species #=> "Homo Sapien"

# with instance_eval, new methods are defined on the metaclass, but the self is the object itself
Person4.instance_eval do
  def species
    "Homo Sapien"
  end

  self.name #=> "Person4"
end
Person4.name #=> "Person3"
Person4.species #=> "Homo Sapien"
```

# Debugging
[At Shopify](https://development.shopify.io/engineering/keytech/reference/ruby_rails/ruby_debugging)
[For Spin](https://development.shopify.io/engineering/keytech/spin/ruby-debugger-in-spin)
[For Spin on SFN](https://sfn.docs.shopify.io/Spin/SFN-on-Spin-FAQ#how-do-i-debug-with-bindingpry-byebug-or-the-rails-debugger)
## In General

### Introspection
```Ruby
someObject.inspect                          #=> lots of info about this object
someObject.class.name                       #=> someObject's class' name
someObject.instance_of?(someObject.class)   #=> true
someObject.methods.inspect                  #=> list of methods you can call on someObject
someObject.respond_to?(:someMethod)         #=> true if someObject has method someMethod
someObject.instance_variables.inspect       #=> [:@instanceVar, :@instanceVar2]
```

### ObjectSpace
```Ruby
ObjectSpace.each_object.inject(Hash.new 0) { |h,o| h[o.class] += 1; h }.sort_by { |k,v| -v }.take(10).each { |klass, count| puts "#{count.to_s.ljust(10)} #{klass}" }
# Prints table with object count for top-10 classes
ObjectSpace.each_object(String).sort_by { |s| s.size }.each { |s| p s }
# Prints all in-memory strings, sorted by size
o = "a" * 100
ObjectSpace.memsize_of(o) # prints size in memory of object
```


## Byebug
### Generic
- Note: 'Enter' repeats last command
- `byebug` - add to source to break into debugger at next line
- `q[uit]` - quit and exit program (use `q!` for no prompt) or `kill` to REALLY quit
- `restart` - restart the program and byebug
- `h[elp] [<command-name>]` - get help on `<command-name>` or list all commands
- `b[ack]t[race]` or `w[here]` - display stack trace
- `hist[ory] <n>` - display last `<n>` commands (default all)

### Control
- `c[ontinue] [<line-number>]` - continue until end, breakpoint, or line `<line-number>`
- `n[ext] [<x>]` - continue for `<x>` lines (default 1)
- `s[tep] [<x>]` - step into, repeat `<x>` times (default 1)
- `fin[ish] [<num-frames>]` - run until `<num-frames>` have returned (default current)

### Breaking
- `b[reak] [<filename>:<line-number>|<class>(.|#)<method>] [<condition>]` - set breakpoint at `<filename>:<line-number>` or start of `<method>` in `<class>` or default current line - if optional `<condition>` is true
- `info breakpoints` - list all breakpoints
- `cond[ition] <n> [<expression>]` - add condition `<expression>` to breakpoint `<n>` or removes if blank
- `del[ete] [<n>]` - deletes breakpoint `<n>` or all breakpoints
- `disable breakpoints [<n>]` - disable breakpoint `<n>` or all breakpoints
- `cat[ch] <exception> [off]` - enable or disable (with `off`) catchpoint on `<exception>`
- `cat[ch]` - list all catchpoints
- `cat[ch] off` - deletes all catchpoints
- `sk[ip]` - passes caught exception back to application

### Frames
- `f[rame] [<n>]` - moves to frame number `<n>` or show current frame (default)
- `up [<n>]` - move up `<n>` frames (default 1)
- `down [<n>]` - move down `<n>` frames (default 1)

### Info
- `info [args|locals|instance_variables|global_variables|variables]` - show info of current frame
- `m[ethod] <class|module>` - show instance methods of given `<class>`/`<module>`
- `m[ethod] i[nstance] <object>` - show methods of `<object>`
- `m[ethod] iv <object>` - show instance variables of `<object>` - same as `v[ar] i[nstance] <object>`
- `v[ar] cl[ass]` - show class variables of self
- `v[ar] co[nst] <object>` - show constants of `<object>`
- `v[ar] g[lobal]` - same as `info global_variables`
- `v[ar] l[ocal]` - same as `info locals`

### Threads
- `th[read]` - show current thread
- `th[read] l[ist]` - list all threads
- `th[read] <number>` - switch context to thread `<number>`
- `th[read] stop|resume <number>` - stop or resume thread `<number>`

### Expressions
- `e[val]|p <exp>` - evaluate `<exp>` and print (you don't need `e|p` but `set noautoeval` disables this)
- `pp <exp>` - evaluate `<exp>` and pretty-print
- `putl <exp>` - evaluate `<exp>` with an array result and columnize the output
- `ps <exp>` - evaluate `<exp>` with an array result, sort and columnize output
- `disp[lay] <exp>` - evaluate `<exp>` whenever program halts
- `info display` - list current display expressions - same as `disp[lay]` no args
- `undisp[lay] [<n>]` - remove display expression `<n>` or all
- `disable|enable display <n>` - disable/enable display expression `<n>`

### Options
Options: autoeval, autoirb, autolist, autoreload, autosave, basename, callstyle, forcestep, fullpath, histfile, histsize, linetrace, tracing_plus, listsize, post_mortem, stack_on_error, testing, verbose, width
- `save <file>` - save current session options in file `<file>`
- `source <file>` - load current session options from file `<file>`
- `set <option>` - change value of option `<option>`
- `show <option>` - view value of option `<option>`

### Source

- `reload` - reload source code
- `info file` - information about the current source file
- `info files` - all currently loaded files
- `info line` - shows the current line number and filename
- `l[ist]` - shows source code after the current line
- `l[ist] –` - shows source code before the current line
- `l[ist] =` - shows source code centred around the current line
- `l[ist] <first>-<last>` - shows all source code from `<first>` to `<last>` line numbers
- `edit [<file:lineno>]` - edit `<file>` (default current file)

## Pry
[See here](https://github.com/pry/pry)

# Testing

## RSpec
[Tutorial](https://www.rubyguides.com/2018/07/rspec-tutorial/)

### Basics
```Ruby
require 'rspec/autorun'

describe Factorial do
  let(:calculator) { Factorial.new } # available to all it in this describe, lazy
  #let! is eager
  it "finds the factorial of 5" do # test name
    expect(calculator.factorial_of(5)).to eq(120)
  end
end

describe Factorial do
  #subject is special let, and auto-defined to the describe
  subject { Factorial.new } # this is the default implicit subject and not necessary
  subject(:calculator) { Factorial.new } # same as let, but allows implicit and named use of subject
  it { should be_an_instance_of(Factorial) }
  it { calculator.should be_an_instance_of(Factorial) }
end

class Factorial
  def factorial_of(n)
    (1..n).inject(:*)
  end
end
```

### Matchers
[You should use expect instead of should/should_not except in one liners](https://github.com/rspec/rspec-expectations/blob/main/Should.md)
```Ruby
expect(x).to be_y # where y is a predicate method (i.e. empty? nil?) to be called on x
expect([1,2,3]).to include(3)
expect([1,2,3]).to start_with(1)
expect([1,2,3]).to end_with(3)
expect(/like/).to match('do you like') # regex match
expect(3).to be_between(1,3)
expect(2).to be_between(1,3).exclusive # default inclusive
expect({:a => 1, :b => 2}).to have_key(:a)
expect({:a => 1, :b => 2}).to have_value(1)
expect{ :x.count }.to raise_error(NoMethodError) # use block
expect{ stock.increment }.to change(stock, :value).by(100)
```

### [Mocks](https://relishapp.com/rspec/rspec-mocks/docs)
#### Create Mocks
```Ruby
  # double = mock object
  obj = double("identifier")
  obj = instance_double("class_name") # mock of class_name
  obj = double("book", :title => "The RSpec Book") # creates test double and declare method stub
```
#### Method Stubs/Expectations
```Ruby
  # specify a return value using `:expect` syntax
  allow(obj).to receive(:message) { :value }
  allow(obj).to receive(:message).and_return(:value)
  allow(obj).to receive(:message).and_return(1, 2, 3) # return values in order, last one for subsequent

  # raise/throw
  allow(obj).to receive(:message).and_raise("this error")
  allow(obj).to receive(:message).and_throw(:this_symbol)
  allow(obj).to receive(:message) { raise "this error" }
  allow(obj).to receive(:message) { throw :this_symbol }

  # arguments
  allow(obj).to receive(:message).with('an argument') { ... }
  allow(obj).to receive(:message).with('more_than', 'one_argument') { ... }
  allow(obj).to receive(:message).with(anything()) { ... }
  allow(obj).to receive(:message).with(an_instance_of(Money)) { ... }
  allow(obj).to receive(:message).with(hash_including(:a => 'b')) { ... }
  allow(obj).to receive(:message).with(array_including(1,2,3)) { ... }
  allow(obj).to receive(:message).with(/abc/) { ... }

  # chaining
  allow(double).to receive_message_chain("foo.bar") { :baz }
  allow(double).to receive_message_chain(:foo, :bar => :baz)
  allow(double).to receive_message_chain(:foo, :bar) { :baz }
  # Given any of the above forms:
  double.foo.bar # => :baz

  #replace 'allow' with 'expect' to validate message received
```

#### Spies
```Ruby
  # verify object received message
  invite = spy("invite") # => same as `double("invite").as_null_object` (or use instance_double etc)

  user.accept_invitation(invite)

  expect(invite).to have_received(:accept)

  # You can also use other common message expectations. For example:
  expect(invite).to have_received(:accept).with(mailer)
  expect(invite).to have_received(:accept).twice
  expect(invite).to_not have_received(:accept).with(mailer)

  # One can specify a return value on the spy the same way one would a double.
  invite = spy('invite', :accept => true)
  expect(invite).to have_received(:accept).with(mailer)
  expect(invite.accept).to eq(true)
```

### Extras
```Ruby
# before/after all or each test
describe Shop do
  before(:all) { Shop.prepare_database }
  before(:each) {Shop.startup}
  after (:each) {Shop.teardown}
  after (:all) { Shop.cleanup_database }
end

# group related tests with context blocks
describe Course do
  context "when user is logged in" do
    it "displays the course lessons" do
    end
    it "displays the course description" do
    endFac
  end
  context "when user it NOT logged in" do
    it "redirects to login page" do
    end
    it "it shows a message" do
    end
  end
end

# don't use shared_examples

# Use let (lazy) or let! (eager, before each example) to define a memozied helper method
$count = 0
describe "let" do
  let(:count) { $count += 1 }

  it "memoizes the value" do
    count.should eq(1)
    count.should eq(1)
  end

  it "is not cached across examples" do
    count.should eq(2)
  end
end

$count = 0
describe "let!" do
  invocation_order = []

  let!(:count) do
    invocation_order << :let!
    $count += 1
  end

  it "calls the helper method in a before hook" do
    invocation_order << :example
    invocation_order.should == [:let!, :example]
    count.should eq(1)
  end
end
# NOTE: skip tests with xit instead of it
```

## Fixtures - factory_bot
[Getting Started](https://github.com/thoughtbot/factory_bot/blob/master/GETTING_STARTED.md)

### Create vs Build
- The `create` method persists the model instance (creates a DB record) while the `build` method does not

## Running Tests
`ruby tests.rb [-e name_of_test]` - run tests including 'name_of_test', or all
- `-f [p|d|j|html]` - change output format, default is `p[rogress]`: ./F/E
- `--profile` - output time for each test

# Links
- [Official Ruby FAQ](https://www.ruby-lang.org/en/documentation/faq/)
