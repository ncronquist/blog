---
layout: post
title:  "Setting up a Web Development Environment"
date:   2015-08-11
category: programming
tags: [ubuntu, web-development, development, tutorial, rails, node]
---

Ok, today we'll be setting up our basic web development environment with Vermeer FrontPage and Adobe Coldfusion.. Ahhh! Just checking to see if you jumped straight to the terminal commands. No, we'll be going over the setup to start developing with Node and Rails.<!--more--> This tutorial will be tailored for Ubuntu 14.04, but you should be able to find most of the same programs for other Linux distros or Mac OSX. Windows development won't be covered in this post, but I highly suggest checking out [Chocolatey](https://chocolatey.org/) for your Windows package management needs.

### Basic Tools

#### Copy and Paste in your Terminal

Since this tutorial is going to cover a lot of terminal commands that you'll probably want to just copy and paste, we better get that sorted out first. The gnome terminal supports copy and paste by default, but the default shortcuts are a bit different than standard copy and paste shortcuts.

``` bash
# copy
ctrl + shift + c

# paste
ctrl + shift + v
```

As you can see, the shortcuts aren't *that* much different, but that's not good enough for me. If it's going to drive you crazy too, we can switch them by going to the **Edit -> Keyboard Shortcuts** menu in your terminal.

Double click on the `copy` and `paste` shortcuts and use your keyboard to enter the normal shortcuts. By default `ctrl + c` is the interrupt shortcut which is used to kill the current command process. This shortcut will automatically get switched to `ctrl + shift + c`.

#### Nautilus Open Terminal

Nautilus is the default file manager in Ubuntu. We probably won't be using the file manager directly too much today, but this is a super useful program that allows you to right click inside the Nautilus file manager and open a terminal window to that location.

``` bash
sudo apt-get install nautilus-open-terminal

# Now restart Nautilus
nautilus -q
```

#### wget and curl

Wget and curl are both command line tools that can be used to download content from FTP, HTTP, and HTTPS. We'll be using these tools to download public key files and shell scripts. On my most recent install of Ubuntu 14.04, curl and wget were both included automatically, but you'll want to make sure you have them installed on your machine.

``` bash
wget -V

curl -V
```

If either of these programs are missing, we can simply update our repository lists and install them with apt-get.

``` bash
sudo apt-get update

sudo apt-get install curl

sudo apt-get install wget
```

#### Git

If you're doing web development, chances are that you're going to use Git at some point in time. Git is a distributed version control system. Even if you choose to use a different VCS or if you choose not to use version control at all (PLEASE RETHINK THIS DECISION), then you'll probably still want Git installed just to easily download other helpful projects or tool source code. You can simply use `sudo apt-get install git` to install Git, but unfortunately the default Ubuntu repository is pointing to an old version. Add the new repository, update, and install.

``` bash
sudo add-apt-repository ppa:git-core/ppa

sudo apt-get update

sudo apt-get install git

```

Lets take just a moment to configure Git locally too.

``` bash
git config --global user.name "John Doe"

git config --global user.email johndoe@example.com

```

We can also configure the a global .gitingore file. A .gitignore file is used to tell Git to ignore certain files so they don't get tracked in the repository. You'll often want to create a project .gitignrore file to ignore specific files for that project, like you environment variables file or node modules, but a global .gitignore is great for ignoring system files like Mac OS's `.DS_Store` files or Vim and Emacs `~` files.

``` bash
echo "*~" >> ~/.gitignore_global

echo ".DS_Store" >> ~/.gitignore_global

git config --global core.excludesfile ~/.gitignore_global
```

