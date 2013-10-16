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

## Iteration 0: Up & Running

For Windows machines, we will have to install a valid SSL certificate. Have the students visit the following link: http://bit.ly/twitterbot-ssl. The code in the gist downloads an SSL certificate and saves it as `C:\RailsInstaller\cacert.pem`. Copy the code, go into IRB, right click the window and select `paste`.

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
$ irb
$ require 'jumpstart_auth'
true
$ twitter = JumpstartAuth.twitter
{% endirb %}

### Dealing with OAuth

When connecting to a third-party service, from the developer's perspective, possibly the simplest form of authentication is passing the user's username and password. Unfortunately, this puts more work on the user and is less secure than more robust schemes such as OAuth.

The OAuth authentication system is a more complex private/public key exchange that requires several steps and a difficult-to-follow workflow that requires a handshake with the remote service. So that we can focus on the important parts of this exercise, all this complexity has been pushed into the `jumpstart_auth` gem. `JumpstartAuth.twitter` initiates the handshake sequence and should open a login screen:

{% img center /images/projects/twitter_bot/twitter_login.png %}

We've setup several test accounts that students can use. Ask Ryan for the username and passwords. Have 2-3 students share a demo account, or login with their own Twitter account.

Twitter will then give you a pin number that's about 7 digits. Copy it to your clipboard, go over to your IRB session, and paste it in where the prompt says `Enter the supplied pin:`.

{% img center /images/projects/twitter_bot/twitter_pin.png %}

## Iteration 1: Posting Tweets

Have the students create a new ruby file called `twitter_bot.rb` in the codenow directory.

### Step 1 - Write the `tweet` Method

```ruby
def tweet(message)
  client = JumpstartAuth.twitter
  client.update(message)
end

puts "Initializing"
tweet("TwitterBot Initialized.")
```

Then run your code by going to your terminal an entering:

{% terminal %}
$ ruby twitter_bot.rb
{% endterminal %}

You should see the output say `Initializing`. Now go to your test account's Twitter page and look for your results!

#### Step 2 - Length Restrictions

Twitter messages are limited to 140 characters. Have the students pass in an string that is longer than 140 characters to the tweet method. Have them describe what happens and have them create some error checking that will prevent users from posting messages longer than 140 characters.

Inside the `tweet` method, have the students write an `if`/`else` block that performs the following logic:

* If the message to tweet is less than or equal to 140 characters long, tweet it.
* Otherwise, print out a warning message and do not post the tweet.

Remind students about the `length` method on strings. Test the new `tweet` method with a message that is less than 140 characters, one that is exactly 140 characters, and one that's longer than 140 characters.

Ask the students how they would generate a string that's exactly 140 characters. They are familiar with the `*` operator for strings. Show them how the `*` works in `irb`:

{% irb %}
puts "a" * 10
=> "aaaaaaaaaa"
{% endirb %}

## Iteration 2: A Better Interface

Point out that every time they want to change their tweet, they have to edit their Ruby file. Ask the students for suggestions on how they can make their programs better. Explain how an interactive prompt interface (like `irb`) would be better.

#### Step 0 - Writing the while loop

Underneath the `tweet` method we'll use a `while` loop to repeat instructions over and over. Replace the last lines of the `twitter_bot.rb` as such:

```ruby
def tweet(message)
  client = JumpstartAuth.twitter
  client.update(message)
end

begin
  command = ""
  puts "Please enter a command: "
end while command != "quit"
```

Have the students run their programs and ask what happens and why. Ask them how they can break out of their program.

#### Step 1 - Getting user input

Students have already seen the `gets` command in their first week. If they can't remember how to get user input, have them look at their Hi-Lo game and their Rock, Paper, Scissors game for hints. They should modify their while loops to look as follows:

```ruby
begin
  command = gets.chomp
  puts "Please enter a command: "
end while command != "q"
```

Have the students run their programs to ensure that their code is in a working state. Ask them what happens this time and why the program is different.

Ask students to explain why we need to call the `chomp` method on the gets. Remind students that `gets` picks up the enter key too. So the `command` variable is actually getting the string `"q\n"` where that backslash-n is a new line. The `chomp` method will remove any whitespace (spaces, tabs, newlines) on the back end of a string.

#### Step 2 - Building if/else statements

We think we're getting the instruction from the user, but we need to actually do something with it. We can use if/else statements to process the commands. Work with the students to come up with a simple structure to handle the q command:

```ruby
if command == "q"
  puts "Goodbye!"
else
  puts "Sorry, I don't know how to #{command}"
end
```

Have the students run their programs and test some commands.

#### Step 3 - Tweeting & Parameters

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

You should see output like `Sorry, I don't know how to (t This is only a test!)`. I wanted it to call the `tweet` method because I started the line with `t`, but then the rest of the line was my message. Instead, it thought the whole line was the command. We need to divide up the input between the command and the text that should be sent to that command.

##### Making the Command-Line Interface Smarter

The line `command = gets.chomp` is kind of telling a lie. It isn't just getting the `command`, it's getting a `command` and a message to send to that command. Lets change this line to `input = gets.chomp` then we'll work with `input` to pull out the command.

