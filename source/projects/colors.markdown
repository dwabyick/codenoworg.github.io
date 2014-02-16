---
layout: page
title: Colors
sidebar: true
---

In this project, we are going to write Ruby code to play with colors in our terminal. It's a neat way for the students to practice key Ruby concepts while breaking the boring black and white monotony of the terminal. 

What's great about this is that once the students know how to use this gem, they can use it in all of their past and future ruby programs (rock paper scissors, twitterbot, etc.) to make the output colored and less boring.

This project is intended for beginners; however, it shouldn't be their first project, as it requires understanding a few programming tricks that would be hard to understand if one weren't comfortable with basics such as variables.

Learning goals:

* Understand what gems are
* Review basic ruby methods (puts, gets, sleep)
* Play with iteration
* Understanding how to use the `%` operator
* A soft intro to arrays

{% img center /images/projects/colors/colors.png %}

##Iteration 0: Setup

There are a few things we need to do. First, we need to install the `colorize` and the `win32console` gems:

{% terminal %}
$ gem install win32console colorize
{% endterminal %}

Explain to the students what gems are: a way for programmers to share the code they've written in a way that allows other programmers to reuse them. There are gems for everything: doing complicated math operations, writing games, writing websites, etc. The great thing about gems is that they're free to use and anyone can make them (yes - even them when they're a bit more advanced with Ruby!)

Let's create a `colors.rb` file, and `require` our gems in it. It tells Ruby we want to use those gems:

```ruby
require 'colorize'
require 'win32console'
```

Let's run the program! Of course, nothing happens. All we have done so far is tell Ruby we want the possibility to use those gems, but we haven't actually used them. We still want to run the program though - if the gems aren't installed, we'll get an error.

Let's now add the following:

```ruby
puts "CodeNow".colorize(:blue)
```

And now let's run it! It should be evident what this does. colorize added a "colorize" method to strings, which makes them appeared colored in the terminal.

Ask the students if they recognize what `:blue` is. They've never seen it before, so a little explanation is required: they're called symbols, and are a bit like strings- except they're immutable. This means they can't be changed.

With a string, we can do:

```ruby
codenow = "codenow"
codenow[4] = "w"
puts codenow #this will print "codewow"
```

This can't be done with a symbol:

```ruby
codenow = :codenow
codenow[4] = "w"
#this will lead to...
NoMethodError: undefined method `[]=' for :codenow:Symbol
    from (irb):9
    from /usr/bin/irb:12:in `<main>'
```

Symbols also can't have spaces; instead, programmers use underscores.

```ruby
:hello codenow
#this will lead to...
SyntaxError: (irb):12: syntax error, unexpected tIDENTIFIER, expecting end-of-input
    from /usr/bin/irb:12:in `<main>'
:hello_codenow #this works!
```

##Iteration 1: Fun with colors

We're now going to write a basic program that prints a word typed by the user in a color specified by the user. Our program should ask for a word, and then a color.

```ruby
puts "What word should we print?"
word = gets.chomp

puts "What color should it be printed in?"
color = gets.chomp
```

We now want to display it. Ask the students how they'd do it! They're going to guess something like:

```ruby
puts word.colorize(color)
```

Unfortunately, this won't work: color is a string, and we need a symbol. The key is to convert it to a symbol, which can be done with the `.to_sym` (short for "to symbol") method:

```ruby
puts word.colorize(color.to_sym)
```

{% img center /images/projects/colors/step1.png %}

##Iteration 2: Color cycling

###Step 1

Now we're going to have a bit of fun with loops and colors. Our first program will ask the user for text, and then print that text in several different colors.

{% img center /images/projects/colors/step2a.png %}

To get started, we want an array with the colors we'll cycle through:

```ruby
colors = [:green, :yellow, :blue, :red]
```
Next, we ask the user which word they want to print:

```ruby
puts "What word do you want to print?"
word = gets.chomp
```
Finally, we want to iterate over our colors, and print the word in that color:

```ruby
colors.each do |color|
	puts word.colorize(color)
end
```

###Step 2
Now for something a bit different: again, we're going to ask the user for some text, and print that text colored. This time however, we are going to alternate the colors for each letter.

{% img center /images/projects/colors/step2b.png %}

The beginning of this program is the same: we want to initialize an array of colors, and ask the user for text to print.

```ruby
puts "What do you want to print?"
word = gets.chomp

colors = [:green, :yellow, :blue, :red]
```
Since we want to print each character in a different color, we are going to iterate over the characters and print them one by one. Note that we use `print`, rather of `puts`; print doesn't insert a newline, whereas `puts` does (have the students use both to see the difference for themselves).

```ruby
word.each_char do |letter|
   print letter
end
```

Running this prints our text on the screen, albeit in boring white- we need to add color! How can we do this?

Walk with the students over what we want to do.

* What should be the color of the 0th letter? (remind them that in programming, we always start at 0)
* What should be the color of the 1st letter? 2nd letter?
* What is `colors[0]` ? `colors[1]`?

We're going to do this by adding a counter, so we can keep track of how many letters we have printed and apply the appropriate color to the current letter.

```ruby
letters_printed = 0

word.each_char do |letter|
   print letter
   
   letters_printed = letters_printed + 1
end
```

We're almost there! The only remaining problem is that we can't just do this:

```ruby
letters_printed = 0

word.each_char do |letter|
   color = colors[letters_printed]
   print letter.colorize(color)
   letters_printed = letters_printed + 1
end
```

Because if we only have 4 colors in the array, `colors[4]` won't work. How do we solve this problem?

Enter the modulo! (`%`)
If the students haven't done a project that uses modulo yet, introduce them to it. The modulo operation is often used in programming, when we want to "wrap" a value . For example, games often use the modulo for angles - turning by 500 degrees is like turning by 140 degrees because `500 % 360 = 140`. So when working with angles, we "wrap" them to 360 degrees because that's how many degrees there are in a full circle.

Here, we want to "wrap" our color by how many colors there are in our `colors` array; in other words, `colors.length`.

We can now finish our program by changing

```ruby
color = colors[letters_printed]
```

to

```ruby
color = colors[ letters_printed % colors.length ]
```

##Iteration 3: Color Reading Game

We're going to write a fun Ruby program to illustrate the ["Stroop Effect"](http://en.wikipedia.org/wiki/Stroop_effect). It's the effect that makes it hard to quickly say which color a word is if the word is of a different color. For example:

{% img center /images/projects/colors/stroop.png %}

Our program will:

1. print a random word in a random color
2. wait for 1 second
3. clear the screen and repeat.

The first step is to setup an array with all of the colors we'll pick from:

```ruby
colors = ["red", "blue", "green", "yellow", "white"]
```

The second step is to make an infinite loop - i.e., our program will only stop if the user presses Ctrl+c. The way we achieve this in Ruby is by having a condition that's always true:

```ruby
begin

end while 1==1
```

Of course, there are many ways one can write the condition; this is just an example (we could also just write `end while true`).

Now that we have our infinite loop, we need to do two things:

* pick a random color for the word
* pick a random color for the word's color

We're going to use the array's .sample method. Give the students a quick primer on it if needed.
We can then do:

```ruby
word = colors.sample
word_color = colors.sample
```

We now have two random colors; we now need to print it. Nothing groundbreaking here- just don't forget the .to_sym:

```ruby
begin
    puts word.colorize( word_color.to_sym )
end while 1==1
```

This works, but it goes really fast. What should we do?
Let's make our program wait for 1 second between each color, using `sleep`:

```ruby
begin
    puts word.colorize( word_color.to_sym )

    sleep(1)
end while 1==1
```

Almost there! The last thing we want is to clear the screen between each color. On Windows, we do this with `system("cls")` (on OSX/Linux, we'd use `system("clear")`).

```ruby
begin
    puts word.colorize( word_color.to_sym )

    sleep(1)
    system("cls")
end while 1==1
```

And voilà! We're done. Behold our wonderful program:

{% img center /images/projects/colors/stroop.gif %}

##Improvements

A few ideas:

1. Make the latest program a game where the colored word flashes for 1 second, then asks the user to type the color of the word.
2. On top of the previous, make it so that the player loses when he guesses wrong. The goal is to get the longest streak possible! (of course, the program should keep count of the user's current streak)
3. On top of the previous, make the word flash faster the higher the player's score is!

##Annex: available colors

Here are all the colors we can use with `colorize`:

```ruby
:black
:red
:green
:yellow
:blue
:magenta
:cyan
:white
:default
:light_black
:light_red
:light_green
:light_yellow
:light_blue
:light_magenta
:light_cyan
:light_white
```

There are also a couple other things the gem offers that we can use:

```ruby
string = "CodeNow!"

puts string.underline #underline the string
puts string.colorize(:green).on_red #green text on a red background
```
