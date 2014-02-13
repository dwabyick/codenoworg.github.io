---
layout: page
title: Hangman
sidebar: true
---

In this project, students will learn how to build a basic hangman game in the terminal.

The game starts by asking the player to type a word to guess. Then, another player plays the game by typing a letter. The computer tells him whether the letter is in the word or not, and displays 1) the right letters guessed so far and 2) the wrong letters guessed so far. The game ends after the player runs out of tries or guesses the complete word.

Learning Goals:

* Practice the use of conditionals
* Practice the use of arrays
* Practice the use of iteration
* Review a couple string methods
* A small dip into defining our own methods

While not an extremely advanced project, it uses all those concepts together to make the game happen; make sure your group is comfortable enough with Ruby basics to tackle this project (and it may be worth it to do a little brushup on array methods, iteration and conditionals before hand if needed).

{% img center /images/projects/hangman/hangman.png %}

##Iteration 0: Setting up the program

Let's create a `hangman.rb` file in the CodeNow directory.

{% terminal %}
$ touch hangman.rb
{% endterminal %}

Next, let's write the code that will setup the variables used for our game. We'll prompt the user for the word to guess and store it as a string; we'll also keep track of the number of tries the player has left as an integer, and the letters guessed in an array.

```ruby
puts "==========="
puts "  HANGMAN  "
puts "==========="

#first let's get the word to guess
puts "What's the word to guess?"
word_to_guess = gets.chomp

#The number of tries the player gets.
num_tries = 10
 
#This array will hold the letters
#already guessed.
letters_guessed = []
```
 
We also want to have a string that will represent the state of the word being guessed. Initially, it will be all underscores ("_"); it will get progressively filled out as the player unveils the word. For example, as the word "code" gets guessed:

{% terminal %}
____
c___
c__e
c_de
code
{% endterminal %}

To do this, we need to do two things:
- get the length of the word to guess
- create a string with that many underscores ("_") in it.

If they haven't seen it, show the students the .length method:

```ruby
"codenow".length
```

If they haven't seen it, show the students the * operator on strings:

```ruby
"codenow" * 12
```

With those two tools in hand, we can now generate the string we want:

```ruby
#This string will display the progress of the player
#Hidden letters will be replaced by "_".
word_progress = "_" * word_to_guess.length
```

##Iteration 1: The main loop

The main loop, much like in the hilo or rock-paper-scissors game, will repeat the core game mechanic (asking the player for a letter) until we hit a *losing condition* or a *winning condition*. Work with the students to figure out what those are: either running out of tries (lose), or guessing the word (win).

The first one is easy:

```ruby
begin

end while num_tries > 0
```

The second one can be achieved in several ways; use this opportunity to show that there is never just one right answer in programming. Our preferred way is to check whether there are any underscores in `word_progress`:

```ruby
begin

end while num_tries > 0 and word_progress.include? '_'
```

Two things here:

* Remind/introduce the students to the fairly straightforward `include?` method on strings
* Work with the students to make sure they understand why we're using the `and` operator (and not `or`, for instance)

Now let's put the basic stuff in our game loop:

* Showing the player their progress so far
* Telling the player how many tries they have left
* Show the letters guessed so far
* Ask for a letter
* Add the letter guessed to the array of letters guessed.

This is all basic stuff that the students should be comfortable with (they might need a little refresher on `<<` ):

```ruby
#Our main loop!
begin
 
#Let's show the player their progress and how many
#tries they have left
puts "Word progress so far: #{word_progress}"
puts "#{num_tries} tries left."
puts "Letters guessed so far: #{letters_guessed}"
 
#Get their guess
puts "Enter a letter:"
letter = gets.chomp

#add the letter just guessed to the list of letters guessed.
letters_guessed << letter

end while num_tries > 0 and word_progress.include? '_'
```

##Iteration 2: Handling player turns
This is the core of the hangman game: determining whether to remove a try from the player (if he guessed the letter wrong), or updating the word (if he guessed the letter right).

###Step 1

First thing first, let's write our full conditional block. `include?` is our friend once again:

```ruby
if word_to_guess.include? letter
    #congratulate them
    puts "Good guess!"
else
    #display a sad message
    puts "Bummer! The letter #{letter} isn't in the word."
end
```

###Step 2

Alright, now we need to fill out the missing code. Let's do the losing condition first, as it is shorter. We just need to remove a try from the player.

This is all stuff the students have seen before:

```ruby
if word_to_guess.include? letter
    #congratulate them
    puts "Good guess!"
else
    #display a sad message
    puts "Bummer! The letter #{letter} isn't in the word."

    #remove a try.
    num_tries = num_tries - 1
end
```

###Step 3a
Now we need to fill out what happens if the player guesses the right letter. We need to replace each occurence of a "_" in word_progress with the letter in every place that the letter occurs.

The first way we're going to do this is by using `String`'s `index` method to find where a letter is in a word.

```ruby
"codenow".index('w') => 6
```

We can store the index at which a character occurs, and then use the brackets to modify that same index in our word_progress string.