Now that we have `input` we need to split it up into pieces. We'll cut it up using the `split` method. Just below the `input = gets.chomp` add a line that says `parts = input.split(" ")` to chop `input` into `parts`.

`split` will take our input string (entered by the user at the
command line) and chop it into an array of smaller strings based on the given parameter.

For example, if the user gave the input: `t tweet my message`

Our `input = gets.chomp` would produce a `parts` array that looked like this: `["t", "tweet", "my", "message"]`

Knowing the structure of this array will allow us to pull out the
parts we need at various places in our program. In the example `parts` array above, `t` is the command we're looking for, so let's pull it out by saying `command = parts[0]`.

Then what do we do with the rest of the `parts`?  They're our message. Our `tweet` method is expecting to be passed in a message, so we need to reassemble the message and add it to our `when` line. In order to get the whole message I'll grab `parts[1..-1]` which gives me all the words in the message from index 1 to the end of the array (-1). That basically just skips the `command` that's in `parts[0]`.

But those `parts[1..-1]` are individual word strings, I need to join them into a single string. We can use the `join` method and tell it to connect the words with a space like this: `parts[1..-1].join(" ")`. Using that idea in the `when` line for `t`, here's what my method looks like right now:

```ruby
  def run
    command = ""
    while command != "q"
      puts ""
      printf "enter command: "
      input = gets.chomp
      parts = input.split
      command = parts[0]
      case command
         when 'q' then puts "Goodbye!"
         when 't' then tweet(parts[1..-1].join(" "))
         else
           puts "Sorry, I don't know how to (#{command})"
      end
    end
  end
```

Try it out and you should finally be able to post tweets over and over!

## Iteration 3: Send Direct Messages

Sending Direct Messages isn't that different from posting a tweet. In fact, we can reuse our existing `tweet` method to do all the hard work.

#### Step 0 - Frameworks

```ruby
def dm(target, message)
  puts "Trying to send #{target} this direct message:"
  puts message
end
```

And we need to add the command to our `run` method. We'll enter the instruction like `dm jumpstartlab Here is the text of the DM`, so our `when` line should look like this:

```ruby
  when 'dm' then dm(parts[1], parts[2..-1].join(" "))
```

Remember that `parts[0]` is the command itself, here `dm`. Then `parts[1]` will be the target username, here `jumpstartlab`. Then everything else is the message, so we join them with spaces `parts[2..-1].join(" ")`.

#### Step 1 - Create and Send the Message

First, inside your `dm` method, create a new string that is a combination of "d", a space, the target username, a space, then the message.

Then call the `tweet` method with this new string as the parameter message.

Try sending a DM to your personal Twitter account. Try sending yourself a DM. If the DM doesn't show up it is probably because you can only send DMs to people who are following you. Start following your `your_testing_account_username` account from your personal account and try it again.

#### Step 2 - Error Checking DM-ability

You can only DM people who are following you. As you saw in Step 1, if you try and DM someone else, though, it doesn't give you an error message. It just fails silently.

Let's add a way to verify that the target is following you before sending the message. In pseudo-code, it'd go something like this:

* Find the list of my followers
* If the target is in this list, send the DM
* Otherwise, print an error message

We can call `@client.followers` which gives us back a list of all our followers but includes lots of information we don't need right now like their follower count, web address, last tweet. All we want is to find their `screen_name`.

What we need to do is pull out just the `screen_name`. We create an array of the followers' screen names with this line of code:

```ruby
screen_names = @client.followers.collect{|follower| follower.screen_name}
```

To read this line out loud it would be like "Call the `followers` method of `@client`, then take that array and, for each element in the array, `collect` together the value of `screen_name`.

Now you have an array of your followers' screen names. Create a conditional block that follows this logic:

* If the `target` username is in the `screen_names` list (use [Array#include?](http://rubydoc.info/stdlib/core/Array#include%3F-instance_method) method), then send the DM
* Otherwise, print out an error saying that you can only DM people who follow you

Test your code by sending a DM to someone who does follow your demo account and someone who does not.

#### Step 3 - Spamming Your Friends

It would be cool to be able to send the same message out to all of our followers. We'll accomplish this in two parts:

* Create a method named `followers_list` that returns an array containing the usernames of all my followers
* Create a method named `spam_my_followers` that finds all followers from `followers_list` and tries to send them a Direct Message using the `dm` method

To create the `followers_list` method...

* Define the method named `followers_list` with no parameters
* Create a blank array named `screen_names`
* On the `@client` call the `followers` method then the `users` method and iterate through `each` of them performing the instruction below:

```ruby
  screen_names << follower["screen_name"]
```

* Return the array `screen_names`

Then for the `spam_my_followers` method...

* Define the method named `spam_my_followers` with a parameter named `message`
* Get the list of your followers from the `followers_list` method
* Iterate through `each` of those followers and use your `dm` method to send them the `message`

Then create a `when` line in your `run` method for the command `spam`. It will look just like the `tweet` line, except it'll send the message into the `spam_my_friends` method.

Test it out and see how many followers you can annoy at once!