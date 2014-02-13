---
layout: page
title: Calculator
sidebar: true
---

In this project, students will build a very basic terminal calculator.

The program asks for a first number, a second number, and an operation. It then displays the result of the operation.

Learning Goals:

* Practice basic Ruby methods and concepts (gets, puts, string interpolation, variables, etc.)
* Basic conditionals

{% img center /images/projects/calculator/calculator.png %}

##Iteration 0 : Setup

Let's create a calculator.rb file.

{% terminal %}
touch calculator.rb
{% endterminal %}

Now we want to tell the user

* to enter a first number (and save it)
* to enter a second number (and save it)
* to enter the operation to perform (and save it)

Here's what the students should come up with:

```ruby
puts "First number?"
first_number = gets.chomp.to_i
 
puts "Second number?"
second_number = gets.chomp.to_i
 
puts "Operation?"
operation = gets.chomp
```

The `.to_i` is important here, as we can't sum strings! Use this opportunity to drive home the concept of types to students.

Make the students test their program, by using string interpolation:

```ruby
puts "You entered the first number #{first_number}"
puts "You entered the second number #{second_number}"
puts "You entered the operation #{operation}"
```

##Iteration 1: Conditionals

The next step is to actually perform the operation. Let's start with addition (when operator == '+'). We first need to write our if block:

```ruby
if operation == '+'

end
```

We then want to fill it out:

```ruby
if operation == '+'
    puts "#{first_number} + #{second_number} = "
    puts "#{ first_number + second_number }"
end
```

Regarding the second line, we can either print the result by storing it in a variable and then printing that variable, or doing the sum directly in the interpolation. Both methods are valid, and students should be aware of both!

We then want to do the same for other operations. This should be fairly straightforward for the students, and will look like this:

```ruby
if operation == '+'
    puts "#{first_number} + #{second_number} = "
    puts "#{ first_number + second_number }"
elsif operation == '-'
    puts "#{first_number} - #{second_number} = "
    puts "#{ first_number - second_number }"
elsif operation == '*'
    puts "#{first_number} * #{second_number} = "
    puts "#{ first_number * second_number }"
elsif operation == '/'
    puts "#{first_number} / #{second_number} = "
    puts "#{ first_number / second_number }"
end
```

##Iteration 2: Refactoring

Finally, we're going to introduce the students to the concept of refactoring; that is, modifying code we just wrote to make it more concise and clean. Ask them if they see anything that could be "refactored". The key insight here is that the lien printing the operation can be condensed as such:

```ruby
puts "#{first_number} #{operation} #{second_number} = "
```

and put at the top of the if block:

```ruby
puts "#{first_number} #{operation} #{second_number} = "
if operation == '+'
    puts "#{ first_number + second_number }"
elsif operation == '-'
    puts "#{ first_number - second_number }"
elsif operation == '*'
    puts "#{ first_number * second_number }"
elsif operation == '/'
    puts "#{ first_number / second_number }"
end
```

##Improvements

If the students finish early, here are a few things that can be done to improve the program:

1. Give an error message if the user doesn't type a proper operation
2. Add operations, for example modulo or exponent.
3. Allow operations to be typed out ('add' would work just like '+') to introduce the "or" in conditionals
4. If the students are familiar with iteration, make the program repeat until "done" is typed for the operation 

##Full code

```ruby
puts "First number?"
first_number = gets.chomp.to_i
 
puts "Second number?"
second_number = gets.chomp.to_i
 
puts "Operation?"
operation = gets.chomp

puts "#{first_number} #{operation} #{second_number} = "
if operation == '+'
    puts "#{ first_number + second_number }"
elsif operation == '-'
    puts "#{ first_number - second_number }"
elsif operation == '*'
    puts "#{ first_number * second_number }"
elsif operation == '/'
    puts "#{ first_number / second_number }"
end
```