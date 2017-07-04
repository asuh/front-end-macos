# Front-End Development Setup on a Mac

This document assumes you're running a fresh and updated copy of **macOS "formerly known as OS X"**.

If you have any comments or suggestions, feel free to give me a shout [on Twitter](https://twitter.com/asuh)!

- [Terminal](#terminal)
- [System update and Disk Encryption](#system-update-and-disk-encryption)
- [System tweaks](#system-tweaks)
- [Projects Directory](#projects-directory)
- [Xcode Command Line Tools](#xcode-command-line-tools)
- [ZSH](#zsh-optional)
- [Homebrew](#homebrew)
- [Homebrew Cask](#homebrew-cask)
- [RVM & Ruby](#rvm-and-ruby)
- [Sass](#sass)
- [Sublime Text and Atom](#sublime-text-and-atom)
- [Vim](#vim)
- [VirtualBox](#virtualbox)
- [Vagrant](#vagrant)
- [Git](#git)
- [Node.js](#nodejs)
- [Preprocessors and Postprocessors](#preprocessors-and-postprocessors)
- [Yeoman](#yeoman)
- [ES6](#es6)
- [MongoDB](#mongodb)
- [Composer](#composer)
- [Apps](#apps)

## Terminal

Throughout this document, you will encounter examples like this that contain one or more of the arguments listed:

    $ sudo command -flag --flag directory file.extention

Front-end development has increasingly moved towards an open-source, command-line interface (CLI) dependent workflow. Whether we access modules, packages or simply useful commands, setting up Terminal to your liking is a good idea to start.
    
Anytime you see this, it is referring to your CLI of choice, whether it's the built-in Terminal.app or a third-party application like [iTerm2](https://iterm2.com/).

## System update and Disk Encryption

### Prerequisite

If you don't have a new Mac laptop or desktop with a freshly installed operating system, you can [reinstall macOS](https://support.apple.com/en-gb/HT204904). This is highly recommended but not required.

### Update macOS

**Apple Icon > App Store > Updates**

### Enable FileVault Encryption

Before installing anything, let's enable disk encryption. This is the best way to protect your data.

**Apple Icon > System Preferences > Privacy & Security > FileVault**

Click on the lock to allow you to turn on and enable FileVault. On a brand new machine or macOS installation, it should take around an hour or less to get this done depending on the size of your drive.

Alternatively, you can use a third-party encryption software like [Veracrypt](https://veracrypt.codeplex.com/), which is open-source and well regarded in the security community.

Why do you want [full-disk encryption](https://en.wikipedia.org/wiki/Disk_encryption)? Theft.

You're most likely using a portable laptop of some kind. If you lose it, the laptop gets stolen or someone tries to hack into it, your personal data is at risk. Using full-disk encryption is an extra layer of security to keep your mind at ease in case of potential intrusion.

Two main caveats:
- Make sure you do not forget your FileVault password. iCloud is an option to store the Filevault password and this means that Apple Support will be able to assist you with recovering data. Losing this password means you cannot log in and everything on your computer is 100% inaccessible.
- If macOS gets corrupted and you need to download files from the drive after accessing the drive from an external case, it's not possible. Make sure you're both backing up using [Time Machine](https://support.apple.com/en-us/HT201250) and a cloud backup provider like [iDrive](https://www.idrive.com/) or [Crashplan](https://www.code42.com/crashplan/)

### Enable Firewall

This is for online protection when you're not in your home network or not behind a router.

    $ sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate on

## System Tweaks

Apple's default system settings are limiting and don't show a lot of information. Let's change the settings for better usability around the system.

### Unhide the Library folder

    $ chflags nohidden ~/Library
    
Alternatively, open Finder, press `⇧⌘H`, `⌘2`, `⌘J` and check “Show Library Folder”.

### Show Path Bar and Status Bar in Finder

    $ defaults write com.apple.finder ShowPathbar -bool true
    $ defaults write com.apple.finder ShowStatusBar -bool true

### Show Full Path in Finder Title Bar

    $ defaults write com.apple.finder _FXShowPosixPathInTitle -bool true

### Prevent Time Machine from Prompting to Use New Hard Drives as Backup Volume

I don’t use Time Machine. It is better than nothing but not necessary. I don’t need these annoying windows popping up whenever I’m plugging in a new external harddrive.

    $ sudo defaults write /Library/Preferences/com.apple.TimeMachine DoNotOfferNewDisksForBackup -bool true

### Show All File Extensions

    $ defaults write -g AppleShowAllExtensions -bool true
    
### Disable Creation of DS_Store Files on Network Volumes and USB Drives

    $ defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true
    $ defaults write com.apple.desktopservices DSDontWriteUSBStores -bool true

## Projects Directory

Create a project directory somewhere on your machine. I like to use `~/Sites/project-name`. I prefer my Sites folder to exist with the rest of my user profile folders.

    $ mkdir -p ~/Sites
    
## Xcode Command Line Tools

An important dependency before Homebrew can work is the **Command Line Tools** for **Xcode**. These include compilers that will allow you to build things from source.

Xcode weighs something ~2GB and is useful for the iOS simulator but is not necessary unless you're developing iOS or Mac apps. Good news is Apple provides a way to install only the Command Line Tools, without Xcode.

Using Terminal, install the Xcode Command Line Tools:

    $ xcode-select --install

For older OSes, go to [http://developer.apple.com/downloads](http://developer.apple.com/downloads), and sign in with your Apple ID (the same one you use for iTunes and app purchases).

Once you reach the downloads page, search for "command line tools", and download **Command Line Tools for Xcode**. Open the **.dmg** file once it's done downloading, and double-click on the **.mpkg** installer to launch the installation. When it's done, you can unmount the disk in Finder.

## ZSH (optional)

By default, Terminal is using Bash Unix shell. Bash contains the command language used to interact with Unix functions, such as `pwd`, `ls`, and `cd`.

[Z Shell](https://en.wikipedia.org/wiki/Z_shell), or ZSH, was written to extend Bash and make improvements to how Bash works. One of the most popular frameworks written around ZSH is called [Oh My Zsh!](http://ohmyz.sh/).

Install it with the following command:

    $ curl -L https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh

Now you have ZSH installed. [Sign up and follow the videos recorded by Wes Bos](http://commandlinepoweruser.com/) to learn a ton more about ZSH and why it's so powerful.

## Homebrew

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for OS X is [Homebrew](http://brew.sh/).

### Install

In terminal paste the following line (without `$`), hit **Enter**, and follow the steps on the screen:

    $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

Open an new terminal tab with **Cmd+T** (you should also close the old one), then run the following command to make sure everything works:

    $ brew doctor

### Usage

To install a package (or **Formula** in Homebrew vocabulary) simply type:

    $ brew install <formula>

Replace `<forumla>` with the name of the formula you want to install.

To see if any of your packages need to be updated:

    $ brew outdated

To update a package:

    $ brew upgrade <formula>

Homebrew keeps older versions of packages installed, in case you want to roll back. That rarely is necessary, so you can do some cleanup to get rid of those old versions:

    $ brew cleanup

To see what you have installed (with their version numbers):

    $ brew list --versions

## Homebrew Cask

Homebrew Cask extends Homebrew to let you install OS X applications and large binaries alike. Use it to install browsers and editors!

    $ brew tap caskroom/cask

### [Google Chrome](http://google.com/chrome)

Let's test this by installing Google Chrome.

    $ brew cask install google-chrome

### Installing multiple applications

Brew Cask is awesome because now that you understand what it does, you can install all your favorite apps in one command! Here's a list of my favorite apps, including Google Chrome, that I need for development on a regular basis.

    $ brew cask install google-chrome firefox opera brave github sourcetree imageoptim imagealpha google-nik-collection vlc filezilla transmission skype virtualbox appcleaner vagrant tunnelblick slack sublime-text

## RVM and Ruby

Ruby does come pre-installed on Mac, but you probably shouldn't be tinkering around with that version. It's best to install a ruby version manager to take care of anything that one might screw up messing around with your system's version of Ruby.

[RVM](https://rvm.io) stands for Ruby Version Manager and is a command-line tool which allows you to easily install, manage, and work with multiple ruby environments from interpreters to sets of gems.

(Optional) If you plan on installing Rails, I first recommend disabling documentation. RVM installs documentation for every gem that Rails depends on and will slow down installation.

    echo "gem: --no-document" >> ~/.gemrc

Installing RVM, Ruby and Rails can be done with just one command! If you don't plan to use Rails, you can replace `--rails` with `--ruby` in the commands below:

    $ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
    $ \curl -sSL https://get.rvm.io | bash -s stable --auto-dotfiles --autolibs=enable --rails

For more installation options, see the [RVM documentation](https://github.com/rvm/rvm#installation).

In rare cases that you need to reopen multiple shell windows, you need to run the following command in all your open shell windows:

    $ source /Users/{username}/.rvm/script/rvm

Replace `{username}` with your computer's user name which can be found right in your Terminal's prompt.

After it's done, quit and relaunch Terminal, then run this command:

    $ rvm | head -n 1

If you get rvm is a function, that means RVM was successfully installed. If not, go to the Troubleshooting section.

To make sure the latest versions of RVM, Ruby and Rails were installed, run the commands below:

    $ rvm -v

You should get `rvm 1.26.11` or higher.

    $ ruby -v

You should get `ruby 2.2.1` or higher.

    $ rails -v

You should get `Rails 4.2.5.2` or higher.

### RVM and ZSH

If you previously installed ZSH, Terminal might throw the following error at the top of any new Terminal session:

    Warning: PATH set to RVM ruby but GEM_HOME and/or GEM_PATH not set, see:
    https://github.com/rvm/rvm/issues/3212

To [resolve this issue](https://stackoverflow.com/questions/27784961/received-warning-message-path-set-to-rvm-after-updating-ruby-version-using-rvm), open the .zshrc file and change the follow path from:

    export PATH="/path/to/something"

to

    export PATH="$PATH:/path/to/something"

If your export `PATH` has no quotations, it will still work correctly just by entering `$PATH:` into the export `PATH`.


## Sass

Install your preprocessor of choice, but I highly recommend using Sass. They all do the same thing but Sass is the most popular and widely supported.

    $ sudo gem install sass

## Sublime Text and Atom

With Terminal, the text editor is a developer's most important tool. Everyone has their preferences, but unless you're a hardcore [Vim](http://en.wikipedia.org/wiki/Vim_(text_editor)) user, a lot of people are going to tell you that [Sublime Text](http://www.sublimetext.com/) is currently the best one out there.

    $ brew cask install sublime-text

Sublime Text is not free, but I think it has an unlimited "evaluation period". Anyhow, we're going to be using it so much that even the seemingly expensive $70 price tag is worth every penny. If you can afford it, I suggest you [support](http://www.sublimetext.com/buy) this awesome tool. :)

The first thing you do after installing Sublime is to [install Package Control](https://packagecontrol.io/installation). This is the most important addition you'll make to Sublime Text and it'll give you the power to install plugins, add-ons, themes, color schemes and everything in between.

Now for the color. I'm going to change two things:
- **Theme** (which is how the tabs, the file explorer on the left, etc. look)
- **Color Scheme** (the colors of the code).

Again, feel free to pick different ones or stick with the default.

A popular Theme is the [Seti_UI Theme](https://packagecontrol.io/packages/Seti_UI). Go to **Tools > Command Palette** (Shift-Command-P), Highlight **Package Control: Install Package** and then search for Seti_UI, make sure it's highlighted then press Enter to install it.

Then go to **Sublime Text > Preferences > Settings - User** and add the following two lines:

    "theme": "Seti.sublime-theme",
    "color_scheme": "Packages/Seti_UI/Scheme/Seti.tmTheme",

Restart Sublime Text for all changes to take affect (Note: on the Mac, closing all windows doesn't close the application, you need to hit **Cmd+Q**).

Let's configure our editor a little. Go to **Sublime Text > Preferences > Settings - User** and paste this code from [my Preferences.sublime-settings file](https://gist.github.com/asuh/67586e056eba7757330f).

Feel free to tweak these to your taste. When done, save the file and close it.

I can also open a file with `$ subl myfile.ext` or start a new project in the current directory with `$ subl .`. Pretty cool.

Sublime Text is very extensible. For now we'll leave it like that, we already have a solid installation.

### Atom

If you like Sublime Text but can't afford or don't want to pay for it, [Atom](https://atom.io/) is an open-source version of Sublime Text that has a healthy community and regular updates.

The main problem for Atom as of summer 2017 is that large files and projects slow down noticeably Atom's performance. 

## Vim

Although Sublime Text will be our main editor, it is a good idea to learn some very basic usage of [Vim](http://www.vim.org/). It is a very popular text editor inside the terminal, and is usually pre-installed on any Unix system.

For example, when you run a Git commit, it will open Vim to allow you to type the commit message.

I suggest you read a tutorial on Vim. Grasping the concept of the two "modes" of the editor, **Insert** (by pressing `i`) and **Normal** (by pressing `Esc` to exit Insert mode), will be the part that feels most unnatural. After that it's just remembering a few important keys.

Vim's default settings aren't great, and you could spend a lot of time tweaking your configuration (the `.vimrc` file). But if you're like me and just use Vim occasionally, you'll be happy to know that [Tim Pope](https://github.com/tpope) has put together some sensible defaults to quickly get started.

First, install [pathogen.vim](https://github.com/tpope/vim-pathogen) by running:

    $ mkdir -p ~/.vim/autoload ~/.vim/bundle
    $ curl -Sso ~/.vim/autoload/pathogen.vim \
        https://raw.githubusercontent.com/tpope/vim-pathogen/master/autoload/pathogen.vim

Then create a file `~/.vimrc` (you can use `$ subl ~/.vimrc`), and paste in the following:

    execute pathogen#infect()
    syntax on
    filetype plugin indent on

And finally, install the Vim "sensible defaults" by running:

    $ cd ~/.vim/bundle
    $ git clone git://github.com/tpope/vim-sensible.git

With that, Vim will look a lot better next time you open it!

## Virtualbox

The traditional way of setting up a front-end development environment used to be to work through FTP or directly on the server. WordPress installations still promote this workflow to some extent.

The last decade has seen significant changes to the front-end workflow, one of the most important being a local development environment: a local database, a local web server, and back-end language among other things. Using this environment decreases time and increases efficiency for front-end development.

There are several ways to setup a local development environment, whether it's [using the built in *AMP stack](http://coolestguidesontheplanet.com/get-apache-mysql-php-phpmyadmin-working-osx-10-9-mavericks/), installing a package like [MAMP](https://www.mamp.info/en/) or [XAMPP](https://www.apachefriends.org/index.html) or using virtual machines like [Parallels](https://www.parallels.com/products/desktop/) or [VMWare Fusion](https://www.vmware.com/products/fusion/).

The free, open-source alternative that I've been enjoying is called [Virtualbox](https://www.virtualbox.org/). This gives you a basic but very capable virtual machine host for any operating system that supports virtual installations.

    $ brew cask install virtualbox

Once installed, you can easily install many [versions of Internet Explorer from the Modern.ie site](http://dev.modern.ie/tools/vms/).

## Vagrant

I have personally tried to move away from MAMP for my dev environment. The alternative I've been enjoying a lot lately is called [Vagrant](https://www.vagrantup.com/). This gives you a powerful way to create a virtual and portable dev environment! It also has built in support talk to your local OS!

    $ brew cask install vagrant

The brilliance of vagrant is its ability to be so portable. When you have a project you work with other developers, creating and destroying the identical dev environment is very simple, reading a local vagrant instruction file. Once created, starting this environment is as simple as typing one command.

    $ vagrant up

A great box to use for new projects is called [Scotch Box](https://box.scotch.io/). It is fully-featured and contains everything I need built in to get started with many projects using PHP, JS or Ruby. For a WordPress environment, [Roots](https://roots.io/) has [Trellis](https://roots.io/trellis/) which includes everything you need for a powerful VM.

## Git

What's a developer without [Git](http://git-scm.com/)? To install, simply run:

    $ brew install git

When done, to test that it installed fine you can run:

    $ git --version

And `$ which git` should output `/usr/local/bin/git`.

Let's set up some basic configuration. Download the [.gitconfig](https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.gitconfig) file to your home directory:

    $ cd ~
    $ curl -O https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.gitconfig

It will add some color to the `status`, `branch`, and `diff` Git commands, as well as a couple aliases. Feel free to take a look at the contents of the file, and add to it to your liking.

Next, we'll define your Git user (should be the same name and email you use for [GitHub](https://github.com/) and [Heroku](http://www.heroku.com/)):

    $ git config --global user.name "FirstName LastName"
    $ git config --global user.email "your_email@youremail.com"

They will get added to your `.gitconfig` file.

**Note**: On a Mac, it is important to remember to add `.DS_Store` (a hidden OS X system file that's put in folders) to your `.gitignore` files. You can take a look at this repository's [.gitignore](/nicolahery/mac-dev-setup/blob/master/.gitignore) file for inspiration.

## Node.js

Install [Node.js](http://nodejs.org/) with Homebrew:

    $ brew install node

The formula also installs the [npm](https://npmjs.org/) package manager. However, as suggested by the Homebrew output, we need to add `/usr/local/share/npm/bin` to our path so that npm-installed modules with executables will have them picked up.

To do so, add this line to your `~/.path` file, before the `export PATH` line:

```bash
PATH=/usr/local/share/npm/bin:$PATH
```

Open a new terminal for the `$PATH` changes to take effect.

We also need to tell npm where to find the Xcode Command Line Tools, by running:

    $ sudo xcode-select -switch /usr/bin

Node modules are installed locally in the `node_modules` folder of each project by default, but there are at least two or three that are worth installing globally. Those are [Grunt](http://gruntjs.com/) or [Gulp](http://gulpjs.com/), and [Bower](http://bower.io):

    $ npm install -g grunt-cli
    $ npm install --global gulp
    $ npm install -g bower

### Npm usage

To install a package:

    $ npm install <package> # Install locally
    $ npm install -g <package> # Install globally

To install a package and save it in your project's `package.json` file:

    $ npm install <package> --save

To see what's installed:

    $ npm list # Local
    $ npm list -g # Global

To find outdated packages (locally or globally):

    $ npm outdated [-g]

To upgrade all or a particular package:

    $ npm update [<package>]

To uninstall a package:

    $ npm uninstall <package>

### Yeoman

If you intend to use a modern Javascript MVC framework and need a quick way to scaffold out a base framework, [Yeoman](http://yeoman.io/) was built specifically for this purpose. It contains generators for all the most popular frameworks and tools.

    $ npm install yo

### ES6

The time has come for you to start learning the newest version of Javascript, ECMAScript 6 (ES6). Where we needed jQuery previously, it is not the golden standard it once was. Browser quirks are decreasing because web standards are regularly implemented and iterated. Thus, the golden age of Javascript is upon us.

As of June 2015, ES6 is ratified as the newest JS version. Until browsers catch up implementing the newest features, it is recommended to use a transpiler to convert your ES6 back to ES5, which is universally supported in all modern browsers.

The most popular transpiler is [Babel](https://babeljs.io/).

    $ npm install --save-dev babel-cli
    
Since it's generally a bad idea to run Babel globally you may want to uninstall the global copy by running `npm uninstall --global babel-cli`.

Pairing this with a build system will give you the ability to write today's scripting language without worrying about incompatibilities. Check out [Babel's tools](https://babeljs.io/docs/setup/) to build your custom stack.

## MongoDB

[MongoDB](http://www.mongodb.org/) is a popular [NoSQL](http://en.wikipedia.org/wiki/NoSQL) database.

### Install

Installing it is very easy through Homebrew:

    $ brew install mongo

### Usage

In a terminal, start the MongoDB server:

    $ mongod

In another terminal, connect to the database with the Mongo shell using:

    $ mongo

I'll let you refer to MongoDB's [Getting Started](http://docs.mongodb.org/manual/tutorial/getting-started/) guide for more!

## Composer

Because so many people use PHP in their day to day work, we need a way to manage PHP scripts and packages similarly to how we manage JS dependencies using NPM and Bower.

One of the most popular PHP dependency managers is called [Composer](https://getcomposer.org/). The difference between Composer and NPM, for example, is that Composer works on a project-by-project basis, there is no global installations. So you must run and setup Composer on every new project if you want to use it.

To install Composer globally, let's go back to Terminal and [use the following two commands](https://getcomposer.org/doc/00-intro.md#globally):

    $ curl -sS https://getcomposer.org/installer | php
    $ mv composer.phar /usr/local/bin/composer

What this does is downloads a file called `composer.phar` and then moves it to the same global directory that other package managers live so that you can use it in any directory on OS X.

## Apps

Here is a quick list of some apps I use, and that you might find useful as well:

- [Mou](http://markedapp.com/): As a developer, most of the stuff you write ends up being in [Markdown](http://daringfireball.net/projects/markdown/). In fact, this `README.md` file (possibly the most important file of a GitHub repo) is indeed in Markdown, written in Sublime Text, and I use Marked to preview the results everytime I save. **(FREE)**
- [Alfred](http://www.alfredapp.com/): If you find Yosemite's Spotlight to be limited in functionality, try Alfred. It has a healthy community and lots of plugins.
- [Evernote](https://evernote.com/): If I don't write something down, I'll forget it. As a developer, you learn so many new things every day, and technology keeps changing, it would be insane to want to keep it all in your head. So take notes, sync them to the cloud, and have them on all your devices. Another alternative is [Simplenote](http://simplenote.com/) because I only take text notes, and Evernote puts extra spaces between paragraphs when I copy & paste into other applications. Simplenote is great for text notes (and it supports Markdown!). **(Both are free)**
- [SoundCleod](https://github.com/salomvary/soundcleod): Standalone app to play Soundcloud audio/music. Because why not?
- [Maximum Awesome](https://github.com/square/maximum-awesome): I originally intended to write my own build script for a lot of this, but Maximum Awesome already does a fantastic job installing iTerm2, Tmux, MacVim and a plethora of other great features!

## TODO
- look over rbenv https://github.com/sstephenson/rbenv
- enable blackboxing [ manage framework blackboxing on chrome]
- Allow apps downloaded from anywhere system preferences

## Credits

- [How to Install Xcode, Homebrew, Git, RVM, Ruby & Rails on Mac OS X](http://www.moncefbelyamani.com/how-to-install-xcode-homebrew-git-rvm-ruby-on-mac/)
- [Web development environment setup in OSX 2015](https://www.youtube.com/watch?v=yZ9TD9bmh-M)
- [macOS Development Environment](https://assortment.io/posts/macos-development-environment)
- [Setting Up A New Mac](https://www.davidculley.com/setting-up-a-new-mac/)
