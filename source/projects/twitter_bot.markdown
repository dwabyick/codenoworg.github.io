---
layout: page
title: TwitterBot
sidebar: true
---

In this multi-phase project, you will build a client that interacts with the Twitter messaging service. Your client will both mimic functionality found through the twitter.com web interface as well as perform many new tasks.

Learning Goals:

* Practice installing and using a gem library
* Writing methods and basic instructions
* Practice basic techniques including conditional branching and looping

<div class="note">
<p>The Twitter API and gem are constantly changing. We do our best to keep this tutorial updated, but sorry if things get confusing.</p>
</div>

<div class="note">
<p>This tutorial is open source. If you notice errors, typos, or have questions/suggestions, please <a href="https://github.com/CodeNowOrg/codenoworg.github.io">submit them to the project on Github</a>.</p>
</div>

## Iteration 0: Up & running

For Windows machines, we will have to install a valid SSL certificate. Have the students visit the following link: http://bit.ly/twitterbot-ssl. The code in the gist downloads an SSL certificate onto the laptop. Copy the code, go into IRB, right click the window and select `paste`. If everything worked correctly, a `cacert.pem` file will have downloaded into the `C:\RailsInstaller` folder.

Next, we'll have to set up an environment variable to reference the downloaded certificate. Students can edit the system environment variables by hitting the windows key and typing "edit the system environment variables."

{% img center /images/projects/twitter_bot/start_menu.png %}

Once they hit enter, they should be brought to the following screen:

{% img center /images/projects/twitter_bot/environment_variables.png %}

Click on `new` to create a new system environment variable. Set the variable name to SSL_CERT_FILE and the variable value to `C:\RailsInstaller\cacert.pem`:

{% img center /images/projects/twitter_bot/new_environment_variable.png %}

Click `OK` on the windows and open the command prompt.

Install the `jumpstart_auth` gem by running `gem install jumpstart_auth` from your command prompt:

{% terminal %}
$ gem install jumpstart_auth
Fetching: oauth-0.4.7.gem (100%)
Fetching: multipart-post-1.1.5.gem (100%)
Fetching: faraday-0.8.4.gem (100%)
Fetching: simple_oauth-0.1.9.gem (100%)
Fetching: twitter-3.7.0.gem (100%)
Fetching: jumpstart_auth-0.2.0.gem (100%)
Successfully installed oauth-0.4.7
Successfully installed multipart-post-1.1.5
Successfully installed faraday-0.8.4
Successfully installed simple_oauth-0.1.9
Successfully installed twitter-3.7.0
Successfully installed jumpstart_auth-0.2.0
6 gems installed
{% endterminal %}

To verify that we've set everything up right, have the students open up IRB and run these two instructions:

{% irb %}
$ require 'jumpstart_auth'
$ => true
$ twitter = JumpstartAuth.twitter
$ => #<#<Class:0x007fab7ded7990>>
{% endirb %}

### Dealing with OAuth

The OAuth authentication system is a complex private/public key exchange that requires several steps and a difficult-to-follow workflow that requires a handshake with the remote service. The JumpstartAuth gem handles this complexity for us. The `JumpstartAuth.twitter` call initiates the handshake sequence and should open a login screen:

{% img center /images/projects/twitter_bot/twitter_login.png %}

We've setup several test accounts that students can use. Ask Ryan for the username and passwords. Have 2-3 students share a demo account, or login with their own Twitter accounts.

Twitter will then present each user with a 7-digit PIN number. Enter the PIN number into the command prompt to complete the handshake sequence. The JumpstartAuth gem will then create a `settings.yml` file which will contain the authentication tokens.

{% img center /images/projects/twitter_bot/twitter_pin.png %}

With that, we're ready to start coding!

## Iteration 1: Posting tweets

Have the students navigate to their codenow directory (typically `C:\Users\codenow\code`) and create a new ruby file using the `touch` command called `twitter_bot.rb`.

### Step 1 - Write the `tweet` Method

The JumpstartAuth gem provides convenience methods to easily tweet from a ruby script. 
Using `irb`, show the students how the JumpstartAuth gem works and verify that it is posting to the linked Twitter account:

