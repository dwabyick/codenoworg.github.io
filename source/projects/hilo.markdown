---
layout: page
title: Hi-Lo Guessing Game
sidebar: true
---

In this project, students will build a procedural version of the Hi-Lo Guessing Game.

Learning Goals:

* Practice the fundamentals of Ruby including setting variables, getting user input, printing messages
* Understand control flow and conditional branching
* Get exposure to while loops

<div class="note">
<p>The design decisions in the following code are intentional to help absolute beginners quickly learn the basics without overwhelming them with intermediate concepts.</p>
</div>

<div class="note">
<p>This tutorial is open source. If you notice errors, typos, or have questions/suggestions, please <a href="https://github.com/CodeNowOrg/codenoworg.github.io">submit them to the project on Github</a>.</p>
</div>

## Iteration 0: Up & running

Have the students create a new file called `hilo.rb` in a codenow directory.

{% terminal %}
touch hilo.rb
{% endterminal %}

To open the file inside SublimeText, we can simply type the filename into the command prompt. If it has not already been set as the default text editor for ruby files, make sure you set it.

{% terminal %}
hilo.rb
{% endterminal %}

### Step 1 - Printing something to the screen

Introduce the students to the `puts` method. Write a simple one-line statement that demonstrates that there is a command called puts and that the string that follows the word `puts` will be printed to the screen.

```ruby
puts "Hello"
```
Teach the students that programs are always run from the command line with `ruby your_program_name.rb`. From your terminal/command prompt, run the program as follows:

{% terminal %}
ruby hilo.rb
{% endterminal %}

### Step 2 - Understanding control flow

Explain that the computer runs code sequentially, starting from the first line of the program and ending at the last line. One way this can be demonstrated is by using a combination of puts and sleep.

```ruby
puts "Hello"
sleep 2
puts "World"
```
Have the students practice running the program and ask them to explain how the program works.

### Step 3 - Writing pseudocode

Work together with the class to first write pseudocode for the Hi-Lo game. Explain the following key concepts: 

1. The number sign indicates that a line is a comment that will not be run by the computer
2. Much like writers create outlines to set up their essay, programmers will often write pseudocode to help set up their code

For the first iteration of the game, this is roughly what the pseudocode should look like:

```ruby
# Computer needs to generate a secret number

# Tell the user to guess a number between 1 and 10

# User guesses a number

# If the guess was higher than the secret number print "Too high!"

# If the guess was lower than the secret number print "Too low!"

# If the guess was equal to the secret number print "Congratulations!"
```
Have the students run the program to prove that the computer does not execute comments.

### Step 4 - Setting variables

Demonstrate how variables work and why they are useful. Explain the following key concepts: 

1. Variables are set using the equals operator (=). The value on the right side of the equals sign, is the value that is stored.
2. Variables look a lot like strings, but without the quotes. The quotes makes a huge difference.
3. Naming variables is important. Intuitive names help us understand code, but they are not required.
4. Once a variable is set, it will store that information until the variable is set to a new value. Write a simple example and ask them what values will be printed to the screen:

```ruby
number = 5
puts "#{number}"
puts "#{number}"
number = 10
puts "#{number}"

```
Return to the `hilo.rb` file and work together to define a variable to store a "secret number." Once the variable is defined, start off by setting the secret number to a fixed value.

```ruby
# Computer needs to generate a secret number
secret_number = 5
```
Challenge the students to fill in the next step by themselves.
```ruby
# Tell the user to guess a number between 1 and 10
puts "Please guess a number!"
```
Have the students run their programs to ensure that their code is in a working state. Frequently, students will forget about quotation marks in the `puts` statement or capitalize `puts`. Stress that capitalization and punctuation are incredibly important in computer programming.

### Step 5 - Getting user input

Have the students call the gets method in their programs, but don't tell them what it does.

```ruby
# User guesses a number
gets
```
Tell the students run their programs and ask if they notice a difference in program behavior. Show them that the computer is waiting for a human response. Explain that the opposite of `puts` is `gets` and that the `s` is short for string.

Next, show them that the value of the gets can be stored inside a variable. Work as a class to choose an appropriate variable name to store the user's guess. As a last step, call `.chomp` on the gets to strip the new line character from the gets. Avoid an in-depth explanation if possible.

```ruby
# User guesses a number
guess = gets.chomp
```

### Step 6 - Conditional statement

Make a detour here to explain conditions and if statements. Start off by explaning what a condition is and show the class how it can be used in an if statement. Have them open IRB and experiment with various conditions.