```ruby
if word_to_guess.include? letter
    #congratulate them
    puts "Good guess!"

    index = word_to_guess.index(letter)
    word_progress[index] = letter
else
    #display a sad message
    puts "Bummer! The letter #{letter} isn't in the word."

    #remove a try.
    num_tries = num_tries - 1
end
```

Have the students run the program and try it. Ask them if they see any problems with it.

###Step 3b

It should be pretty apparent: our program sort of works, but fails when a letter repeats in a word. This is because with `.index` we only get the first occurence of a letter:

```ruby
"codenow".index('o') => 1
```

Ideally, we would want a method on string that gives us a list of all the occurences in the word:

```ruby
"codenow".index('o') => [1, 5]
```

Sadly, such a method does not exist. But we're going to create it!
Let's write the boilerplate code first:

```ruby
class String
    def occurences(letter)

    end
end
```

Explain in a high level fashion to the student that the `class String` around the method is used to tell Ruby that this method should be available for `String` objects. Our method takes a single argument: a letter.

How are we going to write the method? This is a little advanced (but no new concept is used!); depending on the level of your group of students and the time you have left, either help them figure it out on their own or walk them through it. Ultimately, we want something like that:

```ruby
class String
    def occurences(search_letter)
        occurences = []
        idx = 0

        self.each_char do |letter|
            if search_letter == letter
                occurences << idx
            end
            idx += 1
        end

        return occurences
    end
end
```

Great, we now have our `occurences` method! We can now use it to get an array of indexes for all occurences of the correctly guessed letter in the word to guess. We iterate over that array and replace letters the same way we replaced them before our occurences method.

This is what we'll have in our "right guess" block:

```ruby
if word_to_guess.include? letter
    #congratulate them
    puts "Good guess!"

    #update the word progress
    indexes = word_to_guess.occurences(letter)
     
    indexes.each do |index|
        word_progress[index] = letter
    end
else
    [...]
end
```

##Iteration 3: Endgame

The core of our game is programmed; now we just need to display a message depending on whether the player has won or lost.

First, let's display a message indicating that the game has ended.

```ruby
puts "END OF THE GAME!"
```

Then we display a message for the losing condition, and a message for the winning condition. No new material here - we just have to decide which condition we're going to use. Again, there are several ways we can do this; here is a proposed one:

```ruby
#At the end of the game, display a sad message if they ran out of tries
if num_tries == 0
    puts "Oh no! You ran out of tries :("
    puts "The word was #{word_to_guess}"
#Or display a happy message if they guessed the word.
else
    puts "Sweet! You guessed the word #{word_to_guess}!"
end
```

Of course, we could instead test for the presence of "_" in the guessed word to determine if the player has won or not, for example.


##Improvements

Here are some extensions the students can work on if they finish early or want to spend more time on their project:

1. Not allowing the player to play a letter already guessed
2. Not allowing the player to type something invalid (for example a number or a full word)
3. Displaying a real hangman ASCII drawing depending on how close the player is to being out of tries
4. Allowing the player to set the difficulty by choosing how many tries they get.
5. Making the difficulty automatic based on the number of letters in the word. (let the students think it through: what should be the proportion between number of tries, and length of the word?)
6. Instead of asking the user for a word, load it randomly from an array of predefined words.
7. If you have extra time, expound on 6. by showing the students how to read from a file, and load a random word from a dictionary file.

##Finished code

```ruby
class String
    def occurences(search_letter)
        occurences = []
        idx = 0

        self.each_char do |letter|
            if search_letter == letter
                occurences << idx
            end
            idx += 1
        end

        return occurences
    end
end

puts "==========="
puts "  HANGMAN  "
puts "==========="

#first let's get the word to guess
puts "What's the word to guess?"
word_to_guess = gets.chomp

#The number of tries the player gets.
num_tries = 10

#This array will hold the letters
#already guessed.
letters_guessed = []

#This string will display the progress of the player
#Hidden letters will be replaced by "_".
word_progress = "_" * word_to_guess.length

#Our main loop!
begin

#Let's show the player their progress and how many
#tries they have left
puts "Word progress so far: #{word_progress}"
puts "#{num_tries} tries left."
puts "Letters guessed so far: #{letters_guessed}"

#Get their guess
puts "Enter a letter:"
letter = gets.chomp

#add the letter just guessed to the list of letters guessed.
letters_guessed << letter

#if the letter is in the word...
if word_to_guess.include? letter
    #congratulate them
    puts "Good guess!"

    #update the word progress
    indexes = word_to_guess.occurences(letter)
    
    indexes.each do |index|
        word_progress[index] = letter
    end

#if the letter isn't the word...
else
    #display a sad message
    puts "Bummer! The letter #{letter} isn't in the word."
    #remove a try.
    num_tries = num_tries - 1
end

#keep going until there are no tries left or the word isn't fully guessed
end while num_tries > 0 and word_progress.include? '_'

puts "END OF THE GAME!"

#At the end of the game, display a sad message if they ran out of tries
if num_tries == 0
    puts "Oh no! You ran out of tries :("
    puts "The word was #{word_to_guess}"
#Or display a happy message if they guessed the word.
else
    puts "Sweet! You guessed the word #{word_to_guess}!"
end
```