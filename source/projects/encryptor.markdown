---
layout: page
title: Encryptor
sidebar: true
---

In this project, students will begin by building a procedural version of an encryption engine and then advance to modularizing their code with the introduction of methods.

Learning Goals:

* Practice the fundamentals of Ruby including setting variables, getting user input, printing messages
* Exposure to more advance Ruby fundamentals: hashes and arrays
* Introduction to methods
* Introduction to iteration (.each)

<div class="note">
<p>The design decisions in the following code are intentional to help absolute beginners quickly learn the basics without overwhelming them with intermediate concepts.</p>
</div>

<div class="note">
<p>This tutorial is open source. If you notice errors, typos, or have questions/suggestions, please <a href="https://github.com/CodeNowOrg/codenoworg.github.io">submit them to the project on Github</a>.</p>
</div>

## Iteration 0: Up & running

Have the students create a new file called `encryptor.rb` in a codenow directory.

{% terminal %}
touch encryptor.rb
{% endterminal %}

To open the file inside SublimeText, we can simply type the filename into the command prompt. If it has not already been set as the default text editor for ruby files, make sure you set it.

{% terminal %}
encryptor.rb
{% endterminal %}

## Iteration 1: Creating the base encryptor

### Step 1 - Understanding the task

Tell the students they will be building an encryption engine where they will be able to pass a string of plain text into their program and the output will be an encrypted string.  After successfully encrypting, students will be able to decrypt the masked word and reveal its original value.

As always, we will be starting with the simplest thing that could work and then begin to refactor.  Keep in mind that a major goal of this exercise is to show the need / usefulness of methods.

### Step 2 - Creating a plan with pseudocode

We'll be starting with just the encryption part first.

```ruby
  #create an encryption algorithm (steps to mask a word)

  #ask the user for a word

  #break the word up into a collection of its letters and save it to an array

  #create a new array called secret_word as a placeholder to store each encrypted letter

  #iterate thru each letter in word

  #lookup the letters encrypted value

  #store the encrypted value in secret_word array

  #join the secret_word array into one word and print to screen
```

Explain that the word algorithm is nothing to be intimidated by - it's just a list of steps a program takes to achieve a task.  Writing a how-to for making PB&J is an algorithm.

### Step 3 - Introduction to hashes

Speak to the fact that we will need some way to map one value to another value in order for us to create an encryption algorithm.  Start by showing how we could use an array of arrays.

Then speak to the fact that although it is feasible, it is not ideal for keeping track of the values.
Below is an example that could be used

```ruby
phone_contacts = [["tim", "443-797-3443"], ["kyle", "819-867-8888"], ["carl", "456-786-9898"]]
```
If you needed to call kyle, you would have to know that he was the second value in the array and that phone numbers are the second value in the nested array.  To retrieve you would have to do something like:

```ruby
  phone_contacts[1][1] #=> pretty unintuitive
```

There is a better way! Show them how hashes will make their lives better. Points to cover

1. explain key / value store
2. get/set values from a hash
3. stick with only showing strings as keys
4. show that it's possible to store an array or another hash as a value

```ruby
phone_contacts = {"tim" => "443-797-3663", "kyle" => "819-867-9786", "carl" => "687-978-5768" }

phone_contacts["kyle"] #=> "819-867-9786"
```

### Step 4 - Creating a basic encryption

For our encryption algorithm, we will using a ROT design. Where we simply rotate each letter in the alphabet by a set number of times.

ROT-2

* A - C (A is now C)
* B - D (B is now D)
* C - E (C is now E)

```ruby
{"a"=>"c", "b"=>"d", "c"=>"e", "d"=>"f", "e"=>"g",
"f"=>"h", "g"=>"i", "h"=>"j", "i"=>"k", "j"=>"l",
"k"=>"m", "l"=>"n", "m"=>"o", "n"=>"p", "o"=>"q",
"p"=>"r", "q"=>"s", "r"=>"t", "s"=>"u", "t"=>"v",
"u"=>"w", "v"=>"x", "w"=>"y", "x"=>"z", "y"=>"a", "z"=>"b"}
```

