---
layout: page
title: Creating Accounts
sidebar: true
---

* Create a GitHub.com account. Remember to keep your username and email professional. Often usernames include some combination of your first and last names/initials

* Create a repository in GitHub. Make sure to check the box next to “Initialize this repository with a README”.

* Create a Nitrous.io account by signing up with your Github account

* Create a new Ruby/Rails box. Click on “Open Dashboard” to get to the screen from where you can create a new box.

* Add box key to GitHub. Click on “Boxes” on upper hand side. When taken to the screen with your box click on your box. In box details click “Reveal public key” and “Add to Github”. 

* Verify in GitHub that the key was added by going to Account Settings/SSH Keys.
In Nitrous click on IDE for your Ruby box

* Once inside your Box, change directory to “workspace” in the command line console.
Start Git by typing git init. You should get a message that reads “Initialized empty Git repositiry in home/action/workspace/.git/”

* Configure the following settings

{% terminal %}
git config --global user.email <your email>
git config --global user.name <your GitHub username>
git remote add origin git@github.com:<your username>/<your Github repository>.git
{% endterminal %}

<div class="note">
<p>You will not get any messages when doing this!</p>
</div>

* Do your first pull from your GitHub repository by typing 

{% terminal %}
git pull origin master
{% endterminal %}

When asked if you want to continue type yes.
Click on your workspace directory and you should see your READM.md file from your GitHub repository.
Make changes to README.md file and save it

* Start tracking your READM.md file by typing 

{% terminal %}
git add README.md
{% endterminal %}

* Commit the file by typing 

{% terminal %}
git commit -m “First commit”
{% endterminal %}

Push changes to GitHub repository by typing 

{% terminal %}
git push origin master
{% endterminal %}

* You should be able to see your changes in your GitHub repository.
