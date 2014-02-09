---
layout: page
title: Messengr
sidebar: true
---

In this project, students will learn how to build a chat room in the terminal.

Learning Goals:

* Understand the basics of HTTP requests (GET, POST)
* Learn how to use Ruby to interact with the web
* Reinforce fundamental programming concepts like while loops, conditional logic, and debugging

## Iteration 0: Up & running

Install the `httparty` gem by running `gem install httparty` from the command prompt:

{% terminal %}
$ gem install httparty
{% endterminal %}

We will be using httparty to get and post messages to the custom CodeNow messaging service which is located at [messengr.herokuapp.com](http://messengr.herokuapp.com). If you'd like to make changes to the repo, you can do so [here](https://github.com/CodeNowOrg/messengr).

<div class="note">
<p>You will want to clear out all existing messages prior to starting this exercise. The control panel to do so lives <a href="http://messengr.herokuapp.com/messages">here</a>.</p>
</div>

## Iteration 1: Receiving messages

### Step 0: Learning about GET requests

We're going to have to teach the concept of a GET request to students. Using a browser, have the students visit [http://messengr.herokuapp.com/](http://messengr.herokuapp.com/). Have them inspect the source code and explain to the students what HTML code is. Explain that when they are typing a URL into a browser, they are making a GET request to retreive HTML code.

Next, we're going to load the page in our command prompts. In irb, execute the following code.

{% irb %}
$ require 'httparty'
=> true
$ response = HTTParty.get("http://messengr.herokuapp.com")
=> #<HTTParty::Response:0x7fd049a38e20 parsed_response="<!DOCTYPE html>...
$ response.parsed_response
=> "<!DOCTYPE html>\n<html>\n<head>\n  <title>Chattr</title>\n..."
{% endirb %}

Have them closely inspect the code and look for the text "WELCOME TO MESSENGR!" to prove that the browser is, in fact, downloading this HTML code.

### Step 1: Hitting an API in the browser

Next, have them hit the following API endpoint [http://messengr.herokuapp.com/messages.json](http://messengr.herokuapp.com/messages.json) in their browsers. If done correctly, they should see a raw JSON string containing the contents of a single message (it won't look as nice):

{% img center /images/projects/messengr/messages_json.png %}

Explain to the students what an API does and why it is useful. Tell the students that they've actually already interacted with an API in the [TwitterBot](http://codenoworg.github.io/projects/twitter_bot.html) exercise. Twitter's API allowed us to post messages to our twitter streams as well as send direct messages to our followers.

### Step 2: Hitting an API in the command prompt

Next, have them hit the same API endpoint in irb. HTTParty makes it easy to send GET requests via the class method `.get`.

{% irb %}
$ require 'httparty'
=> true
$ response = HTTParty.get("http://messengr.herokuapp.com/messages.json")
=> #<HTTParty::Response:0x7fd049a93988 parsed_response=[{"id"=>28, "text"=>"Welcome to Messengr!"...
$ response.parsed_response
=> [{"id"=>28, "text"=>"Welcome to Messengr!", "room_id"=>nil, "user"=>"edweng", "created_at"=>"2014-01-19T23:01:25.998Z", "updated_at"=>"2014-01-19T23:01:25.998Z"}]
{% endirb %}

Ask the students what data type the parsed response is returning (spoiler: an array of hashes). Ask the students to list off the keys inside the hash.

### Step 3: Writing the reader script

Have the students create a file called `messengr_reader.rb`. Work together to write a method called `get_new_messages` that will be responsible for sending a GET request to the API endpoint and that returns an array of messages. Once completed, it should look something like this:

```ruby
require 'httparty'

def get_new_messages
  response = HTTParty.get("http://messengr.herokuapp.com/messages.json")
  return response.parsed_response
end

puts get_new_messages.inspect
```

## Iteration 2: Sending messages

### Step 0: Learning about POST requests

Next, explain to the students that POST requests are used to send information to a server. Give them some examples of POST requests that they will be familiar with (e.g., sending status updates in Facebook, posting Tweets on Twitter, uploading images on Instagram, filling out your shipping address on Amazon, etc.). Make it clear that POST interactions are typically completed in a browser via a "submit" button, which is different behavior from a GET.

Have the class attempt to post to an endpoint via the command prompt. HTTParty makes it easy to send POST requests via the class method `.post`.

{% irb %}
$ require 'httparty'
=> true
$ response = HTTParty.post("http://messengr.herokuapp.com/")
=> #<HTTParty::Response:0x7fd049cf83b8 parsed_response={"error"=>"Color parameter required.", "status"=>304}, @response=#<Net::HTTPOK 200 OK  readbody=true>, @headers={"x-frame-options"=>["SAMEORIGIN"], "x-xss-protection"=>["1; mode=block"], "x-content-type-options"=>["nosniff"], "x-ua-compatible"=>["chrome=1"], "content-type"=>["application/json; charset=utf-8"], "etag"=>["\"6203cc9bce6069b967018522de122df6\""], "cache-control"=>["max-age=0, private, must-revalidate"], "x-request-id"=>["978ceb14-c626-467a-860d-4bdbeb0e26eb"], "x-runtime"=>["0.018250"], "server"=>["WEBrick/1.3.1 (Ruby/2.0.0/2013-11-22)"], "date"=>["Mon, 20 Jan 2014 00:36:46 GMT"], "content-length"=>["50"], "connection"=>["close"]}>
$ response.parsed_response
=> {"error"=>"Color parameter can't be blank."}
{% endirb %}

Ask the students what data type the parsed response is (spoiler: a hash). Ask them what they think the result means and what they'll need to do in order to receive a successful response from the server.

### Step 1: Sending query parameters

POST requests generally include some kind of information that you want to "post" or "send" to the server. The `.post` method can take a second parameter, a hash, with a key called `:query`. This `:query` key points to a value that is a hash of parameters.

{% irb %}
$ require 'httparty'
=> true
$ response = HTTParty.post("http://messengr.herokuapp.com/", { :query => { :color => "green" } })
=> #<HTTParty::Response:0x7fd04dfb9918 parsed_response={"success"=>"I like green too!"}, @response=#<Net::HTTPOK 200 OK readbody=true>, @headers={"cache-control"=>["max-age=0, private, must-revalidate"], "content-type"=>["application/json; charset=utf-8"], "date"=>["Mon, 20 Jan 2014 00:53:46 GMT"], "etag"=>["\"37fd1df3bb9b0324a5ebcf4c7d1ab154\""], "status"=>["200 OK"], "x-content-type-options"=>["nosniff"], "x-frame-options"=>["SAMEORIGIN"], "x-request-id"=>["c454eea8-c22b-4031-95bb-391d05432cb5"], "x-runtime"=>["0.089777"], "x-ua-compatible"=>["chrome=1"], "x-xss-protection"=>["1; mode=block"], "transfer-encoding"=>["chunked"], "connection"=>["Close"]}>
$ response.parsed_response
=> {"success"=>"I like green too!"}
{% endirb %}

### Step 2: Writing the writer script

Have the students create a file called `messengr_writer.rb`. Have the students set up their file as follows:

```ruby
require 'httparty'

@user = "ENTER NAME OF STUDENT HERE"
```

Work together to write a method called `send_message` that takes an argument and will be responsible for sending a POST request to the API endpoint. Have the students post to the [http://messengr.herokuapp.com/messages.json](http://messengr.herokuapp.com/messages.json) without any parameters. Let them deduce how the query should be structured. Once completed, it should look something like this:

```ruby
require 'httparty'

@user = "edweng"

def send_message(message)
  response = HTTParty.post("http://messengr.herokuapp.com/messages.json", :query => { :user => @user, :text => message })
  puts response.parsed_response.inspect
  puts "ERROR: #{response.parsed_response['errors']}" if response.parsed_response["errors"]
end

send_message("What does the fox say?")
```

Have the students test out the writer script by running it with `ruby messengr_writer.rb` in their command prompt windows. Ask the students what they would change in order to make their script better. Explain why we should modify our programs to accept user input from the command line.

### Step 3: Accepting user input

Students should already be familiar with the `gets` method from the [Hi-Lo Game](http://codenoworg.github.io/projects/hilo.html#iteration-0:-up-&-running). Do a refresher exercise if needed to demonstrate that the `gets` command returns a string, why we need to call `.chomp`, and how we can save the user input into a variable.

Have the students modify their programs to prompt the user to input a message and then send that message to the server. Their scripts should look something like this:

```ruby
require 'httparty'

@user = "edweng"

def send_message(message)
  response = HTTParty.post("http://messengr.herokuapp.com/messages.json", :query => { :user => @user, :text => message })
  puts response.parsed_response.inspect
  puts "ERROR: #{response.parsed_response['errors']}" if response.parsed_response["errors"]
end

message = gets.chomp
send_message(message)
```

Ask the students how we can prove that they are successfully sending messages up to the server. Hopefully, someone will suggest we can use our messengr_reader script to verify! Have them execute their reader scripts and verify they can find their messages.

## Iteration 3: Looping it

### Step 0: Using `while` loops in messengr_writer

Our scripts work great, but they could be even better if we didn't have to run the program every time we wanted to send a message. To get around this, we'll want to modify our messengr_writer script to run in a while loop. Start off with a while loop that runs forever. Students should already be familiar with the while loop, but run through an exercise with them to make sure they understand the structure.

The syntax we've been using throughout the CodeNow course is as follows:

```ruby
begin
  print rand(2)
end while true
```

While unconventional, this ensures that all variables in the condition are present and will prevent us from having to initialize variables.

The while loop should look as follows:

```ruby
require 'httparty'

@user = "edweng"

def send_message(message)
  response = HTTParty.post("http://messengr.herokuapp.com/messages.json", :query => { :user => @user, :text => message })
  puts response.parsed_response.inspect
  puts "ERROR: #{response.parsed_response['errors']}" if response.parsed_response["errors"]
end

begin
  print "Say something: "
  message = gets.chomp
  response = send_message(message)
end while true
```

Have students run their messengr_writer scripts and confirm using their messengr_reader scripts that messages are being posted.

### Step 1: Conditioning the `while` loop

Next, we want to build in functionality such that if the user types in 'quit', our writer script stops looping. Have them modify their programs to incorporate this feature.

```ruby
begin
  print "Say something: "
  message = gets.chomp
  if message != "quit"
    response = send_message(message)
  end
end while message != "quit"
```

### Step 2: Using `while` loops in messengr_reader

We'll also want to use a while loop in our messengr_reader program so that we can continue to receive new messages that appear in the chat room. Have the students add a while loop that repeats indefinitely.

```ruby
require 'httparty'

def get_new_messages
  response = HTTParty.get("http://messengr.herokuapp.com/messages.json")
  return response.parsed_response
end

begin
  puts get_new_messages.inspect
end while true
```

The problem with the above code is that it will spawn a ton of requests to the server and we'll essentially DDoS ourselves. Explain to the students how DDoS work and have the students add in a `sleep` call to slow down the amount of GET requests.

```ruby
require 'httparty'

def get_new_messages
  response = HTTParty.get("http://messengr.herokuapp.com/messages.json")
  return response.parsed_response
end

begin
  puts get_new_messages.inspect
  sleep(5)
end while true
```

The second problem with the code is that it pulls messages that we have already seen. Tell the student that there's a parameter called `:last_message_id` that the API accepts. Ask them what they think that parameter does. Have them experiment with that parameter in IRB.

{% irb %}
$ require 'httparty'
=> true
$ last_message_id = 27 # this id will be different for each class
$ response = HTTParty.get("http://messengr.herokuapp.com/messages.json", :query => { :last_message_id => last_message_id })
=> #<HTTParty::Response:0x7fd049a93988 parsed_response=[{"id"=>28, "text"=>"Welcome to Messengr!"...
$ response.parsed_response
=> [{"id"=>28, "text"=>"Welcome to Messengr!", "room_id"=>nil, "user"=>"edweng", "created_at"=>"2014-01-19T23:01:25.998Z", "updated_at"=>"2014-01-19T23:01:25.998Z"}]
{% endirb %}

We're going to have to modify our `get_new_messages` method to accept an argument `last_message_id`. If they get stuck, have students refer to their implementation of send_message to figure out how they change the method's signature to accept an argument.

When finished, the `get_new_messages` method should look something like this:

```ruby
def get_new_messages(last_message_id)
  response = HTTParty.get("http://messengr.herokuapp.com/messages.json", :query => { :last_message_id => last_message_id })
  return response.parsed_response
end
```

Next, the while loop has to be modified to call our new `get_new_messages` method. Have the students work on modifying their method in messengr_reader such that the last_message_id variable is set in their `while` loops. Run through a refresher with students on how you can access the `id` key inside their message hash. Once modified, the `while` loop of the code should appear as follows:

```ruby
last_message_id = 0
begin
  messages = get_new_messages(last_message_id)
  if messages.length > 0
    last_message_id = messages.last["id"]
    puts messages.inspect
  end
  sleep(5)
end while true
```

### Step 3: Testing everything out

Have students open up a second command prompt window. Have one window be their writing client and have the other window be their reading client. Have them send messages between themselves and make sure that their functionality is working.

{% img center /images/projects/messengr/chat_setup.png %}

## Iteration 4: Prettifying the message format

So far, we've just been printing out the inspected version of the API response. What we're receiving back is an array of message hashes. It would be cool if we could make our messages a little bit prettier. Ask the students which portion of their code we will need to modify in order to make reading messages easier.

### Step 0: Nested loops: Looping through each of the messages

In order to make things look nice, we'll have to work with individual messages. This will require us to use the `each` loop. In the past, students have struggled with grasping the concept of an `each` loop, so make sure you give them plenty of assistance with setting it up and explaining how it works.

The while loop in the `messengr_reader` script should look as follows:

```ruby
last_message_id = 0
begin
  messages = get_new_messages(last_message_id)
  if messages.length > 0
    last_message_id = messages.last["id"]
    messages.each do |message|
      puts message.inspect
    end
  end
  sleep(5)
end while true
```

### Step 1: Printing out the basics

We want to start off simply by printing out the user key and the text key. Students should be familiar with string interpolation and should be able to come up with something that looks as follows:

```ruby
last_message_id = 0
begin
  messages = get_new_messages(last_message_id)
  if messages.length > 0
    last_message_id = messages.last["id"]
    messages.each do |message|
      puts "#{message['user']}: #{message['text']}"
    end
  end
  sleep(5)
end while true
```

Students should see their formatted messages look as follows:

{% irb %}
ruby messengr_reader.rb
edweng: Welcome to Messengr!
edweng: Hello, world!
{% endirb %}

### Step 2: Formatting a time

One feature that might be nice to have message timestamps so that we can determine when a message was last sent.

If we want to format a time to be a human readable string, Ruby provides us with convenience methods to do that. In IRB, using the [Ruby docs](http://apidock.com/ruby/DateTime/strftime) try out a few different ways to format a Time object.

{% irb %}
$ Time.now
=> 2014-02-02 19:29:11 UTC
$ Time.now.getlocal
=> 2014-02-02 15:18:10 -0500
$ Time.now.getlocal.strftime("%m/%d/%Y")
=> "2/2/2014"
$ Time.now.getlocal.strftime("%H:%M")
=> "15:18"
{% endirb %}

When we get the message object back from the API, we're dealing with a string, not a Time object. Show the students the difference between a Time object and a Time string. Then explain to them that in order to use the `strftime` method, we'll need to first parse the time out and then format it.

{% irb %}
$ require 'httparty'
=> true
$ last_message_id = 27 # this id will be different for each class
$ response = HTTParty.get("http://messengr.herokuapp.com/messages.json", :query => { :last_message_id => last_message_id })
=> #<HTTParty::Response:0x7fd049a93988 parsed_response=[{"id"=>28, "text"=>"Welcome to Messengr!"...
$ messages = response.parsed_response
=> [{"id"=>28, "text"=>"Welcome to Messengr!", "room_id"=>nil, "user"=>"edweng", "created_at"=>"2014-01-19T23:01:25.998Z", "updated_at"=>"2014-01-19T23:01:25.998Z"}]
$ message = messages.first
=> {"id"=>28, "text"=>"Welcome to Messengr!", "room_id"=>nil, "user"=>"edweng", "created_at"=>"2014-01-19T23:01:25.998Z", "updated_at"=>"2014-01-19T23:01:25.998Z"}
$ message["created_at"]
=> "2014-01-19T23:01:25.998Z"
$ Time.parse(message["created_at"])
=> 2014-02-02 19:29:11 UTC
$ Time.parse(message["created_at"]).getlocal.strftime("%H:%M")
=> "14:29"
{% endirb %}

Have the students incorporate this into their puts statment.

```ruby
last_message_id = 0
begin
  messages = get_new_messages(last_message_id)
  if messages.length > 0
    last_message_id = messages.last["id"]
    messages.each do |message|
      puts "#{Time.parse(message['created_at']).getlocal.strftime('%H:%M')} #{message['user']}: #{message['text']}"
    end
  end
  sleep(5)
end while true
```

Students should see their formatted messages look as follows:

{% irb %}
ruby messengr_reader.rb
14:29 edweng: Welcome to Messengr!
15:03 edweng: Hello, world!
{% endirb %}

## Extensions

Here are a list of extensions that students can build if they finish early:
- Refactor the puts statement in `messengr_reader.rb` into its own `print_message(message)` method
- Have the `messengr_writer` announce when a user connects and disconnects from a room
- Experiment with the room parameter in the API