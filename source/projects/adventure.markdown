---
layout: page
title: Adventure
sidebar: true
---

In this project, we are going to build a basic text adventure game, akin to games from the 70s such as Hunt the Wumpus, Rogue, etc.

Learning Goals:

* More advanced Ruby concepts: 2-dimensional arrays, global variables.
* Understand how a "real world" program is broken down into methods
* Show the students what they are capable of with everything they've learned so far!

##Iteration 0:  Setup

Let's a create a adventure.rb file.

{%  terminal %}
touch adventure.rb
{% endterminal %}

When first launched, our program will display a title and ask the user if they want instructions. If the user types Y, we display the instructions; otherwise, we don't.

The code here is very straightforward and has no new elements:

```ruby
puts "
                                            WELCOME TO THE WORLD OF
   ▄████████ ████████▄   ▄█    █▄     ▄████████ ███▄▄▄▄       ███     ███    █▄     ▄████████    ▄████████
  ███    ███ ███   ▀███ ███    ███   ███    ███ ███▀▀▀██▄ ▀█████████▄ ███    ███   ███    ███   ███    ███
  ███    ███ ███    ███ ███    ███   ███    █▀  ███   ███    ▀███▀▀██ ███    ███   ███    ███   ███    █▀
  ███    ███ ███    ███ ███    ███  ▄███▄▄▄     ███   ███     ███   ▀ ███    ███  ▄███▄▄▄▄██▀  ▄███▄▄▄
▀███████████ ███    ███ ███    ███ ▀▀███▀▀▀     ███   ███     ███     ███    ███ ▀▀███▀▀▀▀▀   ▀▀███▀▀▀
  ███    ███ ███    ███ ███    ███   ███    █▄  ███   ███     ███     ███    ███ ▀███████████   ███    █▄
  ███    ███ ███   ▄███ ███    ███   ███    ███ ███   ███     ███     ███    ███   ███    ███   ███    ███
  ███    █▀  ████████▀   ▀██████▀    ██████████  ▀█   █▀     ▄████▀   ████████▀    ███    ███   ██████████
                                                                                   ███    ███
"
puts "Display instructions? (Y for instructions)"

display_instructions = gets.chomp

if display_instructions == 'Y'
  puts "You will enter a dark and dangerous forest."
  puts "Your goal is to get as much gold as possible, steal the Crystal Crown from a sleeping dragon, and exit alive."
end
```

