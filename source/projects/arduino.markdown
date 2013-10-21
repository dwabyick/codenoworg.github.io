---
layout: page
title: Arduino
sidebar: true
---

In this multi-phase project, students will build Ruby scripts that interact with an Arduino. It's a cool way to write programs which affect the physical world. In this project, we'll play with some neat lights and see how the programming instructions affect what you observe.

Learning Goals:

* Understand how software allows computers to interface with hardware
* Learn about basic circuitry
* Continue strengthening programming fundamentals

<div class="note">
<p>This tutorial is open source. If you notice errors, typos, or have questions/suggestions, please <a href="https://github.com/CodeNowOrg/codenoworg.github.io">submit them to the project on Github</a>.</p>
</div>

## Iteration 0: Up & running

### Step 1 - Install the Arduino drivers

For Windows machines, we will have to manually install the Arduino USB drivers. Students can get to the device manager by hitting the windows key and typing "device manager."

{% img center /images/projects/arduino/device_manager_1.png %}

Once they hit enter, they should be brought to the following screen:

{% img center /images/projects/arduino/device_manager_2.png %}

Right click the "Unknown device" and select "Update Driver Software..."

{% img center /images/projects/arduino/update_driver_1.png %}

Make sure the "Include subfolders" box is checked and click "Browse..."

{% img center /images/projects/arduino/update_driver_2.png %}

Look for the folder labeled "arduino-1.5.2" (typically located on the desktop) and click "OK".

{% img center /images/projects/arduino/update_driver_3.png %}

Click "Next"

{% img center /images/projects/arduino/update_driver_4.png %}

A Windows Security prompt should open. Click "Install".

{% img center /images/projects/arduino/install_driver_1.png %}

If the driver was successfully installed, a success message should appear:

{% img center /images/projects/arduino/success.png %}

### Step 2 - Installing the `dino` gem

<div class="note">
<p>The latest version of the dino gem does not function properly with this tutorial. You MUST use version 0.10.0.</p>
</div>

We will be using the `dino` Ruby gem to help us quickly start programming our Arduinos. To install the `dino` gem, run `gem install dino --version 0.10.0` from the command prompt:

{% terminal %}
$ gem install dino --version 0.10.0
{% endterminal %}

### Step 3 - Bootstrapping the Arduino

Bootstrapping

We need to load a "bootstrap" program onto the Arduino board that’ll let us manipulate it from Ruby.

Open up the Arduino program from the arduino folder on the desktop.

{% img center /images/projects/arduino/arduino_explorer.png %}

If there is any code inside the editor, have the students delete it. The main screen of the program should look as follows:

{% img center /images/projects/arduino/arduino_main.png %}

Select the port for your Arduino in the Arduino software: Tools => Serial Port => COM3 (or whatever COM port is available).

{% img center /images/projects/arduino/arduino_port.png %}