### Step 5 - Asking for input

The students should already know how to do this.  Remind them that they can open their rps.rb and hilo.rb files to see how they did it in the past if they cannot remember.

```ruby
 plain_word = gets.chomp
```

### Step 6 - Writing the encrypting logic

Show students how they can encrypt a single letter using the encryption hash.

```ruby
encryption["a"] => "c"
```

But, a word is made up of many letters.  We need a way to break a word into its individual letters so that we can encrypt each one.  Show the students split method in irb.


{% terminal %}
"dog".split("")
  #=> ["d", "o", "g"]
"d-o-g".split("-")
  #=> ["d", "o", "g"]
"d$o$g".split("$")
  #=> ["d", "o", "g"]
{% endterminal %}

Now, that students have an array of letters, they need an introduction to iteration.  Be brief here.  Just enough to get them by.  We don't want to overwhelm them.  A good example is show a countdown.

```ruby
[1,2,3,4,5,6,7,8,9,10].each do |number|
  puts "#{number}"
  sleep 1
end

 # more verbose example

numbers = [1,2,3,4,5,6,7,8,9,10]
numbers.each do |number|
  puts "at the top of the loop"
  puts "currently on number: #{number}"
  puts "#{number}" multiplied by 2 is "#{number * 2}"
  puts "- - - -"
  puts ""
  sleep 1
end
```

Now, apply same concept to iterating thru the letters array.

### Step 7 - Saving the encrypted value and outputting it

The students need a way to store their newly encrypted values.  The ultimate goal is to have each encrypted value added to a new array which will be joined as a string to output.

Show them a very similar example

```ruby
names = ["tim", "kim", "beverly"]
names_capitalized = []

names.each do |name|
  names_capitalized << name.capitalized
end

puts names_capitalized
```

Let the students work on their own to create an array of encrypted letters.

After 10 minutes is given to solve, show them the joins method and note how it is opposite of split.

{% terminal %}
["d", "o", "g"].join("")
  #=> "dog"
["d", "o", "g"].join("-")
  #=> "d-o-g"
["d", "o", "g"].join("$")
  #=> "d$o$g"
{% endterminal %}

Student's file should look similar to below...

```ruby
  #create an encryption algorithm (steps to mask a word)
  encryption = {"a"=>"c", "b"=>"d", "c"=>"e", "d"=>"f", "e"=>"g",
                "f"=>"h", "g"=>"i", "h"=>"j", "i"=>"k", "j"=>"l",
                "k"=>"m", "l"=>"n", "m"=>"o", "n"=>"p", "o"=>"q",
                "p"=>"r", "q"=>"s", "r"=>"t", "s"=>"u", "t"=>"v",
                "u"=>"w", "v"=>"x", "w"=>"y", "x"=>"z", "y"=>"a", "z"=>"b"}

  #ask the user for a word
  plain_word = gets.chomp

  #break the word up into a collection of its letters and save it to an array
  letters = plain_word.split("")

  #create a new array called secret_word as a placeholder to store each encrypted letter
  secret_word = []

  #iterate thru each letter in word
  letters.each do |letter|
    #lookup the letters encrypted value
    encrypted_letter = encryption[letter]

    #store the encrypted value in secret_word array
    secret_word << encrypted_letter
  end

  #join the secret_word array into one word and print to screen
  puts secret_word.join("")
```
## Iteration 2: Creating the base decryptor

### Step 1 - Decrypting

Now that we have successfully encrypted, we need to solve the other half of the problem - decryption.  Let the student's know that a lot of the logic to solve decryption can be copied from what they have already done with encrypting.

The biggest hurdle is flip flopping the keys and values of the encryption hash.  Have the students realize that if you do any ROT other than 13, decryption will fail because the new letter value doesn't map back to its original value.  Luckily, much time doesn't need to be spent solving it as we will be using ruby's invert method.

```ruby
{"a" => "c", "b" => "d", "c" => "e"}.invert
 #=> {"c" => "a", "d" => "b", "e" => "c"}
```