{% irb %}
$ require 'jumpstart_auth'
$ => true
$ client = JumpstartAuth.twitter
$ => #<#<Class:0x007fab7ded7990>>
$ client.update("I'm tweeting from IRB.")
$ => #<Twitter::Tweet:0x007fab79048b08>
{% endirb %}

We want to encapsulate this functionality into a method called `tweet` which takes one argument, a message. If students get stuck on defining the method, have them refer to their encryptor programs they wrote in week 2.

```ruby
require 'jumpstart_auth'

def tweet(message)
  client = JumpstartAuth.twitter
  client.update(message)
end

puts "Initializing"
tweet("TwitterBot Initialized.")
```

Have the students run their programs. If everything was entered correctly, `Initializing` should be printed to the terminal and a message should appear on their Twitter stream.

### Step 2 - Length Restrictions

Twitter messages are limited to 140 characters. Have the students pass in an string that is longer than 140 characters to the tweet method. Ask the students to describe what happens and tell them we want to  write some code that will prevent users from posting messages longer than 140 characters.

Work with the students to come up with pseudocode for the length-checking functionality:

```ruby
# If the message to tweet is less than or equal to 140 characters long, tweet it.
# Else, print out a warning message and do not post the tweet.
```

Remind students about the `length` method on strings and use irb to show them some examples of how it works. Work with the students to fill in the pseudocode

```ruby
# If the message to tweet is less than or equal to 140 characters long, tweet it.
if message.length <= 140
  client.update(message)
# Else, print out a warning message and do not post the tweet.
else
  puts "Message is over 140 characters!"
end
```

Test the new `tweet` method with a message that is less than 140 characters, one that is exactly 140 characters, and one that's longer than 140 characters.

Ask the students how they would generate a string that's exactly 140 characters. They are familiar with the `*` operator for strings. Show them how the `*` works in `irb`:

{% irb %}
$ puts "a" * 10
$ => "aaaaaaaaaa"
{% endirb %}

## Iteration 2: Improving the interface

Point out that every time they want to change their tweet, they have to edit their Ruby file. Ask the students for suggestions on how they can make their programs better. Explain how an interactive command-line prompt interface (like `irb`) would be better.

### Step 1 - Writing the while loop

Underneath the `tweet` method we'll want to use a `while` loop to repeat instructions over and over. Remind the students that there are three parts to the while loop: the begin, the end, and the condition. Replace the last lines of the `twitter_bot.rb` as such:

```ruby
require 'jumpstart_auth'

def tweet(message)
  client = JumpstartAuth.twitter
  if message.length <= 140
    client.update(message)
  else
    puts "Message is over 140 characters!"
  end
end

begin
  command = ""
  puts "Please enter a command: "
end while command != "q"
```

Have the students run their programs and ask what happens and have them explain the output. Show them that `ctrl + c` will break out of their infinite loop.

### Step 2 - Getting user input

Students have already seen the `gets` command in their first week. If they can't remember how to get user input, have them look at their Hi-Lo game and their Rock, Paper, Scissors game for hints. They should modify their while loops to look as follows:

```ruby
begin
  command = gets.chomp
  puts "Please enter a command: "
end while command != "q"
```

Have the students run their programs to ensure that their code is in a working state. Ask them what happens this time and why the program is different.

Ask students to explain why we need to call the `chomp` method on the gets. Remind students that `gets` picks up the enter key too. So the `command` variable is actually getting the string `"q\n"` where that backslash-n is a new line. The `chomp` method will remove any whitespace (spaces, tabs, newlines) on the back end of a string.

### Step 3 - Building if/else statements

We think we're getting the instruction from the user, but we need to actually do something with it. We can use if/else statements to process the commands. Work with the students to come up with a simple structure to handle the q command:

```ruby
if command == "q"
  puts "Goodbye!"
else
  puts "Sorry, I don't know how to #{command}"
end
```

Have the students run their programs and test some commands.

### Step 4 - Tweeting and arguments

Work with the students to modify the if/else structure to work with the tweet method. Add a `elsif` block that executes when the `command` is `"t"`. Have it call our `tweet` method.