Copy and paste the contents of the [du.ino gist](http://bit.ly/codenow-arduino10) into the Arduino editor. In the file window, first click the verify button, then click the right-arrow-in-a-circle button for "Upload". During upload, small LEDs will flash on the Arduino board. Once the LEDs are steady, the upload has finished.

{% img center /images/projects/arduino/arduino_bootstrap.png %}

## Iteration 1: Wiring Up

Before the students can begin programming their Arduinos, they need to wire up the hardware first.

### LEDs

An LED is a small light with two wires. When one side is connected to a positive electical source and the other to negative (or "ground"), the LED lights up.

#### Is It Dangerous?

LEDs *are not dangerous*. The electrical voltage we're using here is so low that even if you intentionally tried to electrocute yourself, you probably couldn't even feel it. Don't be concerned.

#### LED Structure

It looks like this:

![LED](http://www.societyofrobots.com/images/electronics_led_diagram.png)

The led has two stiff wires that we'd prefer not to bend. The shorter one goes to the ground or negative line -- this is called the "cathode". The other, longer pin, goes to the positive line -- this is called the "annode".

### Let There Be Light! 

Let's build a simple circuit that involves no programming, just wiring.

* Find the following materials in your kit:
  * Blue Arduino board
  * Black USB cord
  * White "breadboard"
  * 1 long red wire
  * 1 long yellow wire
  * 1 resistor (of any resistance)
  * 1 LED

With those materials, build this circuit:

* Connect the USB cord to your computer and the Arduino. You should see the small green "ON" led light up on the board
* Put one end of the *red wire* into the hole marked *5V*
* Put one end of the *yellow wire* into any of the holes marked *GND*
* Set up the breadboard like in the diagram below, with the resistor between the cathode and GND.
* Let there be light!

{% img center /images/projects/arduino/led_setup.png %}

## Iteration 2: Flashing Lights

Have the students switch the *red wire* from the hole that is labeled *5V* to the hole that is labeled *13*. We will control the output of pin 13 from our ruby code.

Let's write a simple program and see if we can observe a reaction from the Arduino. Have the students create a file called `led.rb`. Add the contents to the file:

```ruby
require 'dino'
board = Dino::Board.new(Dino::TxRx.new)
led = Dino::Components::Led.new(pin: 13, board: board)

led.send(:off)
puts :off
sleep 2
led.send(:on)
puts :on
sleep 2
led.send(:off)
puts :off
sleep 2
```

Ask the students to walk through the code and have them run `led.rb` from the command prompt. With some hand-waving, attempt to explain the concept of classes and instances of classes.

#### Using `while` loops
Have students work together to write a while loop to repeatedly flash the lights. The program should look like this:

```ruby
require 'dino'
board = Dino::Board.new(Dino::TxRx.new)
led = Dino::Components::Led.new(pin: 13, board: board)

begin
  led.send(:off)
  puts :off
  sleep 2
  led.send(:on)
  puts :on
  sleep 2
end while true
```

{% exercise %}

#### Experimenting with Pins

Have the students move the red wire from pin 13 to pin 11. What happens? Have students figure out how to modify their programs in order to make the LED work again.

#### Faster Blinking

Tell the students we want to make the LED blink faster. Have them modify their programs to make the LED stay on for 0.1 seconds and off for 0.1 seconds.

{% endexercise %}

## Iteration 2: Refactoring blink

Work with the students to create a method called `blink` that takes two arguments: the led and the number of times to blink. The program should look as follows:

```ruby
require 'dino'
board = Dino::Board.new(Dino::TxRx.new)
led = Dino::Components::Led.new(pin: 13, board: board)

def blink(led, times)
  counter = 0
  begin
    led.send(:off)
    sleep 0.1
    led.send(:on)
    sleep 0.1
    counter = counter + 1
  end while counter < times
end

begin
  blink(led, 1)
end while true
```

Have the students run their programs and make sure their `blink` methods are working.

## Iteration 3: Revisiting the Hi-Lo Game

Here, the students will modify the Hi Lo games they built in week 1. Have them modify their games such that a correct answer results in 20 blinks, too high results in 10 blinks and too low results in no blinks.

This is how the final program might look.

```ruby
require 'dino'
board = Dino::Board.new(Dino::TxRx.new)
led = Dino::Components::Led.new(pin: 13, board: board)

def blink(led, times)
  counter = 0
  begin
    led.send(:off)
    sleep 0.1
    led.send(:on)
    sleep 0.1
    counter = counter + 1
  end while counter < times
end

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
    blink(led, 10)
  end

  # If the guess was lower than the secret number print "Too low!"
  if guess < secret_number
    puts "Too low!"
  end

  # If the guess was equal to the secret number print "Congratulations!"
  if guess == secret_number
    puts "Congratulations"
    blink(led, 20)
  end
end while guess != secret_number and tries != max_tries

if tries == max_tries and guess != secret_number
  puts "You lose!"
end
```