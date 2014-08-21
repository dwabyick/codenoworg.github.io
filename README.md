This is the repository for CodeNow's [curriculum page](http://codenoworg.github.io/).

It is generated with [Octopress](http://octopress.org). This means that your changes should be done on the `source` branch.

If you're looking to edit content, then you probably want to be in the `source/projects/` folder.

Get in touch with [Ed Weng](https://github.com/wengzilla) or [Guillaume Ardaud](https://github.com/gardaud) for any questions :)

#Updating the curriculum 101

Clone the repo, then `cd` into it.

```
git checkout source origin/source
git checkout source
```

Then make your changes, commit as needed. Remember, you want to be on `source`.
Debug locally with:

```
bundle exec rake preview
```

Then, to deploy. 

The first time you deploy from a machine:

```
mkdir _deploy
cd _deploy
git init
git remote add origin https://github.com/CodeNowOrg/codenoworg.github.io.git
git pull origin master
cd ..
```

To deploy:

```
bundle exec rake generate
bundle exec rake deploy
```

All done.