Create the following pseudocode with the students and then set them free to solve the decryption

```ruby
  #create a decryption hash

  #break the secret_word up into its letters

  #create a new array called decoded_word as a placeholder to store each decrypted letter

  #iterate thru each letter
    #find the letter's decrypted value

    #store the decrypted value in decoded_word array

  #join the decoded_word array into a string and store its value

  #show final output
    #compare decoded_word to original word

    #print string that says the word and its encrypted value
```

Give the students a full pomodoro to solve

### Step 2 - Complete basic implementation

The final program file should look like the following...

```ruby
  #create an encryption algorithm (steps to mask a word)
  encryption = {"a"=>"c", "b"=>"d", "c"=>"e", "d"=>"f", "e"=>"g",
                "f"=>"h", "g"=>"i", "h"=>"j", "i"=>"k", "j"=>"l",
                "k"=>"m", "l"=>"n", "m"=>"o", "n"=>"p", "o"=>"q",
                "p"=>"r", "q"=>"s", "r"=>"t", "s"=>"u", "t"=>"v",
                "u"=>"w", "v"=>"x", "w"=>"y", "x"=>"z", "y"=>"a", "z"=>"b"}

  #ask the user for a word
  plain_word = gets.chomp

  #break the word up into a collection of its letters and save it to an array
  letters = plain_word.split("")

  #create a new array called secret_word as a placeholder to store each encrypted letter
  secret_word = []

  #iterate thru each letter in word
  letters.each do |letter|
    #lookup the letters encrypted value
    encrypted_letter = encryption[letter]

    #store the encrypted value in secret_word array
    secret_word << encrypted_letter
  end

  #join the secret_word array into one word and print to screen
  #no need to print word here for final program output

  #create a decryption hash
  decryption = encryption.invert

  #break the secret_word up into its letters
  letters = secret_word.split("")

  #create a new array called decoded_word as a placeholder to store each decrypted letter
  decoded_word = []

  #iterate thru each letter
  letters.each do |letter|
    #find the letter's decrypted value
    decrypted_letter = decryption[letter]

    #store the decrypted value in decoded_word array
    decoded_word << decrypted_letter
  end

  #join the decoded_word array into a string and store its value
  decoded_word = decoded_word.join("")

  #show final output
    #print string that says the word and its encrypted value
    puts "The encrypted version of #{plain_word} is #{secret_word}"

    #compare decoded_word to original word
    if decoded_word == plain_word
      puts "Decryption was a success"
    else
      puts "Decryption was a failure"
    end
```

## Iteration 3: Making the encryption dynamic

### Step 1: Understanding the problem

Currently, our encryption algorithm is fixed at a ROT-2 sequence.  To improve the security of our encryptor, we should be able to easily change the ROT sequence to whatever we please.  We need to be able to build to encryption hash dynamically.

Outline the pseudocode

```ruby
  #create an array of the alphabet called characters

  #create another array which rotates the alphabet array some value (under 23) called rotated_characters

  #merge characters array and rotated_characters array into a single hash such that characters array are the keys and rotated_characters array are the values
```

### Step 2: Introduction to rotate method

Ruby has method called rotate which can be called on an array.  It takes a number as an argument, which tells the array how many positions to rotate each element. Hop in irb to demo.

{% terminal %}
["a", "-", "-"].rotate(1)
  #=> ["-", "-", "a"]
["a", "-", "-"].rotate(2)
  #=> ["-", "a", "-"]
["a", "-", "-"].rotate(3)
  #=> ["a", "-", "-"]
{% endterminal %}

### Step 3: Introduction to Ruby ranges

We are showing ranges simply as a convenient way to generate an array of the alphabet.  Don't spend too much time explaining their nuances.  Just enough to get by.

{% terminal %}
("a".."z")
  #=> "a".."z"
("a".."z").class
  #=> Range
("a".."z").member?("a")
  #=> true
("a".."z").member?("c")
  #=> true
("a".."z").member?(5)
  #=> false
("a".."z").to_a
  #=> ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n",
       "o", "p", "q", "r", "s", "t", "u", "v", "w", "x", "y", "z"]
{% endterminal %}