```ruby
if command == "q"
  puts "Goodbye!"
elsif command == "t"
  tweet(message)
else
  puts "Sorry, I don't know how to #{command}"
end
```

Have the students run their programs and try entering `t This is only a test!`.

Students should see, `Sorry, I don't know how to (t This is only a test!)`. Ask the class why the message wasn't tweeted.

### Step 5 - Splitting the command

The line `command = gets.chomp` is an example of bad variable naming. It isn't just getting the `command`, it's actually getting a `command` and a message to send to that command. Have the class change this line to `input = gets.chomp` then we'll work with `input` to pull out the command.

Now that we have `input` we need to split it up into pieces. Demonstrate how the `split` method works with some simple examples in `irb`. Just below the `input = gets.chomp` add a line that says `parts = input.split(" ")` to chop `input` into `parts`.

Remind the students how to access elements inside of an array. Show them how we can use ranges to access multiple elements and have them attempt to set two variables called `command` and `message`.


```ruby
require 'jumpstart_auth'

def tweet(message)
  client = JumpstartAuth.twitter
  if message.length <= 140
    client.update(message)
  else
    puts "Message is over 140 characters!"
  end
end

begin
  puts "Please enter a command: "
  input = gets.chomp
  parts = input.split(" ")
  command = parts[0]

  if command == "q"
    puts "Goodbye!"
  elsif command == "t"
    message = parts[1..-1].join(" ")
    tweet(message)
  else
    puts "Sorry, I don't know how to #{command}"
  end
end while command != "q"
```

Have the class try out their programs to test sending multiple tweets. At this point, it's possible that they will exceed the Twitter API rate limits. If that is the case, have them either create new accounts or wait a few minutes for Twitter to restore their access.

## Iteration 3: Direct messaging

We want to increase the functionality of our Twitter bot by giving it the ability to send our followers private messages. Sending these Direct Messages isn't that different from posting tweets. In fact, we can reuse our existing `tweet` method to do most of the hard work.

### Step 0 - Writing the dm method

Explain to the students that we want to package the direct message functionality into a method. Have them attempt to write a method called `dm` that takes two arguments, a twitter handle, and a message, and prints them to the screen. It should look as follows:

```ruby
def dm(target, message)
  puts "Trying to send #{target} this direct message:"
  puts message
end
```

In order to test the method, have the students copy the method directly into `irb` and have them call the method.

### Step 1 - Incorporating the dm method

And we need to add the command to our while loop. Ask the students how they would expect the dm call to work. Guide them towards inputting something like: `dm user_name text of the message`. Have them attempt to write the new if statement. It should look as follows:

```ruby
begin
  puts "Please enter a command: "
  input = gets.chomp
  parts = input.split(" ")
  command = parts[0]

  if command == "q"
    puts "Goodbye!"
  elsif command == "t"
    message = parts[1..-1].join(" ")
    tweet(message)
  elsif command == "dm"
    target = parts[1]
    message = parts[2..-1].join(" ")
    dm(target, message)
  else
    puts "Sorry, I don't know how to #{command}"
  end
end while command != "q"
```

### Step 2 - Creating and sending the message

In order to direct message someone using the JumpstartAuth gem, we simply use the `update` method with a message that starts with "d user_name". Have the students test the direct messaging functionality in irb.

{% irb %}
$ client = JumpstartAuth.twitter
$ => #<#<Class:0x007fe9c8e5fa88>:0x007fe9c8ec18c8>
$ client.update("d jsl_demo_01 Hello, world.")
$ => #<Twitter::DirectMessage:0x007fe9c8eb8520>
{% endirb %}

Ask the students how we can use the `tweet` method in order to send our direct messages. Have the students work together to modify the `dm` method to use the `tweet` method. Remind them about string interpolation. The resulting method should look as follows:

```ruby
def dm(target, message)
  tweet("d #{target} #{message}")
end
```

Have the students try sending a direct message to other demo accounts. If the DM doesn't show up it is probably because DMs can only be sent to people who are following the account. Have the recipient follow the sender and try again.

## Iteration 4: Writing a spam bot

### Step 1 - Getting a list of followers

We want to create a method named `followers_list` that returns an array containing the usernames of all my followers

