# Front-End Development Setup on a Mac

This document assumes you're running a fresh and updated copy of **macOS "formerly known as OS X"**.

The following workflow assumes a clean installation of macOS. While it's okay to have third-party software installed, the installation process will be more streamlined and less convoluted with a new macOS.

- [Command Line Shell](#command-line-shell)
- [System update and Disk Encryption](#system-update-and-disk-encryption)
- [System tweaks](#system-tweaks)
- [Projects Directory](#projects-directory)
- [Xcode Command Line Tools](#xcode-command-line-tools)
- [ZSH](#zsh-optional)
- [Homebrew](#homebrew)
- [Homebrew Cask](#homebrew-cask)
- [RVM & Ruby](#rvm-and-ruby)
- [Git](#git)
- [Node.js](#nodejs)
- [ES6](#es6)
- [Sass](#sass)
- [Sublime Text, Atom, and VSCode](#sublime-text-atom-and-vscode)
- [Vim](#vim)
- [VirtualBox](#virtualbox)
- [Vagrant](#vagrant)
- [MongoDB](#mongodb)
- [Composer](#composer)

## Command Line Shell

Throughout this document, you will encounter examples like this that contain one or more of the arguments listed:

    sudo command -flag --flag directory file.extention

Front-end development has increasingly moved towards an open-source, command-line interface (CLI) dependent workflow. Whether we access modules, packages or simply useful commands, setting up a command-line shell to your liking is a good idea to start.
    
Anytime you see this, it is referring to your CLI of choice, whether it's the built-in Terminal.app or a third-party application like [iTerm2](https://iterm2.com/).

## System update and Disk Encryption

Step One - Update the system!
**Apple Icon > App Store > Updates**

Step Two - Turn on FileValut
**Apple Icon > System Preferences > Privacy & Security > FileVault**

Click on the lock to allow you to turn on and enable FileVault. On a brand new machine or macOS installation, it should take around an hour or less to get this done depending on the size of your drive.

Alternatively, you can use a third-party encryption software like [Veracrypt](https://en.wikipedia.org/wiki/VeraCrypt/), which is open-source and well regarded in the security community.

### Enable FileVault Encryption

Why do you want [full-disk encryption](https://en.wikipedia.org/wiki/Disk_encryption)? Theft.

You're most likely using a portable laptop of some kind. If you lose it, the laptop gets stolen or someone tries to hack into it, your personal data is at risk. Using full-disk encryption is an extra layer of security to keep your mind at ease in case of potential intrusion.

Two main caveats:
- Make sure you do not forget your FileVault password. Losing this password means you cannot log in and everything on your computer is inaccessible. iCloud is an option to store the Filevault password and this means that Apple Support will be able to assist you with recovering data.
- If macOS gets corrupted and you need to download files from the drive after accessing the drive from an external case, it's not possible. Make sure you're both backing up using [Time Machine](https://support.apple.com/en-us/HT201250) on an external drive or a NAS, and a cloud backup provider like [Backblaze](https://www.backblaze.com/), [Carbonite](https://carbonite.com/), or [iDrive](https://www.idrive.com).

### Enable Firewall

This is for online protection when you're not in your home network or not behind a router.

    sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate on

## System Tweaks

Apple's default system settings are limiting and don't show a lot of information. Let's change the settings for better usability around the system.

### Unhide the Library folder

    chflags nohidden ~/Library
    
Alternatively, open Finder, press `⇧⌘H`, `⌘2`, `⌘J` and check “Show Library Folder”.

### Show Path Bar and Status Bar in Finder

    defaults write com.apple.finder ShowPathbar -bool true
    defaults write com.apple.finder ShowStatusBar -bool true

### Show Full Path in Finder Title Bar

    defaults write com.apple.finder _FXShowPosixPathInTitle -bool true

### Prevent Time Machine from Prompting to Use New Hard Drives as Backup Volume

I don’t use Time Machine. It is better than nothing but not necessary. I don’t need these annoying windows popping up whenever I’m plugging in a new external harddrive.

    sudo defaults write /Library/Preferences/com.apple.TimeMachine DoNotOfferNewDisksForBackup -bool true

### Show All File Extensions

    defaults write -g AppleShowAllExtensions -bool true
    
### Disable Creation of DS_Store Files on Network Volumes and USB Drives

    defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true
    defaults write com.apple.desktopservices DSDontWriteUSBStores -bool true

## Projects Directory

If you don't already have one, create a projects directory somewhere on your machine. I like to use `~/Sites/project-name`. I prefer my Sites folder to exist with the rest of my user profile folders.

    mkdir -p ~/Sites

Depending on the type of projects you work on, this might not be necessary or preferable.

## Xcode Command Line Tools

An important dependency before Homebrew can work is the **Command Line Tools** for **Xcode**. These include compilers that will allow you to build things from source.

Xcode weighs something ~2GB and is useful for the iOS simulator but is not necessary unless you're developing iOS or Mac apps. Good news is Apple provides a way to install only the Command Line Tools, without Xcode.

Using Terminal, install the Xcode Command Line Tools:

    xcode-select --install

For older OSes, go to [http://developer.apple.com/downloads](http://developer.apple.com/downloads), and sign in with your Apple ID (the same one you use for iTunes and app purchases).

Once you reach the downloads page, search for "command line tools", and download **Command Line Tools for Xcode**. Open the **.dmg** file once it's done downloading, and double-click on the **.mpkg** installer to launch the installation. When it's done, you can unmount the disk in Finder.

## ZSH (optional)

By default, Terminal is using Bash Unix shell. Bash contains the command language used to interact with Unix functions, such as `pwd`, `ls`, and `cd`.

[Z Shell](https://en.wikipedia.org/wiki/Z_shell), or ZSH, was written to extend Bash and make improvements to how Bash works. One of the most popular frameworks written around ZSH is called [Oh My Zsh!](http://ohmyz.sh/).

Install it with the following command:

    curl -L https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh

Now you have ZSH installed. [Sign up and follow the videos recorded by Wes Bos](http://commandlinepoweruser.com/) to learn a ton more about ZSH and why it's so powerful.

## Homebrew

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for OS X is [Homebrew](http://brew.sh/).

### Install

    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

Open an new terminal tab with **Cmd+T** (you should also close the old one), then run the following command to make sure everything works:

    brew doctor

### Usage

To install a package (or **Formula** in Homebrew vocabulary) simply type:

    brew install <formula>

Replace `<forumla>` with the name of the formula you want to install.

To see if any of your packages need to be updated:

    brew outdated

To update a package:

    brew upgrade <formula>

Homebrew keeps older versions of packages installed, in case you want to roll back. That rarely is necessary, so you can do some cleanup to get rid of those old versions:

    brew cleanup

To see what you have installed (with their version numbers):

    brew list --versions

## Homebrew Cask

Homebrew Cask extends Homebrew to let you install OS X applications and large binaries alike. Use it to install browsers and editors!

    brew tap caskroom/cask

For archived and beta/alpha software, there's an addition called alternative versions of Casks

    brew tap homebrew/cask-versions

Let's test this by installing Firefox.

    brew cask install firefox

### Installing multiple applications

Brew Cask is awesome because now that you understand what it does, you can install all your favorite apps in one command! Here's a list of my favorite apps, including Google Chrome, that I need for development on a regular basis.

    brew cask install google-chrome google-chrome-canary chromium firefox firefox-developer-edition opera brave torbrowser slack sublime-text-dev visual-studio-code atom github sourcetree imageoptim imagealpha google-nik-collection vlc filezilla transmission skype virtualbox authy appcleaner vagrant tunnelblick slack sublime-text

Note: Google Chrome contains Adobe Reader, Flash and Java by default. Running standalone versions of each is not recommended because they are a security risk without regular maintenance and updates.

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
    $ ruby -v
    $ rails -v

### RVM and ZSH

If you previously installed ZSH, Terminal might throw the following error at the top of any new Terminal session:

    Warning: PATH set to RVM ruby but GEM_HOME and/or GEM_PATH not set, see:
    https://github.com/rvm/rvm/issues/3212

To [resolve this issue](https://stackoverflow.com/questions/27784961/received-warning-message-path-set-to-rvm-after-updating-ruby-version-using-rvm), open the .zshrc file and change the follow path from:

    export PATH="/path/to/something"

to

    export PATH="$PATH:/path/to/something"

If your export `PATH` has no quotations, it will still work correctly just by entering `$PATH:` into the export `PATH`.

## Git

What's a developer without [Git](http://git-scm.com/)?

    brew install git
    git config --global user.name "Your Name Here"
    git config --global user.email "your_email@youremail.com"

**Note**: On a Mac, it is important to remember to add `.DS_Store` (a hidden OS X system file that's put in folders) to your `.gitignore` files. You can take a look at this repository's [.gitignore](/nicolahery/mac-dev-setup/blob/master/.gitignore) file for inspiration.

## Node.js

Install [Node.js](http://nodejs.org/) with Homebrew:

    brew install node

It also installs the [npm](https://npmjs.org/) package manager. 

As suggested by the Homebrew output, we need to add `/usr/local/share/npm/bin` to our path so that npm-installed modules with executables will have them picked up. To do so, add this line to your `~/.path` file, before the `export PATH` line:

```bash

PATH=/usr/local/share/npm/bin:$PATH

```

Open a new terminal for the `$PATH` changes to take effect.

We also need to tell npm where to find the Xcode Command Line Tools, by running:

    sudo xcode-select -switch /usr/bin

Node modules are installed locally in the `node_modules` folder of each project by default, but there's at least a couple that are worth installing globally such as [Gulp](http://gulpjs.com/) or [webpack](https://webpack.js.org/), depending on your project requirements:

    npm install -g gulp
    npm install webpack webpack-cli --save-dev

### Npm usage

To install a package:

    npm install <package> # Install locally
    npm install -g <package> # Install globally

To install a package and save it in your project's `package.json` file:

    npm install <package> --save

To see what's installed:

    npm list # Local
    npm list -g # Global

Other useful commands:

    npm outdated # Outdated packages
    npm update <package> # Update a package
    npm uninstall <package> # Uninstall a package
    
## ES6

Javascript frameworks such as Angular, React and Vue now rely on the newest version of Javascript starting with ECMAScript 2015 (ES6). Browser quirks that gave rise to jQuery are less problematic because web standards are regularly implemented and iterated. Thus, the golden age of Javascript is upon us.

Until browsers catch up implementing the newest features of ES6, it is recommended to use a transpiler to convert your unsupported ES6 back to ES5, which is universally supported in all modern browsers.

The most popular transpiler is Babel(https://babeljs.io/). Only install this locally into an already created project.

    npm install --save-dev babel-cli babel-preset-env

Since it's generally a bad idea to run Babel globally you may want to uninstall the global copy by running `npm uninstall --global babel-cli`

## Sass

Install your preprocessor of choice, but I highly recommend using Sass. They all do the same thing but Sass has the most momentum behind it right now.

With WSL command line:

    npm install -g sass

Keep in mind that the utility that Sass offers is slowly being complimented and deprecated with the rise of [CSS variables](https://medium.freecodecamp.org/everything-you-need-to-know-about-css-variables-c74d922ea855).

## Sublime Text, Atom, and VSCode

The text editor is a developer's most important tool. Everyone has their preferences, but unless you're a hardcore [Vim](http://en.wikipedia.org/wiki/Vim_(text_editor)) user, I recommend one of three editors: [Sublime Text](http://www.sublimetext.com/), [VSCode](https://code.visualstudio.com/) or [Atom](https://atom.io/).

### Sublime Text

My preferred editor is Sublime Text 3.

    brew cask install sublime-text-dev

I prefer using the [beta version of Sublime Text 3](https://sublimetext.com/3) which is usually just as stable as version 2.

Sublime Text is not free, but it has an unlimited "evaluation period". The seemingly expensive $70 price tag is worth every penny. If you can afford it, I suggest you [support](http://www.sublimetext.com/buy) this awesome editor. :)

After installing Sublime Text, add [Package Control](https://packagecontrol.io/installation). This is the most important addition you'll make to Sublime Text and it'll give you the power to install plugins, add-ons, themes, color schemes and more.

#### Colors

I recommend to change two color settings:

- **Theme** (which is how the tabs, the file explorer on the left, etc. look)
- **Color Scheme** (the colors of the code).

My favorite theme is [Material Design Darker](https://equinsuocha.io/material-theme/#/darker) and a close second to [Seti_UI Theme](https://packagecontrol.io/packages/Seti_UI). 

Go to **Tools > Command Palette** (Shift-Command-P), Highlight **Package Control: Install Package** and then search for Seti_UI, make sure it's highlighted then press Enter to install it.

Then go to **Sublime Text > Preferences > Settings - User** and add the following two lines (using Seti UI) and restart Sublime:

    "theme": "Seti.sublime-theme",
    "color_scheme": "Packages/Seti_UI/Scheme/Seti.tmTheme",

#### Settings

Let's configure our editor a little. Go to **Sublime Text > Preferences > Settings - User** and paste this code from [my Preferences.sublime-settings file](https://gist.github.com/asuh/67586e056eba7757330f).

Feel free to tweak these to your preference. When done, save the file and close it.

I can also open a file with `$ subl myfile.ext` or start a new project in the current directory with `$ subl .`. Pretty cool!

### Atom

If you like Sublime Text but can't afford or don't want to pay for it, [Atom](https://atom.io/) is an open-source editor in the spirit of Sublime Text that has a healthy community and regular updates.

The main problem for Atom as of autumn 2018 is that large files and projects noticeably slow down Atom's performance.

### VSCode

Visual Studio Code is 2018's "it" open-source code editor. There's a ton of great tutorials and articles, such as [VS Code Docs](https://code.visualstudio.com/docs/introvideos/basics) and [VS Code Can Do That?](https://vscodecandothat.com/).

## Vim

It is a good idea to learn some very basic usage of [Vim](http://www.vim.org/). It is a very popular open-source editor accessed using command-line shells and is usually pre-installed on Unix systems.

For example, when you run a `git commit`, it will open Vim to allow you to type the commit message.

I suggest you read a tutorial on Vim. Grasping the concept of the two "modes" of the editor, **Insert** (by pressing `i`) and **Normal** (by pressing `Esc` to exit Insert mode), will be the part that feels most unnatural. After that it's just remembering a few important keys.

Vim's default settings aren't great, and you could spend a lot of time tweaking your configuration (the `.vimrc` file). But if you're like me and just use Vim occasionally, you'll be happy to know that [Tim Pope](https://github.com/tpope) has put together some sensible defaults to quickly get started.

First, install [pathogen.vim](https://github.com/tpope/vim-pathogen) by running:

    mkdir -p ~/.vim/autoload ~/.vim/bundle
    curl -Sso ~/.vim/autoload/pathogen.vim \
        https://raw.githubusercontent.com/tpope/vim-pathogen/master/autoload/pathogen.vim

Then create a file `~/.vimrc` (you can use `$ subl ~/.vimrc`), and paste in the following:

    execute pathogen#infect()
    syntax on
    filetype plugin indent on

And finally, install the Vim "sensible defaults" by running:

    cd ~/.vim/bundle
    git clone git://github.com/tpope/vim-sensible.git

With that, Vim will look a lot better next time you open it!

## Virtualbox

The traditional way of setting up a front-end development environment used to be to work through FTP or directly on the server. WordPress installations still promote this workflow to some extent.

The last decade has seen significant changes to the front-end workflow, one of the most important being a local development environment: a local database, a local web server, and back-end language among other things. Using this environment decreases time and increases efficiency for front-end development.

There are several ways to setup a local development environment, whether it's [using the built in *AMP stack](http://coolestguidesontheplanet.com/get-apache-mysql-php-phpmyadmin-working-osx-10-9-mavericks/), installing a package like [MAMP](https://www.mamp.info/en/) or [XAMPP](https://www.apachefriends.org/index.html) or using virtual machines like [Parallels](https://www.parallels.com/products/desktop/) or [VMWare Fusion](https://www.vmware.com/products/fusion/) which give you isolated environment to run the above packages.

The free, open-source alternative that I've been enjoying is called [Virtualbox](https://www.virtualbox.org/). This gives you a basic but very capable virtual machine host for any operating system that supports virtual installations.

    brew cask install virtualbox

Once installed, you can easily install many [versions of Internet Explorer from the Microsoft's VM site](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/).

## Vagrant

I have personally tried to move away from MAMP for my dev environment. The alternative I've been enjoying a lot lately is called [Vagrant](https://www.vagrantup.com/). This gives you a powerful way to create a virtual and portable dev environment! It also has built-in connection to your local OS so that you develop in macOS but the environment runs in the VM.

    brew cask install vagrant

The brilliance of vagrant is its ability to be so portable. When you have a project you work with other developers, creating and destroying the identical dev environment is very simple, reading a local vagrant instruction file. Once created, starting this environment is as simple as typing one command.

    vagrant up

A great box to use for new projects is called [Scotch Box](https://box.scotch.io/). It is fully-featured and contains everything I need built in to get started with many projects using PHP, JS or Ruby. For a WordPress environment, [Roots](https://roots.io/) has [Trellis](https://roots.io/trellis/) which includes everything you need for a powerful VM.

## MongoDB

[MongoDB](http://www.mongodb.org/) is a popular [NoSQL](http://en.wikipedia.org/wiki/NoSQL) database.

### Install

Installing it is very easy through Homebrew:

    brew install mongo

### Usage

In a terminal, start the MongoDB server:

    mongod

In another terminal, connect to the database with the Mongo shell using:

    mongo

I'll let you refer to MongoDB's [Getting Started](http://docs.mongodb.org/manual/tutorial/getting-started/) guide for more!

## Composer

Because so many people use PHP in their day to day work, we need a way to manage PHP scripts and packages similarly to how we manage JS dependencies using NPM and Bower.

One of the most popular PHP dependency managers is called [Composer](https://getcomposer.org/). The difference between Composer and NPM, for example, is that Composer works on a project-by-project basis, there is no global installations. So you must run and setup Composer on every new project if you want to use it.

To install Composer, let's go back to Terminal and [use the following commands](https://getcomposer.org/doc/00-intro.md#globally):

```php
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
mv composer.phar /usr/local/bin/composer
```

What this does is downloads a file called `composer.phar` and then moves it to the same global directory that other package managers live so that you can use it in any directory on OS X.

## TODO
- look over rbenv https://github.com/sstephenson/rbenv
- enable blackboxing [ manage framework blackboxing on chrome]
- Allow apps downloaded from anywhere system preferences

## Credits

- [How to Install Xcode, Homebrew, Git, RVM, Ruby & Rails on Mac OS X](http://www.moncefbelyamani.com/how-to-install-xcode-homebrew-git-rvm-ruby-on-mac/)
- [Web development environment setup in OSX 2015](https://www.youtube.com/watch?v=yZ9TD9bmh-M)
- [macOS Development Environment](https://assortment.io/posts/macos-development-environment)
- [Setting Up A New Mac](https://www.davidculley.com/setting-up-a-new-mac/)