### Step 4: Zipping it all up

The zip method on a array is a pretty complex method. You may have to do some hand waving here.  The goal is not for them to fully understand the zip method, but rather have a dynamically created encryption hash.

Explain the zip method does exactly like its name implies.  It takes two values and merges them together just like a zipper on a jacket.

{% terminal %}
names = ["tim", "kim", "jim"]
area_codes = ["443", "813", "975"]

zipped_values = names.zip(area_codes)
  #=> [["tim", "443"], ["kim", "813"], ["jim", "975"]]
{% endterminal %}

Hash has a class method ``` [] ``` that takes an array of arrays

{% terminal %}
zipped_values = [["tim", "443"], ["kim", "813"], ["jim", "975"]]

Hash[zipped_values]
  #=> {"tim"=>"443", "kim"=>"813", "jim"=>"975"}
{% endterminal %}

The students should now be able to complete the pseudocode.  Give them a full pomodoro.  Final code should look similar...

```ruby
  #create an array of the alphabet called characters
  characters = ("a".."z").to_a

  #create another array which rotates the alphabet array some value (under 23) called rotated_characters
  rotated_characters = characters.rotate(10)

  #merge characters array and rotated_characters array into a single hash such that characters array are the keys and rotated_characters array are the values
  zipped_values = characters.zip(rotated_characters)
  encryption = Hash[zipped_values]
```

### Step 5: Updating the base encryptor

Students will need to pass in the rotation value in the beginning of their program.

```ruby
  #ask the user for the rotation value
  rotation = gets.chomp.to_i

  #create an encryption algorithm (steps to mask a word)
    #create an array of the alphabet called characters
    characters = ("a".."z").to_a

    #create another array which rotates the alphabet array some value (under 23) called rotated_characters
    rotated_characters = characters.rotate(rotation)

    #merge characters array and rotated_characters array into a single hash such that characters array are the keys and rotated_characters array are the values
    zipped_values = characters.zip(rotated_characters)
    encryption = Hash[zipped_values]

  #ask the user for a word
  plain_word = gets.chomp

  #break the word up into a collection of its letters and save it to an array
  letters = plain_word.split("")

  #etc... (rest of file omitted)
```

## Iteration 4: Refactor to use methods

### Step 1: Understanding the problem

Goal here is to illuminate why using methods to organize our code is a win. Let the students know that
as it stands now, our code works but is not very flexible.  What would happen if we needed to encrypt an entire a file of words?  Or, if our program grew longer and we needed to doing encryption later in our program - we would have to repeat our same steps.  Our programs would grow huge and be hard to follow.  We need a better way to organize our code so that we can give names to the logic that we are writing as well be able to reuse it.

### Step 2: Introduction to methods

Methods are a tough concept to learn.  The biggest blockers we have seen are the passing in of arguments and understanding return values.  As always, the goal here is to teach them enough to get by.
Talking points for covering methods

1. breakdown the syntax (def, name, args, body, end)
2. show the concept of return values by explicitly using the ruby keyword 'return'
3. show how args get passed and are like local variables

I find the below example to be helpful, use your own if you would like. The drawback is it uses ```puts``` which confuses the issue of using explicit ```return```.

Ask the kids to give you the steps for making pie

```ruby
 #here is our pie creating logic
puts "how to make apple pie"
puts "first: mix sugar with the apples"
puts "second: pour mixture into a crust"
puts "third: bake at 350 degrees for 40 minutes"
puts "fourth: let cool, then enjoy"
```

Now if we wanted to make our pie logic dynamic so that we could make any type of fruit pie would use variables.  Our new variables would be fruit, oven_temp, and cook_time

```ruby
 #our my dynamic pie logic
fruit = "apple"
oven_temp = 350
cook_time = 40

puts "how to make #{fruit} pie"
puts "first: mix sugar with the #{fruit}s"
puts "second: pour mixture into a crust"
puts "third: bake at #{oven_temp} degrees for #{cook_time} minutes"
puts "fourth: let cool, then enjoy"
```