The JumpstartAuth gem provides a method `followers` that returns an array of followers for a given account. The follower objects behave much like hashes. Walk through a simple example with the class:

{% irb %}
$ client = JumpstartAuth.twitter
$ => #<#<Class:0x007fe9c8e5fa88>:0x007fe9c8ec18c8>
$ followers = client.followers
$ => [ #<Twitter::User:0x007fba710863c0>, #<Twitter::User:0x007fba71086230>, #<$ Twitter::User:0x007fba71086280>, ... ]
$ follower = followers[0]
$ => #<Twitter::User:0x007fba710863c0>
$ follower["screen_name"]
$ => "jsl_demo_06"
{% endirb %}

Work with the class to sketch out some pseudocode for the `followers_list` method:

```ruby
# Define a method called `followers_list` that takes no parameters
# Create a `client` variable and set it to the JumpstartAuth client
# Create a `screen_names` variable and set it to a blank array
# Create a `followers` variable and set it to the followers
# Iterate through the `followers` using an each loop and `push` screen names into the array
# Return the `screen_names` variable
```

Have students work together to write out the `followers_list` method. Challenge the class to write an each loop to get all of the screen names of their followers. If they get stuck, have them refer to their encryptor programs they built in Week 2. It should look as follows:

```ruby
# Define a method called `followers_list` that takes no arguments
def followers_list
  # Create a `client` variable and set it to the JumpstartAuth client
  client = JumpstartAuth.twitter

  # Create a `screen_names` variable and set it to a blank array
  screen_names = []

  # Create a `followers` variable and set it to the client's followers
  followers = client.followers

  # Iterate through the `followers` using an each loop and `push` screen names into the array
  followers.each do |follower|
    screen_names.push(follower["screen_name"])
  end

  # Return the `screen_names` variable
  return screen_names
end
```

### Step 2 - Direct messaging all followers

Next, we need to write a method named `spam_my_followers` that uses the followers_list and tries to send them a direct message using the `dm` method

We'll again start off by writing some pseudo code:

```ruby
# Define a method called `spam_my_followers` that takes one argument named `message`
# Use the `followers_list` method to retrieve a list of followers and save that in a variable called `screen_names`
# Loop through the `screen_names` variable using an `each` loop
# Use the `dm` method to send them a message
```

Work with the class to fill in the pseudocode.

```ruby
# Define a method called `spam_my_followers` that takes one argument named `message`
def spam_my_followers(message)
  # Use the `followers_list` method to retrieve a list of followers and save that in a variable called `screen_names`
  screen_names = followers_list
  
  # Loop through the `screen_names` variable using an `each` loop
  screen_names.each do |screen_name|

    # Use the `dm` method to send them the `message` argument
    dm(screen_name, message)
  end
end
```

We want to be able to execute the spam functionality from our command-line interface. Ideally, we'd like to be able to type in `s some_message` and have it spam "some_message" to all of our followers. Have the class implement this functionality, and have them test it out to make sure that it works. 

Here's what the full script should look like:

```ruby
require 'jumpstart_auth'

def tweet(message)
  client = JumpstartAuth.twitter
  if message.length <= 140
    client.update(message)
  else
    puts "Message is over 140 characters!"
  end
end

def dm(target, message)
  tweet("d #{target} #{message}")
end

def followers_list
  client = JumpstartAuth.twitter
  screen_names = []
  followers = client.followers

  followers.each do |follower|
    screen_names.push(follower["screen_name"])
  end

  return screen_names
end

def spam_my_followers(message)
  screen_names = followers_list
  screen_names.each do |screen_name|
    dm(screen_name, message)
  end
end

begin
  puts "Please enter a command: "
  input = gets.chomp
  parts = input.split(" ")
  command = parts[0]

  if command == "q"
    puts "Goodbye!"
  elsif command == "t"
    message = parts[1..-1].join(" ")
    tweet(message)
  elsif command == "dm"
    target = parts[1]
    message = parts[2..-1].join(" ")
    dm(target, message)
  elsif command == "s"
    message = parts[1..-1].join(" ")
    spam_my_followers(message)
  else
    puts "Sorry, I don't know how to #{command}"
  end
end while command != "q"
```