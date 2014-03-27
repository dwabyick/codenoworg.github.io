---
layout: page
title: Rock, Paper, Scissors
sidebar: true
---

In this project, students will build a procedural version of Rock, Paper, Scissors.

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

Have the students create a new file called `rps.rb` in a codenow directory.

{% terminal %}
touch rps.rb
{% endterminal %}

To open the file inside SublimeText, we can simply type the filename into the command prompt. If it has not already been set as the default text editor for ruby files, make sure you set it.

{% terminal %}
rps.rb
{% endterminal %}

### Step 1 - Writing pseudocode

Have the students try writing the pseudocode for Rock, Paper, Scissors. If they get stuck, have them look at the Hi-Lo game for hints.

Work together to get to the following pseudocode:

```ruby
# Computer needs to pick rock, paper, or scissors

# Tell the player to pick rock, paper, or scissors

# Output whether the player won, lost, or tied
```

Work with the students to generate pseudocode for the win, loss, tie scenarios. If they get stuck, show them what the first if statement might look like.

For the first iteration of the game, this is roughly what the pseudocode should look like:

```ruby
# Computer needs to pick rock, paper, or scissors

# Tell the player to pick rock, paper, or scissors

# Get the player's move

# Output whether the player won, lost, or tied

  # If the player chose rock and the computer chose rock, print a tie message

  # Otherwise, if the player chose rock and the computer chose scissors, print a win message

  # Otherwise, if the player chose rock and the computer chose paper, print a lose message

  # Otherwise, if the player chose paper and the computer chose paper, print a tie message

  # Otherwise, if the player chose paper and the computer chose rock, print a win message

  # Otherwise, if the player chose paper and the computer chose scissors, print a lose message

  # Otherwise, if the player chose scissors and the computer chose scissors, print a tie message

  # Otherwise, if the player chose scissors and the computer chose paper, print a win message

  # Otherwise, if the player chose scissors and the computer chose rock, print a lose message

```

Ask the students how to run this program. Ask them what happens if you run the program in it's current state.

### Step 2 - Setting computer moves

Review the concept of setting variables. Explain that the right hand side is evaluated before it is saved into the variable. You might use the following example to prove that evaluation happens before variable assignment:

{% irb %}
$ y = 10
$ => 10
$ x = 2 * y
$ => 20
$ x
$ => 20
{% endirb %}

Return to the `rps.rb` file and work together to define a variable to store a "computer move." Once the variable is defined, start off by setting the computer move to a fixed value.

```ruby
# Computer needs to pick rock, paper, or scissors
computer_move = "rock"
```

### Step 3 - Getting player input

Challenge the students to fill in the next two steps by themselves. It should look like this:
```ruby
# Tell the player to pick rock, paper, or scissors
puts "Please enter a move: rock, paper, or scissors"
# Get the player's move
player_move = gets.chomp
```

If the students get stuck, have them open up hilo.rb and look at how they got the player input. Ask them why they don't need to call to_i on the player_move variable. Have the students run their programs to ensure that their code is in a working state. 

### Step 4 - Conditional statement

The conditions in Rock, Paper, Scissors is slightly more complicated than the students covered in Hi-Lo. There are two major concepts that we'll want to cover:

1. Touch on if/elsif/else statements. Make sure they know that the if/elsif/else statements will operate sequentially and will exit the if/elsif/else as soon as one condition is satisified.
2. Explain && and || and how that plays into forming more complex conditions

Ask the class how many conditions you will need to have for the Rock, Paper, Scissors game. Work together to come up with the 9 conditions. The code should look something like this:

```ruby
# If the player chose rock and the computer chose rock, print a tie message
if player_move == "rock" && computer_move == "rock"
  tie_message

# Otherwise, if the player chose rock and the computer chose scissors, print a win message
elsif player_move == "rock" && computer_move == "scissors"
  puts win_message

# Otherwise, if the player chose rock and the computer chose paper, print a lose message
elsif player_move == "rock" && computer_move == "paper"
  puts lose_message

# Otherwise, if the player chose paper and the computer chose paper, print a tie message
elsif player_move == "paper" && computer_move == "paper"
  tie_message

# Otherwise, if the player chose paper and the computer chose rock, print a win message
elsif player_move == "paper" && computer_move == "rock"
  puts win_message

# Otherwise, if the player chose paper and the computer chose scissors, print a lose message
elsif player_move == "paper" && computer_move == "scissors"
  puts lose_message

# Otherwise, if the player chose scissors and the computer chose scissors, print a tie message
elsif player_move == "scissors" && computer_move == "scissors"
  tie_message

# Otherwise, if the player chose scissors and the computer chose paper, print a win message
elsif player_move == "scissors" && computer_move == "paper"
  puts win_message

# Otherwise, if the player chose scissors and the computer chose rock, print a lose message
elsif player_move == "scissors" && computer_move == "rock"
  puts lose_message
end
```
Have students try to play their game.