Now, to wrap our pie making into a method

```ruby
def make_pie(fruit, oven_temp, cook_time)
  puts "how to make #{fruit} pie"
  puts "first: mix sugar with the #{fruit}s"
  puts "second: pour mixture into a crust"
  puts "third: bake at #{oven_temp} degrees for #{cook_time} minutes"
  puts "fourth: let cool, then enjoy"
end

 #now we can call our method whenever we want to make pie

 make_pie("apple", 350, 40)
 make_pie("pear", 375, 30)
 make_pie("blueberry", 400, 25)
```

The students will most likely need to see more than one example.  Here is an example to show explicit returns.

```ruby
 #wrapping the split logic up
def make_into_letters(word)
  return word.split("")
end

make_into_letters("dog") #=> ["d", "o", "g"]
```

###Step 3: Wrap the encryptor logic up into methods

Our current logic for the encryption hash

```ruby
  #create an encryption algorithm (steps to mask a word)
    #create an array of the alphabet called characters
    characters = ("a".."z").to_a

    #create another array which rotates the alphabet array some value (under 23) called rotated_characters
    rotated_characters = characters.rotate(rotation)

    #merge characters array and rotated_characters array into a single hash such that characters array are the keys and rotated_characters array are the values
    zipped_values = characters.zip(rotated_characters)
    encryption = Hash[zipped_values]
```

As a method...

```ruby
def encryption(rotation)
  characters = ("a".."z").to_a
  rotated_characters = characters.rotate(rotation)

  zipped_values = characters.zip(rotated_characters)
  return Hash[zipped_values]
end
```

Copy and past into irb to demo

{% terminal %}
encryption(3)
  #=> {"a"=>"d", "b"=>"e", "c"=>"f", "d"=>"g", "e"=>"h", "f"=>"i",
       "g"=>"j", "h"=>"k", "i"=>"l", "j"=>"m", "k"=>"n", "l"=>"o", "m"=>"p",
       "n"=>"q", "o"=>"r", "p"=>"s", "q"=>"t", "r"=>"u", "s"=>"v", "t"=>"w",
       "u"=>"x", "v"=>"y", "w"=>"z", "x"=>"a", "y"=>"b", "z"=>"c"}
{% endterminal %}

Work thru the encrypt and decrypt logic with the kids. Outline the goals with pseudocode

```ruby
  #wrap the encryption hash in a method called encryption that takes a rotation as an argument

  #wrap the decryption hash in a method called decryption that takes a rotation as an argument

  #wrap the encrypting logic in method called encrypt that takes word as an argument

  #wrap the decryption logic in method called decrypt that takes a secret_word as an argument

  #wrap the comparison logic for a decryption being successful in a method called decryption_successful? that takes the original word and decrypted word as arguments
```

Allow the kids a full pomodoro to work thru these steps.  Their code should look similar to the following...

```ruby
  #wrap the encryption hash in a method called encryption that takes a rotation as an argument
  def encryption(rotation)
    characters = ("a".."z").to_a
    rotated_characters = characters.rotate(rotation)

    zipped_values = characters.zip(rotated_characters)
    return Hash[zipped_values]
  end

  #wrap the decryption hash in a method called decryption that takes a rotation as an argument
  def decryption(rotation)
    characters = ("a".."z").to_a
    rotated_characters = characters.rotate(rotation)

    zipped_values = rotated_characters.zip(characters)
    return Hash[zipped_values]
  end

  #wrap the encrypting logic in method called encrypt that takes word as an argument
  def encrypt(word, rotation)
    letters = word.split("")
    secret_word = []

    letters.each do |letter|
      encrypted_letter = encryption(rotation)[letter]
      secret_word << encrypted_letter
    end

    return secret_word.join("")
  end

  #wrap the decryption logic in method called decrypt that takes a secret_word as an argument
  def decrypt(secret_word, rotation)
    letters = secret_word.split("")
    decoded_word = []

    letters.each do |letter|
      decrypted_letter = decryption(rotation)[letter]
      decoded_word << decrypted_letter
    end

    return decoded_word.join("")
  end

  #wrap the comparison logic for a decryption being successful in a method called decryption_successful? that takes the original word and decrypted word as arguments
  def decryption_successful?(word, decoded_word)
    if word == decoded_word
      return true
    else
      return false
    end
  end
```