{% irb %}
$ 5 < 10
$ => true
$ 10 < 5
$ => false
$ x = 5
$ => 5
$ 10 > x
$ => true
$ y = 10
$ => 10
$ y > x
$ => true
$ x == y
$ => false
$ x != y
$ => true
{% endirb %}

Avoid teaching them about else and elsif statements for now. Work with the class to come up with the three if statements needed for Hi-Lo. It should look something like this:

```ruby
# If the guess was higher than the secret number print "Too high!"
if guess > random_number
  puts "Too high!"
end

# If the guess was lower than the secret number print "Too low!"
if guess < random_number
  puts "Too low!"
end

# If the guess was equal to the secret number print "Congratulations!"
if guess == random_number
  puts "Congratulations"
end
```
Have students try to play their game. It shouldn't work. They should receive an ArgumentError, `'>': comparison of String with Fixnum failed (ArgumentError)`

Reiterate that because we used gets, the value stored inside guess is a string. Have them open IRB and try to replicate the error. Teach them about the `to_i` method. Ask them how they how they would change their program.

```ruby
# User guesses a number
guess = gets.chomp
guess = guess.to_i
```

Have the students run their program to confirm this works and test all three cases. Ask them how we should make the game more fun. The typical response is that the secret number should not be fixed.

## Iteration 1: Adding randomness

We are going to use the `rand` method in order to add randomness to the secret_number variable. Have students open up IRB and have them experiment with rand(n). Explain that rand(10) won't generate numbers in the range that we want (rand generates between 0 and n-1, whereas we want to make numbers between 1 and n). Challenge them to think of a solution.

```ruby
# Computer needs to generate a secret number
secret_number = rand(10) + 1
```
Have students play their game and prove that the secret number is no longer fixed. Since the secret number is random, students should have difficulty proving all three cases. Show them how they can debug their programs by printing out the secret number using `puts`.

Ask them how we should make the game more fun. The typical response is that they'd like to be able to make more than one guess.

## Iteration 2: Looping the program

Teach the class about the while loop. Use the begin/end/while syntax and open up a new script to demonstrate how it works.

```ruby
begin
  x = rand(2)
  print "#{x}"
  sleep 1
end while 5 < 10
```
Ask students what they think this program does and walk through the execution line by line. Once they grasp the concept, remove the sleep and show that computers can do repetitive tasks at an extremely high speed.

Work together to use the begin/end/while syntax to loop their game. Start off by identifying which portion of the program they want to repeat. Then discuss what the condition should look like in order to satisfy the Hi-Lo game. The loop should look like this:

```ruby
begin
  tries = tries + 1

  # Tell the user to guess a number between 1 and 10
  puts "Guess a number between 1 and #{range_limit}"

  # Let the user guess a number
  guess = gets.chomp.to_i

  # If the guess was higher than the secret number print "Too high!"
  if guess > secret_number
    puts "Too high!"
  end

  # If the guess was lower than the secret number print "Too low!"
  if guess < secret_number
    puts "Too low!"
  end

  # If the guess was equal to the secret number print "Congratulations!"
  if guess == secret_number
    puts "Congratulations"
  end
end while guess != secret_number
```

## Iteration 3: Being the product manager

Ask students what other features they think the game should have. Some features we've seen:
- Limiting the number of guesses (think of having 3 lives in a video game)
- Printing out a "You Lose!" message
- Dynamically setting the range of random numbers, so that the upper limit is not always 10

Depending on how much time is left, pick one or two features and work with the class to implement it. This is how the final program might look.

```ruby
# Get the upper limit from the user
puts "Please enter the upper bound for your limit."
upper_limit = gets.chomp.to_i

# Get the maximum number of guesses from the user
puts "How many tries do you want to allow?"
max_tries = gets.chomp.to_i

# Initialize tries variable to 0
tries = 0

# Computer needs to generate a secret number
secret_number = rand(upper_limit) + 1

begin
  tries = tries + 1

  # Tell the user to guess a number between 1 and 10
  puts "Guess a number between 1 and #{upper_limit}"

  # Let the user guess a number
  guess = gets.chomp.to_i

  # If the guess was higher than the secret number print "Too high!"
  if guess > secret_number
    puts "Too high!"
  end

  # If the guess was lower than the secret number print "Too low!"
  if guess < secret_number
    puts "Too low!"
  end

  # If the guess was equal to the secret number print "Congratulations!"
  if guess == secret_number
    puts "Congratulations"
  end
end while guess != secret_number and tries != max_tries

if tries == max_tries and guess != secret_number
  puts "You lose!"
end
```