Reiterate that because we used gets, the value stored inside player_move is a string. Unlike the Hi-Lo game, we dont' receive the same ArgumentError because strings can be compared with other strings.

Have the students run their program to confirm this works and test all 9 cases. In order to do this, they will have to change the computer_move variable.

## Iteration 1: Adding randomness

We are going to use the `sample` method on the `Array` class in order to add randomness to the computer_move variable. First, we'll need to cover the concept of an array. The main concepts that you'll want to mention:

1. An Array holds a collection of items, the types of these items are not important
2. Order is important
3. Elements can be accessed by using the bracket operators `[n]`, where n is the index.
4. The array is zero-indexed
5. Every array has convenience methods on it, such as `first`, `last`, `length`

{% irb %}
$ zoo = [1, "zebra", 2]
$ => [1, "zebra", 2]
$ zoo = ["elephant", "zebra", "camel"]
$ => ["elephant", "zebra", "camel"]
$ zoo[0]
$ => "elephant"
$ zoo[2]
$ => "camel"
$ zoo.first
$ => "elephant"
$ => zoo.sample
$ => "zebra"
{% endirb %}

Have students open up IRB and have them experiment with arrays. Have them generate their own arrays of animals and have them save it in a variable called `zoo`. Have them call `sample` on the zoo array. Challenge them to think of how they could get a random animal using the `rand(n)` method we used in the Hi-Lo game.

{% irb %}
$ zoo[rand(3)]
$ => "camel"
{% endirb %}

In order to randomly generate a computer move, we'll have to first define an array of possible moves. Work with the students to come up with an array and sample the array to pick a computer move.

```ruby
# Computer needs to pick rock, paper, or scissors
possible_moves = ["rock", "paper", "scissors"]
computer_move = possible_moves.sample
```

Have students play their game and prove that computer_move is no longer fixed. Since the computer's move is random, students should have difficulty proving all nine cases. Show them how they can debug their programs by printing out computer_move using `puts`.

Ask them how we should make the game more fun. The typical response is that they'd like to be able to make more than one move.

## Iteration 2: Looping the program

Review the begin/end/while syntax and ask the students to try to make it so that we can play Rock, Paper, Scissors indefinitely.

If the students get stuck, work together to use the begin/end/while syntax to loop their game. Start off by identifying which portion of the program they want to repeat. Then discuss what the condition should look like in order to satisfy our requirement. The loop should look like this:

```ruby
begin
  # Computer needs to pick rock, paper, or scissors
  possible_moves = ["rock", "paper", "scissors"]
  computer_move = possible_moves.sample

  # Tell the player to pick rock, paper, or scissors
  puts "Please enter a move: #{possible_moves.join(", ")}"

  # Get the player's move
  player_move = gets.chomp

  # Output whether the player won, lost, or tied

  # If the player chose rock and the computer chose rock, print a tie message
  if player_move == "rock" && computer_move == "rock"
    puts "#{computer_move} is equal to #{player_move}. Oh no, it's a tie!"

  # Otherwise, if the player chose rock and the computer chose scissors, print a win message
  elsif player_move == "rock" && computer_move == "scissors"
    puts "#{player_move} trumps #{computer_move}. You win!"

  # Otherwise, if the player chose rock and the computer chose paper, print a lose message
  elsif player_move == "rock" && computer_move == "paper"
    puts "#{computer_move} trumps #{player_move}. Computer wins!"

  # Otherwise, if the player chose paper and the computer chose paper, print a tie message
  elsif player_move == "paper" && computer_move == "paper"
    puts "#{computer_move} is equal to #{player_move}. Oh no, it's a tie!"

  # Otherwise, if the player chose paper and the computer chose rock, print a win message
  elsif player_move == "paper" && computer_move == "rock"
    puts "#{player_move} trumps #{computer_move}. You win!"

  # Otherwise, if the player chose paper and the computer chose scissors, print a lose message
  elsif player_move == "paper" && computer_move == "scissors"
    puts "#{computer_move} trumps #{player_move}. Computer wins!"

  # Otherwise, if the player chose scissors and the computer chose scissors, print a tie message
  elsif player_move == "scissors" && computer_move == "scissors"
    puts "#{computer_move} is equal to #{player_move}. Oh no, it's a tie!"

  # Otherwise, if the player chose scissors and the computer chose paper, print a win message
  elsif player_move == "scissors" && computer_move == "paper"
    puts "#{player_move} trumps #{computer_move}. You win!"

  # Otherwise, if the player chose scissors and the computer chose rock, print a lose message
  elsif player_move == "scissors" && computer_move == "rock"
    puts "#{computer_move} trumps #{player_move}. Computer wins!"
  end
end while true
```