Now to run the program now...

```ruby
puts "what word would you like to encrypt?"
word = gets.chomp

puts "what is your desired rotation?"
rotation = gets.chomp.to_i

encrypted_word = encrypt(word, rotation)
puts "The encrypted version of #{word} is #{encrypted_word}"
puts "running it thru the decryptor now..."
sleep 2

decrypted_word = decrypt(encrypted_word, rotation)
puts "Your original word was #{word}"
if decryption_successful?(word, decrypted_word)
  puts "Success: The decrypted version is #{decrypted_word}"
else
  puts "Failure: The decrypted version is #{decrypted_word}"
end
```

##Iteration 5: Extra Credit - The Decryption Engine

###Step 1: Understanding the task

Have the students partner with someone else.  Each partner should alternate who generates the secret word.  The partner generating the secret word should not tell the other student the rotation that he/she used.  They should only tell the other partner what the encrypted version of the word is.

The other partner should then run the encrypted word thru their decryption engine and then be able to tell the other partner what the decrypted word is and the rotation that the other student used.

Have the students first solve it procedurally then refactor to use a method

### Step 2: Outlining the pseudocode

```ruby
  #save the value of the sting given to you as secret_word

  #create a range of numbers from 0 to 22 and store as an array

  #iterate thru each number

  #decrypt the word and save it as decrypted_word

  #print the current rotation and the decrypted_word
```

Their code should look similar to...

```ruby
  #save the value of the sting given to you as secret_word
  secret_word = "sgpvdc"

  #create a range of numbers from 0 to 22 and store as an array
  rotations = (1..22).to_a

  #iterate thru each number
  rotations.each do |rotation|
    #decrypt the word and save it as decrypted_word
    decrypted_word = decrypt(secret_word, rotation)

    #print the current rotation and the decrypted_word
    puts "rotation: #{rotation} | decrypted word: #{decrypted_word}"
  end
```

### Step 3: Refactor to use the decryption_engine method

Now, have them wrap up this decryptin logic into a new method

```ruby
def decryption_engine(secret_word)
  rotations = (1..22).to_a
  rotations.each do |rotation|
    decrypted_word = decrypt(secret_word, rotation)
    puts "rotation: #{rotation} | decrypted word: #{decrypted_word}"
  end
end
```

Final game output

Student 1 (goes into irb and runs their encrypt method)
{% terminal %}
encrypt("dragon" 15)
  #=> "sgpvdc"
{% endterminal %}

Student 2 (runs encrypted method thru their decryption engine)
```ruby
decryption_engine("sgpvdc")
 #=>rotation: 0 | decrypted word: sgpvdc
    rotation: 1 | decrypted word: rfoucb
    rotation: 2 | decrypted word: qentba
    rotation: 3 | decrypted word: pdmsaz
    rotation: 4 | decrypted word: oclrzy
    rotation: 5 | decrypted word: nbkqyx
    rotation: 6 | decrypted word: majpxw
    rotation: 7 | decrypted word: lziowv
    rotation: 8 | decrypted word: kyhnvu
    rotation: 9 | decrypted word: jxgmut
    rotation: 10 | decrypted word: iwflts
    rotation: 11 | decrypted word: hveksr
    rotation: 12 | decrypted word: gudjrq
    rotation: 13 | decrypted word: ftciqp
    rotation: 14 | decrypted word: esbhpo
    rotation: 15 | decrypted word: dragon
    rotation: 16 | decrypted word: cqzfnm
    rotation: 17 | decrypted word: bpyeml
    rotation: 18 | decrypted word: aoxdlk
    rotation: 19 | decrypted word: znwckj
    rotation: 20 | decrypted word: ymvbji
    rotation: 21 | decrypted word: xluaih
    rotation: 22 | decrypted word: wktzhg
```

The student can now look the only word that is readable.
















