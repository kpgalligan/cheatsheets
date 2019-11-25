[back to overwiev](/../..)  
Looking for [Rails](../master/Ruby-on-Rails-Cheatsheet.md)?

# Ruby Cheatsheet

##### Table of Contents

[Basics](#basics)  
[Vars, Constants, Arrays, Hashes & Symbols](#vars-constants-arrays-hashes--symbols)  
[Methods](#methods)
[Classes](#classes)
[Modules](#modules)
[Blocks & Procs](#blocks--procs)  
[Lambdas](#lambdas)
[Calculation](#calculation)  
[Comment](#comment)  
[Conditions](#conditions)  
[Printing & Putting](#printing--putting)  
[User Input](#user-input)  
[Loops](#loops)
[Sorting & Comparing](#sorting--comparing)  
[Usefull Methods](#usefull-methods)

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
- `1_000_000`: 1000000 –– just easier to read\*

## Vars, Contants, Arrays, Hashes & Symbols

```Ruby
my_variable = “Hello”
my_variable.capitalize! # ! changes the value of the var same as my_name = my_name.capitalize
my_variable ||= "Hi" # ||= is a conditional assignment only set the variable if it was not set before.
```

### Constants

```Ruby
MY_CONSTANT = # something
```

### Arrays

```Ruby
my_array = [a,b,c,d,e]
my_array[1] # Getting value from Array, which is 'b' in this case
my_array[4] = f # Setting value in Array
[1, 2, 3] << 4 # [1, 2, 3, 4] same as [1, 2, 3].push(4)

# Other stuff
my_array[2..-1] # c , d , e
multi_d = [[0,1],[0,1]]

```

### Hashes

```Ruby
hash = { "key1" => "value1", "key2" => "value2" } # same as objects in JavaScript
hash = { key1: "value1", key2: "value2" } # the same hash using symbols instead of strings
my_hash = {} # Create an empty hash

pets["key1"] # Getting a value for key 'key1', which is 'value1'
pets["key3"] = "value3" # Setting value for key 'key3' to 'value3'

pets["key4"] # This doesn't exist, so will return nil

if pets["somekey"]
 # Key has a value
else
 # Key doesn't have a value
end

# Remember hashes are good to group stuff by a string
# If you have an array of people objects, and want to group by hair color, iterate on people, hash key is 
# hair color string

hair_hash = {}
people.each {|person|
 if hair_hash[person.hair]
  hair_hash[person.hair] << person
 else
  hair_hash[person.hair] = [person]
 end
}

hair_hash["brown"] # Gets an array of people objects with brown hair

# Also remember counting

hair_count_hash = {}
people.each {|person|
 if hair_count_hash[person.hair]
  hair_count_hash[person.hair] += 1
 else
  hair_count_hash[person.hair] = 1
 end
}

hair_count_hash["brown"] # Gets an integer, which is the number of people with brown hair


# Other Stuff
hash.select{ |key, value| value > 3 } # selects all keys in hash that have a value greater than 3
hash.each_key { |k| print k, " " } # ==> key1 key2
hash.each_value { |v| print v } # ==> value1value2

my_hash.each_value { |v| print v, " " }
# ==> 1 2 3
```

### Symbols

```Ruby
:symbol # symbol is like an ID in html. :Symbols != "Strings"
# Symbols are often used as Hash keys or referencing method names.
# They can not be changed once created. They save memory (only one copy at a given time). Faster.
:test.to_s # converts to "test"
"test".to_sym # converts to :test
"test".intern # :test
# Symbols can be used like this as well:
my_hash = { key: "value", key2: "value" } # is equal to { :key => "value", :key2 => "value" }
```

#### Functions to create Arrays

```Ruby
"bla,bla".split(“,”) # takes sting and returns an array (here  ["bla","bla"])
```

## Methods

**Methods**

```Ruby
def greeting(hello, *names) # *name is a splat argument, takes several parameters passed in an array
  return "#{hello}, #{names}"
end

start = greeting("Hi", "Justin", "Maria", "Herbert") # call a method by name

def name(variable=default)
  ### The last line in here get's returned by default
end
```

## Classes

_custom objects_

```Ruby
class ClassName # class names are rather written in camelcase
  @@count = 0
  attr_reader :name # make it readable
  attr_writer :name # make it writable
  attr_accessor :numval # makes it readable and writable

  # Remember parameter names don't have to be the same as the attributes (although they usually are)
  def initialize(thename, num_with_default = 42)
    @name = thename
    @numval = num_with_default
  end

  def methodname(somenum)
    @@count += somenum
  end

  def self.cound
    @@count
  end
end

a = ClassName.new("firstone")
a.name # firstone
a.numval # 42
b = ClassName.new("secondone", 23)
b.numval # 23
```

## Errors

### Remember the arguments!!!

```ruby
class Wog
  attr_accessor :name, :weight
  def initialize(name, weight)
    @name = name
    @weight = weight
  end
end

bink = Wog.new
```

*wrong number of arguments (given 0, expected 2) (ArgumentError)*

This always means you didn't send the right number of arguments. Look at the method definition, and add arguments.

### You're calling methods on obect instances!!!

```ruby
class Wog
  attr_accessor :name, :weight, :owner
  def initialize(name, weight)
    @name = name
    @weight = weight
    @owner = nil
  end

  def adoptMe(owner)
    self.owner = owner
    adoptWog(self)
  end
end

class Owner
  attr_accessor :name
  def initialize(name)
    @name = name
    @wogs = []
  end

  def adoptWog(wog)
    @wogs << wog
  end
end
```

*undefined method `adoptWog' for #<Wog:0x00007fd60094e368> (NoMethodError)*

In `adoptMe`, we're not calling `adoptWog` on the owner. This is very important. It should look like this.

```ruby
  def adoptMe(owner)
    self.owner = owner
    owner.adoptWog(self)
  end
```

## Blocks & Procs

### Code Blocks

_Blocks are not objects_ A block is just a bit of code between do..end or {}. It's not an object on its own, but it can be passed to methods like .each or .select.

```Ruby
def yield_name(name)
  yield("Kim") # print "My name is Kim. "
  yield(name) # print "My name is Eric. "
end

yield_name("Eric") { |n| print "My name is #{n}. " } # My name is Kim. My name is Eric.
yield_name("Peter") { |n| print "My name is #{n}. " } # My name is Kim. My name is Eric. My name is Kim. My name is Peter.
```

### Proc

_saves blocks and are objects_ A proc is a saved block we can use over and over.

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

Diff between blocs and lambdas:

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

## Comment

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

### IF

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

### unless

```Ruby
unless false # unless checks if the statement is false (opposite to if).
puts “I’m here”
else
puts “not here”
end
# or
puts "not printed" unless true
```

### case

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

### While loop:

```Ruby
i = 1
while i < 11
  puts i
  i = i + 1
end
```

### Until loop:

```Ruby
i = 0
until i == 6
  puts i
  i += 1
end
```

### .each

```Ruby
things.each do |item| # for each things in things do something while storing that things in the variable item
  print “#{item}"
end
```

on hashes like so:

```Ruby
hashes.each do |x,y|
  print "#{x}: #{y}"
end
```

## Usefull Methods

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
