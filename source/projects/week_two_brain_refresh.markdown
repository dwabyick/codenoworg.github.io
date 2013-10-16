---
layout: page
title: Week 2 Brain Refresh
sidebar: false
---

Students will review the following concepts

* Variables
* Control Flow
* Boolean Logic

<div class="note">
<p>This tutorial is open source. If you notice errors, typos, or have questions/suggestions, please <a href="https://github.com/CodeNowOrg/codenoworg.github.io">submit them to the project on Github</a>.</p>
</div>

## Understanding the task

Have the students create a new file called ```hobbit.rb```.  The goal of the exercise is to create three boolean variables at the top of the file that determine which conditional gets run.  Run thru the program three times.  With each time, change the truthiness of the variables so that different outcomes occur.

The students do not need to copy the setup below.  The students file should just say the outputted statement.

### Round 1

This is what gets displayed on the projector

```ruby
sword = true
ring  = true
lucky = false

if !sword && lucky
  puts "got away with only my life"
elsif sword && ring && lucky
  puts "yes, I get the gold"
elsif sword && ring && !lucky
  puts "sneezed, dragon killed me"
elsif sword || ring || lucky
  puts "maybe going to die"
elsif ring && (sword || lucky)
  puts "maybe get the gold"
elsif !sword && !ring && !lucky
  puts "definitely going to die"
end
```

Here would be what the students have in their file after seeing the above

```ruby
  #round one
  "sneezed, dragon killed me"
```

### Round 2

Just change the top variables

```ruby
sword = true
ring  = false
lucky = false
```

Student's file

```ruby
  #round one
  "sneezed, dragon killed me"

  #round two
  "maybe going to die"
```

### Round 3

```ruby
sword = true
ring  = true
lucky = true
```

Student's file

```ruby
  #round one
  "sneezed, dragon killed me"

  #round two
  "maybe going to die"

  #round three
  "yes, I get the gold"
```