You can check out more Git configuration options [here](https://git-scm.com/book/tr/v2/Customizing-Git-Git-Configuration)

Running Git locally is a great way to keep your code revision history, but you'll probably want your code backed up to a web-based repository hosting service. This is where [GitHub](https://github.com/), [GitLab](https://about.gitlab.com/), and [Bitbucket](https://bitbucket.org/) come in handy. GitHub seems to be the most popular, but GitLab and Bitbucket both offer unlimited public and private repositories for free. GitHub and GitLab both seem pretty solid with GitHub's user interface maybe being slightly nicer. I haven't had the chance to try Bitbucket yet, but it's used by some pretty major companies, so I would guess it's pretty decent too.

In order to get set up for a remote hosting service, we'll need to create an SSH key.

``` bash
ssh-keygen -t rsa -C "YOUR@EMAIL.com"
```

Next we want to add this SSH key to our GitHub, GitLab, and/or Bitbucket accounts. Copy the output of the following command.

``` bash
cat ~/.ssh/id_rsa.pub
```

And paste it [here for GitHub](https://github.com/settings/ssh), [here for GitLab](https://gitlab.com/profile/keys), and at `https://bitbucket.org/account/user/YOUR_BITBUCKET_USERNAME/ssh-keys/` for BitBucket.

Now test it out.

``` bash
ssh -T git@github.com

# Expected reply: 'Hi YOUR_USERNAME! You've successfully authenticated, but GitHub does not provide shell access.'
```

``` bash
ssh -T git@gitlab.com

# Expected reply: 'Welcome to GitLab, YOUR_NAME!
```

#### Prettier Terminal and Zshell

While the default gnome terminal and bash shell are both great, I think Zshell and an updated theme for the gnome terminal can go a long way to improving productivity and making your terminal a bit nicer to look at ALL DAY LONG. First, let's grab Chris Kempson's [base16-gnome-terminal](https://github.com/chriskempson/base16-gnome-terminal) themes.

``` bash
git clone https://github.com/chriskempson/base16-gnome-terminal.git ~/.config/base16-gnome-terminal
```

The above command will use git to clone Chris's theme repository from GitHub into your home/.config directory. Next we need to execute all of the theme shell scripts. This git.io link is a short link to a simple shell script that should do just that. This command takes several seconds to run, so give it a minute.

``` bash
curl -fsSL http://git.io/vOpiF | bash
```

All of the base16-gnome-terminal themes should now be available in your terminal under  `Edit --> Profiles`. Make sure to select the *Profile used when launching a new terminal:*. You will need to restart your terminal after making any changes.

Now that our terminal is nice and pretty, let's get Zshell set up to make it a bit more user friendly.

``` bash
sudo apt-get install zsh
```

You can run Zsh simply by typing `zsh` into your shell and hitting enter. The first time you do this, you will be asked to set up Z Shell. I am comfortable with the defaults, so I usually just type `0` to exit and create the ~/.zshrc file. Typing `1` will give you the chance to go through and tweak the configuration though.

Now make Zsh your default shell.

``` bash
chsh -s $(which zsh)
```

**After you run that command, you will need to logout and log back in.** Zsh is a powerful shell, but to really see it shine, you'll want to check out [Oh My Zsh](https://github.com/robbyrussell/oh-my-zsh) as well. Oh My Zsh adds hundreds of plugins and themes for Z Shell.

``` bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Oh-my-zsh adds a pretty solid default configuration for Z Shell, but if you really want to make it your own, check out the configuration file.

``` bash
gedit ~/.zshrc
```

Oh-my-zsh comes with over 100 themes! You can check out the screenshots [here](https://github.com/robbyrussell/oh-my-zsh/wiki/themes). Somehow, even with all these themes, I ended up creating my own which is a combination of the robbyrussell theme and the Fish shell look.

![Robby Fish Zsh Theme](http://i.imgur.com/bkSpHSd.png "RobbyFishZshTheme")

This curl command will add my RobbyFish theme to your Oh My Zsh themes folder.

``` bash
curl -fsSL http://git.io/v3fbq -o ~/.oh-my-zsh/themes/robbyfish.zsh-theme
```

Activate this theme by changing the following line in the .zshrc config file.

``` bash
# Change this line:
ZSH_THEME="robbyrussell"

# To this:
ZSH_THEME="robbyfish"
```

Aliases are abbreviations for often used commands. You can add aliases to Zsh at the bottom of the ~/.zshrc config file. I have aliases to directories I often want to jump to like project folders as well as for tasks I use frequently. Here are some aliases you may find useful.

``` bash
alias c="clear" # Clears your terminal screen
alias n.="nautilus ." # Opens the nautilus file viewer from the directory you are currently viewing in terminal
# Foreman is a manager for Procfile-based applications. We'll get to this more later, but for now you can add the aliases.
# Nodemon is a monitor script that watches files in the directory in which nodemon was started, and if any files change, nodemon will automatically restart your node application; We'll add this later too
alias fn="foreman run nodemon"
alias fr="foreman run rails s"
alias frc="foreman run rails c"
alias fruby="foreman run ruby"
alias srv="python -m SimpleHTTPServer \$1" # Simple, but awesome python http server; If you're working with a static http page, you can use this to serve it rather than viewing the file directly in the browser
# PostgreSQL aliases
alias psqle="sudo -u postgres psql" #
alias pgserver="sudo -u postgres service postgresql start"
# MongoDB aliases
alias mongoStart="sudo service mongod start"
alias mongoStop="sudo service mongod stop"
alias mongoRestart="sudo service mongod restart"
```

### Web Browser

#### Chrome

Since we're covering web development, we also better get set up with a web browser. Ubuntu comes pre-installed with Firefox, but in general I prefer Chrome for development, and even if you like Firefox, you'll want Chrome for testing.

Install the from the Google Linux Repository.

``` bash
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
```

After installing the key we can add it to the repository.

``` bash
sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
```

Now we just need to update our repository packages lists and install Chrome.

``` bash
sudo apt-get update

sudo apt-get install google-chrome-stable
```

### Text Editors / IDEs

Now it's time to get down to the good stuff. *Real* programmers use VIM or EMACS of course, so it's fortunate that Sublime Text and Atom both support some form of Vim or Vi mode :) For those of us who havn't taken the Vim/Emacs dive yet, Sublime Text 3 and Atom are both great options. Sublime Text seems faster and more stable that Atom, but Atom is relatively new and it's getting better all the time. Atom was heavily influenced by Sublime Text so no matter which one you choose, switching shouldn't be too much of an issue. While Sublime Text is freeware and will set you back $70 for the license, Atom is free and open source, and backed by GitHub. (*Note* I mostly use Atom now, but I paid for Sublime Text and never regretted it once. It's a great piece of software.)

#### Sublime Text

Let's install Sublime Text first.

``` bash
sudo add-apt-repository ppa:webupd8team/sublime-text-3

sudo apt-get update

sudo apt-get install sublime-text-installer
```

Sublime Text has tons of settings you can tweak to your liking, so I'm not going to go into that here. If you want to add plugins to Sublime though, and you will, then you will need to install [Package Control](https://packagecontrol.io/installation). I would also highly suggest going through Tuts+ [Perfect Workflow in Sublime Text 2](https://code.tutsplus.com/courses/perfect-workflow-in-sublime-text-2) course. The course is a bit old, some of their suggested packages are outdated, and it was done on a Mac so some of the shortcuts will be a little different, but it has loads of great information on being more productive with Sublime Text and that experience will transfer to Atom as well.

#### Atom

As I mentioned before, Atom is my primary text editor now and I think it does a better job of making settings more accessible, it comes with a package manager, and it seems to have more useful preinstalled packages.

``` bash
sudo add-apt-repository ppa:webupd8team/atom

sudo apt-get update

sudo apt-get install atom
```

### Languages and Frameworks

There are tons of frameworks for web development out there, but I'm going to cover Node and Ruby on Rails. They are both very popular with large and helpful communities and plenty of available resources.

#### Node and JavaScipt

Just interested in front-end JavaScript development? You're in luck! The text editors we have already installed will handle writing JavaScript and you can do all your testing right in the browser. Want to do back-end JavaScript? We better get Node installed.

``` bash
curl -sL https://deb.nodesource.com/setup_0.12 | sudo bash -

sudo apt-get install -y nodejs
```

Awesome, now we should be set up with Node and NPM which is the package manager for Node. You can check these with teh following commands.

``` bash
node -v

npm -v
```

You can start a node REPL session by typing `node` into your terminal and hitting enter.

Remember the [nodemon](https://www.npmjs.com/package/nodemon) and [foreman](https://www.npmjs.com/package/foreman) aliases we touched on earlier? Now we can install those with NPM. The `-g` option in the following commands means that we are installing these tools globally. When you are installing npm modules for a specific project, you would skip out on the `-g` option and instead use `--save` which will add a reference to that module to your projects `package.json` file.

``` bash
sudo npm install -g nodemon

sudo npm install -g foreman
```

Now you can run your node projects with `foreman run nodemon` or our alias `fn`.

#### Rails and Ruby

JavaScript is built into your browser, but if we want to run Ruby files, we'll need to install the Ruby interpreter first. [Go Rails](https://gorails.com/setup) has SUPER awesome tutorials on getting set up for Ruby on Rails development. They even break it down for each individual version of Ubuntu or Mac OSX. I'm just going to cover their instructions for Ubuntu 14.04.

We're going to install Ruby version 2.2.2 using rbenv. First we'll install the dependencies for Ruby and then we'll actually get ruby set up. (*NOTE* Anytime we add to the path, we will do it for both bash and zsh.)

``` bash
sudo apt-get update

sudo apt-get install zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev

cd

git clone git://github.com/sstephenson/rbenv.git .rbenv

echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc

echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc

echo 'eval "$(rbenv init -)"' >> ~/.bashrc

echo 'eval "$(rbenv init -)"' >> ~/.zshrc

exec $SHELL


git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build

echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc

echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.zshrc

exec $SHELL


git clone https://github.com/sstephenson/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash


rbenv install 2.2.2

rbenv global 2.2.2

ruby -v
```

As NPM is the node package management, Rubygems are Ruby's version of package management. Rubygems was installed during the Ruby install process. Now we'll tell ruby to not install the documentation for each package locally.

``` bash
echo "gem: --no-ri --no-rdoc" > ~/.gemrc
```

Unlike NPM modules, Gems are always installed globally so there is no need for `-g` or `--save`. *NOTE* Ruby on Rails projects will still have a gemfile which contains the gems that are specific to that project. This allows you to easily get someone else's Rails project up and running by running `Rails bundle` from whithin the project. Of course, we better install `bundler` if we want to do that.

``` bash
gem install bundler
```

We should be all set up with Ruby, so now lets get down to Rails. *NOTE* Rails actually needs a JavaScript runtime like Node setup to allow for certain things in rails. If you skipped the Node setup above, you'll want to go back and do that now.

``` bash
gem install rails -v 4.2.1
```

Make the rails executable available for rbenv.

``` bash
rbenv rehash
```

### Databases

Like web development frameworks, there's a wealth of database options out there as well. [PostgreSQL](http://www.postgresql.org/) seems like a solid and popular choice for an RDBMS and [MongoDB](https://www.mongodb.org/) has become quite popular for it's role in the MEAN stack.

#### PostgreSQL

PostgreSQL is an open source object-relational database system. When your data seems like it would fit nicely in the row/column format of an Excel spreadsheet and when you have many different types of objects that all relate to one another, then Postgres is probably your best bet. The following commands will install PostgreSQL version 9.3.9 and pgAdmin III which is an administration and development platform (basically a GUI) for PostgreSQL.

``` bash
sudo apt-get install postgresql

sudo apt-get install pgadmin3
```

We also want to add a user for our database.

``` bash
sudo -u postgres createuser YOUR_USERNAME -s

# This next command will take you into the Postgres command line interface
sudo -u postgres psql
# OR use our earlier alias
psqle

# From the PostgresCLI (postgres=#) type
\password YOUR_USERNAME

# You will then be asked to enter a new password twice. MAKE SURE YOU REMEMBER IT!

# Some useful psql commands:
\? # access PostgreSQL help
\list # list all of the available databases
\dt # list all of the available tables in the current database
\conninfo # check your connection information
CREATE DATABASE testdb # creates a database called testdb
\connect testdb # connects to the testdb database
\q # exit the Postgres CLI
```

#### MongoDB

MongoDB is one of the leading NoSQL document databases. Basically this means a schema-less database where the information is stored as a JSON/BSON document. To install Mongo, first add the MongoDB public GPG key.

``` bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
```

Then create a list file for MongoDB.

``` bash
echo "deb http://repo.mongodb.org/apt/ubuntu "$(lsb_release -sc)"/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list
```

Now we just update our repository and install.

``` bash
sudo apt-get update

# 32-bit install
sudo apt-get install -y mongodb

# 64-bit install
sudo apt-get install -y mongodb-org
```

Now that Mongo is all set up here are a couple quick commands for you.

``` bash
sudo service mongod start # start MongoDB
mongoStart # Our earlier alias to start MongoDB

sudo service mongod stop # stop MongoDB
mongoStop # Our earlier alias to stop MongoDB

sudo service mongod restart # restart MongoDB
mongoRestart # Our earlier alias to restart MongoDB

Control + C # Run this in the terminal where the mongod instance is running to stop it
```

`mongo` will start the mongo console. Here is a list of [mongo shell quick references](http://docs.mongodb.org/manual/reference/mongo-shell/).