(the title ASCII-art can be generated with a website such as [this one](http://patorjk.com/software/taag))

<div class="note">
<p>The twist here is that we're going to wrap our code in a method named `display_title`. (If the students are a bit rusty on methods, a small intro/refresher beforehand should be done. Keypoints: methods have a name, can take "arguments", have a return value, are delimited by their begin...end, variables in a method can only be accessed in the method).</p>
</div>

When wrapped into a method, our code doesn't look very different:

```ruby
#This method will print the game's title and instructions
def display_title

  puts "
                                            WELCOME TO THE WORLD OF
   ▄████████ ████████▄   ▄█    █▄     ▄████████ ███▄▄▄▄       ███     ███    █▄     ▄████████    ▄████████
  ███    ███ ███   ▀███ ███    ███   ███    ███ ███▀▀▀██▄ ▀█████████▄ ███    ███   ███    ███   ███    ███
  ███    ███ ███    ███ ███    ███   ███    █▀  ███   ███    ▀███▀▀██ ███    ███   ███    ███   ███    █▀
  ███    ███ ███    ███ ███    ███  ▄███▄▄▄     ███   ███     ███   ▀ ███    ███  ▄███▄▄▄▄██▀  ▄███▄▄▄
▀███████████ ███    ███ ███    ███ ▀▀███▀▀▀     ███   ███     ███     ███    ███ ▀▀███▀▀▀▀▀   ▀▀███▀▀▀
  ███    ███ ███    ███ ███    ███   ███    █▄  ███   ███     ███     ███    ███ ▀███████████   ███    █▄
  ███    ███ ███   ▄███ ███    ███   ███    ███ ███   ███     ███     ███    ███   ███    ███   ███    ███
  ███    █▀  ████████▀   ▀██████▀    ██████████  ▀█   █▀     ▄████▀   ████████▀    ███    ███   ██████████
                                                                                   ███    ███
"
  puts "Display instructions? (Y for instructions)"

  display_instructions = gets.chomp

  if display_instructions == 'Y'
      puts "You will enter a dark and dangerous forest."
      puts "Your goal is to get as much gold as possible, steal the Crystal Crown from a sleeping dragon, and exit alive."
  end

end
```

Note the comment above the method's name: while not required by Ruby, it is a good practice to always have a short description of what a method does. Insist that the students do this from now on.

Of course, if the program is run now, nothing will happen - the method needs to be called. This can be done simply with:

```ruby
display_title
```

We're also going to add a `system('cls')` (or `system('clear')` on UNIX systems) to clear the screen after the title and instructions have been displayed.

##Iteration 1: Generating the forest

Now is time to get started on the game itself. For simplicity's sake, our player will move in a square, 2-dimensional grid-like forest divided into cells. The player will move from one cell to another.

We need a way to represent the forest in our program. Get ideas from the students - what type of variable would we use? brainstorming here is critical; insist that as programmers, one of the most challenging problems is to figure out how to represent ideas or real world objects into our code.

How do you represent a car in a computer program? It depends on what your program does. If it is a racing game, then we care about things like the car's speed, durability, etc. If our program manages sales for a car dealership, then we care about the car's options, different prices, etc. What we represent in our program will depend on the goal of our program.

Here, we're going to represent a forest with a 2-dimensional array, i.e. a grid. A 2-dimensional array is an array in which each element itself is an array. Here's what a 3x3 array looks like:

```ruby
my_array = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9],
]
```

A 2-dimensional array can be addressed as such:

```ruby
my_array[0][0] = "42"
```

Of course, the normal restrictions of arrays apply; this won't work on a 3x3 array:

```ruby
my_array[42][0] = "12"
```

2-dimensional arrays can be of any size by any size - but for our forest, we'll stick with a square one; that is, one where the width and height are the same. In each cell, we'll store what's in this part of the forest- by default, it'll be empty.

```ruby
forest = [
    ['empty', 'empty', 'empty'],
    ['empty', 'empty', 'empty'],
    ['empty', 'empty', 'empty'],
]
```

##Iteration 2: Custom sized forest

The previous is great; however, if we want to change the forest's size (for example to increase the difficulty or size of the world to explore), we need to edit our code. Ideally, we want a smart way to initialize our forest.

So far, we've been declaring arrays as such:

```ruby
some_array = [] #if we want an empty array
some_other_array = [0,0,0] #if we want an array with 3 zeroes in it
```

In Ruby, there's always more than one way to do things. Another way to declare an array is like this:

```ruby
some_array = Array.new #empty array
some_other_array = Array.new(3) #empty array of size 3
another_array = Array.new(5){0} #empty array of size 5 filled with zeroes (also called "default value")
```

How will this translate for our forest? Have the students figure it out, ideally in two steps:

- Creating a one-dimensional forest:

```ruby
forest = Array.new(3){'empty'}
```

- Creating a two-dimensional forest; the trick is that the default value doesn't have to be a string or a number:

```ruby
forest = Array.new(3){ Array.new(3){'empty'} }
```

And here we go! We generated our forest, in a single line.

The last little tweak we're going to do is not use numbers directly in our code. Often, in programming, we want things to be as easy to read as possible - numbers are bad because someone reading the code later will have no clue what the meaning behind the 3 is. Numbers hanging around like that are called "magic values", and they're bad. What we want to do instead is put that number in a variable with a meaningful name.

The size of the forest won't size throughout our program: it's a *constant*. In most programming languages, we type constants in ALL_CAPS.

```ruby
FOREST_SIZE = 3
forest = Array.new(FOREST_SIZE){ Array.new(FOREST_SIZE){'empty'} }
```

##Iteration 3: Moving around the forest

Alright! We have an empty forest. Time for us to put a player in it, and have him move around.

First of all, we need a way to know where our player is. Work with the students: how do we identify a position in the forest? (x,y coordinates) How can we represent those? (for now, two variables)

```ruby
player_x = 0
player_y = 0
```

This will make our player start at the top-left corner, which is great for now. Maybe later we'll want the player to start at a random position in the forest (where an evil wizard teleported him to), but for now top-left is good.

Why top-left? Remember that our forest is a two-dimensional array. Therefore `forest[0]` will return the first array; and `forest[0][0]` will be the first element of the first array. If we visualize our 2d array as a top-view map of the forest, then (0,0) is the top left corner.

```ruby
[
    [0, 1, 2]
    [0, 1, 2]
    [0, 1, 2]
]
```

So let's have 2 variables to store the player position:

```ruby
player_x = 0
player_y = 0
```

Now we'll prompt the player about where they wish to move the player. Unfortunately, because our game is text only, we can't use the arrow keys to move the player around in "real time": our game will have to be turn by turn.

Every turn, we will ask the player where they wish to move (up/down/left/right) and move the player accordingly. The students should be able to implement that fairly easily; the only catch is the coordinates system. Keeping the structure of the array in mind should help with that.

```ruby
begin
    puts "You are now at #{player_x}, #{player_y}"
    puts "Where to, noble adventurer?"
    direction = gets.chomp

    if direction == 'left'
        player_x = player_x - 1
    elsif direction == 'right'
        player_x = player_x + 1
    elsif direction == 'up'
        player_y = player_y - 1
    elsif direction == 'down'
        player_y = player_y + 1
    end
end while true
```

Let's try out our program. What's wrong with it?

##Iteration 4: Bounding player movement

The problem with the previous code is that the player can easily step out of the forest's boundaries. We need to only let the player move in a certain direction if he'll stay within the forest's boundaries.

How do we figure this out? We need to know which values within which the 2 player coordinate variables (player_x, player_y) must remain. That's 0 for a minumum, and FOREST_SIZE for a maximum. Have the students try ot figure out what to add in the code so that the player can't overstep their boundaries.

```ruby
begin
    puts "You are now at #{player_x}, #{player_y}"
    puts "Where to, noble adventurer?"
    direction = gets.chomp

    if direction == 'left'
        if player_x > 0
            player_x = player_x - 1
        end
    elsif direction == 'right'
        if player_x < FOREST_SIZE
            player_x = player_x + 1
        end
    elsif direction == 'up'
        if player_y > 0
            player_y = player_y - 1
        end
    elsif direction == 'down'
        if player_y < FOREST_SIZE
            player_y = player_y + 1
        end
    end
end while true
```

Boom! Wonderful. (we can also decide to display a "You can't go there message" with an else for each of the inner ifs- that's up to the students.)

##Iteration 5: Refactoring the movement code

The previous code works great, but there is a slight problem - our main loop is already 15 lines long, and we haven't even added treasures and enemies yet! We want to keep our code as easy to read as possible.

To do this, we are going to move the movement code into its own method. Ultimately, we want our main loop to look like this:

```ruby
begin
    puts "You are now at #{player_x}, #{player_y}"
    puts "Where to, noble adventurer?"
    direction = gets.chomp

    move_player(direction)
end while true
```

This is much better, and will keep the code easy to read as we add more features. Have the students write the `move_player` method.

```ruby
#Move the player around the forest with the given direction
def move_player(direction)
    if direction == 'left'
        if $player_x > 0
            $player_x = $player_x - 1
        end
    elsif direction == 'right'
        if $player_x < $FOREST_SIZE
            $player_x = $player_x + 1
        end
    elsif direction == 'up'
        if $player_y > 0
            $player_y = $player_y - 1
        end
    elsif direction == 'down'
        if $player_y < $FOREST_SIZE
            $player_y = $player_y + 1
        end
    end
end
```

This looks great! Unfortunately, trying to run the code won't work.

{% terminal %}
-> % ruby forest_2.rb
You are now at 0, 0
Where to, noble adventurer?
up
forest_2.rb:18:in `move_player': undefined local variable or method `player_y' for main:Object (NameError)
    from forest_2.rb:33:in `main'
{% endterminal %}

Remember: the only variables we can have access to in methods are the variables defined in that method. Here, `player_x`, `player_y` and `FOREST_SIZE` were all defined outside of the method, so we can't use them inside the method (the inside of the method is also called its "scope").

Fortunately, there's an easy way to solve this problem called global variables - a way for methods to access variables defined outside of its scope. For a variable to be global, its name must start with a `$`.

```ruby
#This won't work
my_variable = 0

def test
    my_variable = my_variable + 1
end

#But this will
$my_variable = 0

def test
    $my_variable = $my_variable + 1
end
```

So all we need to do is make our `player_x`, `player_y` and `FOREST_SIZE` variables global. The final code for this part of the program will look like this:

```ruby
$FOREST_SIZE = 3
forest = Array.new($FOREST_SIZE){ Array.new($FOREST_SIZE){'empty'} }

$player_x = 0
$player_y = 0

def display_title
    #...
end

#Move the player around the forest with the given direction
def move_player(direction)
    if direction == 'left'
        if $player_x > 0
            $player_x = $player_x - 1
        end
    elsif direction == 'right'
        if $player_x < $FOREST_SIZE
            $player_x = $player_x + 1
        end
    elsif direction == 'up'
        if $player_y > 0
            $player_y = $player_y - 1
        end
    elsif direction == 'down'
        if $player_y < $FOREST_SIZE
            $player_y = $player_y + 1
        end
    end
end

#Our program starts here
display_title

begin
    puts "You are now at #{$player_x}, #{$player_y}"
    puts "Where to, noble adventurer?"
    direction = gets.chomp

    move_player(direction)
end while true
```

##Iteration 6: Adding a map

Our game is slowly taking shape! Right now though, it's kind of hard to visualize where we are in the forest. To solve this problem, we are going to draw a map of the forest each turn. We're going to write a `draw_forest` method which, when called, will draw the forest on screen.

The first step here is seeing that this method will need to access the `forest` variable; therefore, we should make it global.

```ruby
$forest = Array.new($FOREST_SIZE){ Array.new($FOREST_SIZE){'empty'} }
```

Let's write our method- the first thing we want to do is iterate through our forest structure to display what's in each cell. Using `.each`, let the students figure it out. They should come up with something like:

```ruby
#Draw a map of the forest
def draw_forest

    $forest.each do | forest_row|

        $forest_row.each do |cell|
            print cell
        end
        puts '' #print an empty line here so that cells don't get displayed next to one another
    end

end
```

We're getting there! Our map is a bit hard to read though. First thing first, let's display the `▯` character instead of `empty` for when a cell is empty. How do we do that?

```ruby
#Draw a map of the forest
def draw_forest

    $forest.each do |forest_row|

        $forest_row.each do |cell|

            if cell == 'empty'
                print '▯'
            end

        end
        puts '' #print an empty line here so that cells don't get displayed next to one another
    end

end
```

This works just fine. However, remember that in Ruby, there are always different ways to achieve the same goal. Here, while our method works, we're going to want to display the player's position on the map - that is, use the x,y coordinates we have for the player - and we don't have a way to track x and y coordinates with the `.each`.

Instead, we're going to use the `.times` method. This method is used as follows:

```ruby
5.times do |counter|
  puts counter
end
```

Let the students grok it, and then brainstorm with them- how can we use `.times` to our advantage to go over every cell of the forest and keep track of our current (x,y) coordinates? We're going to want to use `$FOREST_SIZE`, and because we have 2 separate coordinates, we'll need two nested `for` loops.

```ruby
#Draw a map of the forest
def draw_forest

    $FOREST_SIZE.times do |y|
        $FOREST_SIZE.times do |x|

            if $forest[x][y] == 'empty'
                print '▯'
            end
        end
        puts
    end

end
```

<div class='note'>
<p>
    Note that our first loop is y, and the second loop is x - rather than the opposite. This is because the first loop will loop over the subarrays of our $forest structure (that is, the y dimension) and the second loop will loop over the cells themselves. If a student writes the loop the wrong way by mistake, their program will have the bug where typing left moves the character up, right moves the character down, etc.
</p>
</div>

And our last step is to draw a `☺` where the player is. The students should be able to figure it out:

```ruby
#Draw a map of the forest
def draw_forest

    $FOREST_SIZE.times do |x|
        $FOREST_SIZE.times do |y|

            if ($player_x == x) && ($player_y == y)
                print '☺'
            elsif $forest[x][y] == 'empty'
                print '▯'
            end
        end
        puts
    end

end
```

Now as we play the game, we get a neat looking map as the player moves around.

{% img center /images/projects/adventure/adventure_map.gif %}

##Iteration 7: Adding gold chests

Well, we have a forest, and our player can move around in it- but it's completely empty! Time to start the fun part- adding objects and enemies to our forest! Let's start with adding gold chest. When the player arrives on a cell with a gold chest, they will gain +10 gold.

First, we want to define a constant for the number of chests we'll have in the forest. Remember, we always put constants at the top of the file, grouped together.

```ruby
$NUM_CHESTS = 3
```

Now we're going to write a method which places chests randomly in the forest. Have the students come up with pseudocode for this; it should look something like:

```
- pick a random position on the map
- if it's empty, place a chest there
- if we have placed $NUM_CHESTS, end; otherwise, repeat.
```

In Ruby, we'll get this:

```ruby
def place_chests
  chests_placed = 0

  begin
    chest_x = rand($FOREST_SIZE)
    chest_y = rand($FOREST_SIZE)

    if $forest[chest_x][chest_y] == 'empty'
      $forest[chest_x][chest_y] = 'chest'
      chests_placed = chests_placed + 1
    end

  end while chests_placed < $NUMBER_CHESTS

end
```

Let's call the method in our program. Where do we want to put it? (right before our main loop)

Have the students run the game. What's wrong? The chests don't appear on the map :( So let's modify the `draw_forest` method to display chests as well. This should be easy to do:

```ruby
#Draw a map of the forest
def draw_forest

    $FOREST_SIZE.times do |y|
        $FOREST_SIZE.times do |x|

            if ($player_x == x) && ($player_y == y)
                print '☺'
            elsif $forest[x][y] == 'empty'
                print '▯'
            elsif $forest[x][y] == 'chest'
                print '$'
            end

        end
        puts
    end

end
```

Let's run the program - the gold chests should show up on the map now!

##Iteration 8: Picking up the gold

Have the player walk around on the map. What happens when the player goes over a gold chest? Sadly, nothing. Let's fix that!

The first thing we're going to do is add a variable that holds how much gold the player has. Let's add it right after our definition for `$player_x` and `$player_y`.

```ruby
$player_gold = 0
```

We're now going to add a method named `check_for_action`. What will this method do?

This method will be called in the main loop, right after drawing the map, and will check if there is any action to perform on the current cell. For now this will only be opening gold chests; but later, it can be anything we want it to! (for example fight ennemies, avoid a trap, teleport somewhere else, etc.)

So let's write our method. In pseudo code, what should it do?

```
- check the current player's cell
- if it's a gold chest:
-- give the player 10 gold
-- remove the chest
```

In Ruby:

```ruby
def check_for_action
  if $forest[$player_x][$player_y] == 'chest'
    #give the gold to the player
    puts "You open a chest and find 10 gold!"
    $player_gold = $player_gold + 10
    #remove the chest
    $forest[$player_x][$player_y] = 'empty'
  end
end
```

Let's now try out our program:

{% img center /images/projects/adventure/adventure_2.gif %}

##Iteration 9: Adding bats

##Iteration 10: Adding the dragon and the ring

##Iteration 11: Wrapping up

##Improvements
- Add more enemies

- Allow player to choose a "class" at the beginning of the game (e.g.: warrior: gets 2 more arrows; thief: can sneak by bats 3 times per game.)

- Use the colorize module to make the game more pleasant

- ... anything you want! This project is very open ended, and students can do whatever they please - the only limit is their imagination and the amount of work they put in. For inspiration, show them YouTube videos of famous text games such as Rogue, Dwarf Fortress, etc.

##Full code