## Iteration 3: Refactoring

The code we wrote is a little messy and there are some nice refactorings that can be done. Explain the concept of refactoring and why it is useful to clean up code.

The first refactor we'd like to accomplish is modifying the program to only use one if statement for the three tie scenarios. Challenge the students to write this refactor by themselves. Give them the hint to compare that we'll need to compare the player's move to the computer's move.

```ruby
# If the player_move is the same as the computer_move, print a tie message
if player_move == computer_move
  puts "#{computer_move} is equal to #{player_move}. Oh no, it's a tie!"
```

The second refactor we'd like to accomplish is modifying the program to save the messages into variables.

```ruby
# Save win, lose, and tie messages in variables
win_message = "#{player_move} beats #{computer_move}. You win!"
lose_message = "#{computer_move} beats #{player_move}. Computer wins!"
tie_message = "#{computer_move} is equal to #{player_move}. Oh no, it's a tie!"

# If the player_move is the same as the computer_move, print a tie message
if player_move == computer_move
  puts "#{tie_message}"
elsif ...
```
## Iteration 4: Extensions

If students finish early, let them choose one of the following extensions:

1. Modify the program so that instead of running forever, the game ends after three rounds.
1. Modify the program (without changing the logic), such that the computer always wins
2. Modify the program such that it does not accept invalid inputs
3. Modify the program such that it keeps track of how many times you have won and lost.
4. Modify the program such that you can type 'quit' and the program stops.

A completed version of the program might look as follows:

```ruby
tries = 0

begin
  #increment tries
  tries = tries + 1

  # Computer needs to pick rock, paper, or scissors
  possible_moves = ["rock", "paper", "scissors"]
  computer_move = possible_moves.sample

  # Tell the player to pick rock, paper, or scissors
  puts "Please enter a move: #{possible_moves.join(", ")}"

  # Get the player's move
  player_move = gets.chomp

  # Save win, lose, and tie messages in variables
  win_message = "#{player_move} beats #{computer_move}. You win!"
  lose_message = "#{computer_move} beats #{player_move}. Computer wins!"
  tie_message = "#{computer_move} is equal to #{player_move}. Oh no, it's a tie!"

  # Output whether the player won, lost, or tied

  # If the player_move is the same as the computer_move, print a tie message
  if player_move == computer_move
    puts "#{tie_message}"

  # Otherwise, if the player chose rock and the computer chose scissors, print a win message
  elsif player_move == "rock" && computer_move == "scissors"
    puts "#{win_message}"

  # Otherwise, if the player chose rock and the computer chose paper, print a lose message
  elsif player_move == "rock" && computer_move == "paper"
    puts "#{lose_message}"

  # Otherwise, if the player chose paper and the computer chose rock, print a win message
  elsif player_move == "paper" && computer_move == "rock"
    puts "#{win_message}"

  # Otherwise, if the player chose paper and the computer chose scissors, print a lose message
  elsif player_move == "paper" && computer_move == "scissors"
    puts "#{lose_message}"

  # Otherwise, if the player chose scissors and the computer chose paper, print a win message
  elsif player_move == "scissors" && computer_move == "paper"
    puts "#{win_message}"

  # Otherwise, if the player chose scissors and the computer chose rock, print a lose message
  elsif player_move == "scissors" && computer_move == "rock"
    puts "#{lose_message}"
  end
end while tries < 3 && player_move != "quit"
```