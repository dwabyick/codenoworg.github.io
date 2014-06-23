---
layout: page
title: Setting Up SublimeText
sidebar: true
---

In this project, students will learn how to set up SublimeText on their Windows machines.

Learning Goals:

* Understand what SublimeText is
* Learn how to make SublimeText their default text editor
* Addding customizations to SublimeText

<div class="note">
<p>This tutorial is open source. If you notice errors, typos, or have questions/suggestions, please <a href="https://github.com/CodeNowOrg/codenoworg.github.io">submit them to the project on Github</a>.</p>
</div>

## Overview

Tell the students what a text editor is and why we need it for computer programming. Explain that like they use Microsoft Word to write their essays, programmers use SublimeText to write their code.

## Setting a default text editor

Students can get to the command prompt by hitting the windows key and typing "command prompt with ruby and rails." Once they hit enter, they should be brought to the command prompt that has paths for Rails and Ruby set in the environment.

<div class="note">
<p>Make sure students are opening "Command Prompt with Ruby and Rails." Many perplexing errors occur when students open the regular version of the Command Prompt. None of the typical commands will work.</p>
</div>

Have the students create a new ruby file called `hilo.rb` in a codenow directory. Then have them type in `hilo.rb` directly into the command prompt.

{% terminal %}
touch hilo.rb
hilo.rb
{% endterminal %}

If a default text editor has not yet been set for Ruby files, a window should open up:

{% img left /images/projects/sublime_text/select_program.png %}

Click "Select a program from a list of installed programs" and another window should open up:

{% img left /images/projects/sublime_text/select_program_2.png %}

Look inside Program Files/Sublime Text 2 for the sublime_text executable. Select it and click "Open":

{% img left /images/projects/sublime_text/open_with.png %}

You should then return to the program select screen with Sublime Text 2 highlighted. Make sure the "Always use the selected program to open this kind of file" is checked. This will set Sublime Text as the default text editor for all Ruby files going forward:

{% img left /images/projects/sublime_text/open_with_2.png %}

Once you click "OK", SublimeText should open up:

{% img left /images/projects/sublime_text/sublime_text.png %}

## Customizing SublimeText

From the SublimeText window, click "Preferences" on the toolbar:

{% img left /images/projects/sublime_text/sublime_text_preferences.png %}

Click on "Settings - User" which should bring you to a blank file. Paste in the following configuration.

```ruby
{
  "auto_indent": true,
  "auto_match_enabled" : true,
  "bold_folder_labels": true,
  "color_scheme": "Packages/Color Scheme - Default/Solarized (Dark).tmTheme",
  "draw_white_space": "all",
  "folder_exclude_patterns": [".git"],
  "font_face": "Courier New",
  "font_size": 14.0,
  "ignored_packages":
  [
    "Vintage"
  ],
  "line_numbers": true,
  "save_on_focus_lost": true,
  "smart_indent": true,
  "tab_size": 2,
  "translate_tabs_to_spaces": true
}
```

Your SublimeText is now configured and you're ready to write some code!
