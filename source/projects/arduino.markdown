---
layout: page
title: Arduino
sidebar: true
---

In this multi-phase project, students will build Ruby scripts that interact with an Arduino.

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
