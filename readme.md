# Front-End Development Setup on a Mac

This document assumes you're running a fresh and updated copy of **macOS "formerly known as OS X"**.

The following workflow assumes a clean installation of macOS. While it's okay to have third-party software installed, the installation process will be more streamlined and less convoluted with a new macOS.

- [Command Line Interface](#command-line-interface)
- [System update and Disk Encryption](#system-update-and-disk-encryption)
- [System tweaks](#system-tweaks)
- [Projects Directory](#projects-directory)
- [Xcode Command Line Tools](#xcode-command-line-tools)
- [Homebrew](#homebrew)
- [Homebrew Cask](#homebrew-cask)
- [Privacy](#privacy)
- [ZSH](#zsh)
- [Node.js](#nodejs)
- [Git](#git)
- [SSH](#ssh)
- [ES6](#es6)
- [Sass](#sass)
- [Composer](#composer)
- [Sublime Text, Atom, and VSCode](#sublime-text-atom-and-vscode)
- [Vim](#vim)
- [VirtualBox](#virtualbox)
- [Docker](#docker)
- [Vagrant](#vagrant)

## Command Line Interface

Throughout this document, you will encounter examples like this that contain one or more of the arguments listed:

```bash
sudo command -flag --flag directory file.extention # Comments are behind pound signs
```

Anytime you see this, it is referring to your CLI of choice, whether it's the built-in Terminal.app or a third-party application like [iTerm2](https://iterm2.com/).

Front-end development has increasingly moved towards an open-source driven, command-line interface (CLI) dependent workflow. Whether we access modules, packages or simply useful commands, setting up a command-line shell to your liking is a good idea.

## System update and Disk Encryption

Step One - Update the system!
**Apple Icon > System Preferences > Software Updates**

Step Two - Turn on FileValut
**Apple Icon > System Preferences > Privacy & Security > FileVault**

Click on the lock (bottom-left of window) to allow you to turn on and enable FileVault. On a brand new machine or macOS installation, it should take around an hour or less to get this done depending on the size of your drive.

Alternatively, you can use a third-party encryption software like [Veracrypt](https://en.wikipedia.org/wiki/VeraCrypt/), which is open-source and well regarded in the security community.

### Enable FileVault Encryption

Why do you want [full-disk encryption](https://en.wikipedia.org/wiki/Disk_encryption)? Theft.

You're most likely using a portable laptop of some kind. If you lose it, the laptop gets stolen or someone tries to hack into it, your personal data is at risk. Using full-disk encryption is an extra layer of security to keep your mind at ease in case of potential intrusion.

Two main caveats:
- **Do not misplace or forget your FileVault recovery key or login password**. Losing this password means you cannot log in and without the recovery key everything on your computer is inaccessible if you can't decrypt the files during a recovery. iCloud is an option to store the Filevault password. Using iCloud, Apple Support will be able to assist you with recovering data. iCloud isn't fully encrypted so whilt it's convenient, it's less secure.
- If macOS gets corrupted and you need to download files from the drive after accessing the drive from an external case, it's not possible without the password and recovery key. Make sure you're both backing up using [Time Machine](https://support.apple.com/en-us/HT201250) on an external drive or a NAS, and a cloud backup provider like [Backblaze](https://www.backblaze.com/), [Carbonite](https://carbonite.com/), or [iDrive](https://www.idrive.com).

### Enable Firewall

This is for online protection when you're not in your home network or not behind a router.

```bash
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate on
```

## System Tweaks

Apple's default system settings are limiting and don't show a lot of information. Let's change the settings for better usability around the system.

### Unhide the Library folder

    chflags nohidden ~/Library
    
Alternatively, open Finder, press `⇧⌘H`, `⌘2`, `⌘J` and check “Show Library Folder”. Unhiding this folder could be useful for manual backup, but it's not necessary.

### Show Path Bar and Status Bar in Finder

```bash
defaults write com.apple.finder ShowPathbar -bool true
defaults write com.apple.finder ShowStatusBar -bool true
```

### Show Full Path in Finder Title Bar

```bash
defaults write com.apple.finder _FXShowPosixPathInTitle -bool true
```

### Prevent Time Machine from Prompting to Use New Hard Drives as Backup Volume

I don’t use Time Machine. It is better than nothing but not necessary. But keep this on if you have an external drive you use for backups.

```bash
sudo defaults write /Library/Preferences/com.apple.TimeMachine DoNotOfferNewDisksForBackup -bool true
```

### Show All File Extensions

```bash
defaults write -g AppleShowAllExtensions -bool true
```
    
### Disable Creation of DS_Store Files on Network Volumes and USB Drives

```bash
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true
defaults write com.apple.desktopservices DSDontWriteUSBStores -bool true
```

## Projects Directory

If you don't already have one, create a projects directory somewhere on your machine. I like to use `~/Sites/project-name`. I prefer my Sites folder to exist with the rest of my user profile folders.

```bash
cd / # Go to home directory
mkdir -p ~/Sites
```

Depending on the type of projects you work on, this might not be necessary or preferable.

## Xcode Command Line Tools

An optional but nice-to-have add-on is **Command Line Tools** for **Xcode**. These include compilers that will allow you to build things from source.

Xcode weighs something ~2GB and is useful for the iOS simulator but is not necessary unless you're developing iOS or Mac apps. Good news is Apple provides a way to install only the Command Line Tools, without Xcode.

Using Terminal, install the Xcode Command Line Tools:

    xcode-select --install

### Older OSes
Go to [http://developer.apple.com/downloads](http://developer.apple.com/downloads), and sign in with your Apple ID (the same one you use for iTunes and app purchases).

Once you reach the downloads page, search for "command line tools", and download **Command Line Tools for Xcode**. Open the **.dmg** file once it's done downloading, and double-click on the **.mpkg** installer to launch the installation. When it's done, you can unmount the disk in Finder.

## Homebrew

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for OS X is [Homebrew](http://brew.sh/).

### Install

    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

Run the following command to make sure everything works:

    brew doctor

### Usage

To install a package (or **Formula** in Homebrew vocabulary) simply type:

    brew install <formula>

Replace `<forumla>` with the name of the formula you want to install.

Helpful commands:

```bash
brew outdated # check for outdated packages
brew upgrade <formula> # upgrade package to latest version
brew list --versions # check installed packages and versions
```

## Homebrew Cask

Homebrew Cask extends Homebrew to let you install applications and large binaries. Use it to install macOS software like browsers, editors and tools.

```bash
brew tap caskroom/cask # for modern software
brew tap homebrew/cask-versions # archived and beta/alpha software
```

Let's test this by installing Firefox.

    brew cask install firefox

### Installing multiple applications

Brew Cask is awesome because now that you understand what it does, you can install all your favorite apps in one command! Here's a list of my favorite apps, including Google Chrome, that I need for development on a regular basis (requires both cask commands above).

    brew cask install firefox firefox-developer-edition brave torbrowser slack sublime-text-dev vscodium atom sourcetree imageoptim imagealpha google-nik-collection vlc signal transmission skype virtualbox authy appcleaner vagrant docker tunnelblick iterm2 libreoffice wireguard-tools zoomus

Note: Most modern browsers contain PDF readers like Adobe Reader and sometimes Flash and Java by default. Running standalone versions of each is not recommended because they are a security risk without regular maintenance and updates.

## Privacy

I think now is the time to briefly let you know that macOS communicates with remove Apple services by default. Apple collects data on how you use the operating system through a process called [Differential Privacy](https://www.wired.com/2016/06/apples-differential-privacy-collecting-data/). With this process, Apple know how many people or devices use what and how often. Apple also knows about their user’s habits as a collective, not individuals. [There's not a lot of transparency](https://www.schneier.com/blog/archives/2016/06/apples_differen.html) about what's going on but there are many free and open source applications that help us shut down and block as many as we know about.

First, I recommend you look through [PrivacyTools.io](https://www.privacytools.io/). There's a ton of valuable software and links to consume.

One of the more popular network monitors and script blockers is called [Little Snitch](https://www.obdev.at/products/littlesnitch/index.html). It will keep applications from reporting back stats that can compromise privacy and security.

## ZSH

macOS 10.15 and newer come with zsh as the default shell. Install [Oh My Zsh!](http://ohmyz.sh/) for extra help and nice defaults. 

```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Don't forget to customize ZSH!

[Themes](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes) are available.
[Autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) and [Syntax Highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) will improve ZSH user experience too.

[Sign up and follow the videos recorded by Wes Bos](http://commandlinepoweruser.com/) to learn a ton more about ZSH and why it's so powerful. Or a [free 80 minute video on YouTube by Karl Hadwen](https://www.youtube.com/watch?v=MSPu-lYF-A8).

## Git

What's a developer without [Git](http://git-scm.com/)?

    brew install git
    git config --global user.name "Your Name Here"
    git config --global user.email "your_email@youremail.com"

**Note**: It is important to remember to add `.DS_Store` (a hidden system file that's put in folders) to your `.gitignore` files. You can take a look at this repository's [.gitignore](https://github.com/nicolashery/mac-dev-setup/blob/master/.gitignore) file for inspiration.

## Node.js

For modern Javascript programming, Node.js is required. There are two ways to install Node depending on your needs.

1. Install [Node.js](http://nodejs.org/) with Homebrew:

    brew install node

2. Install [Node Version Manager (nvm)](https://github.com/nvm-sh/nvm/) which installs Node.js. NVM allows you to easily switch between Node versions and is useful for projects on different versions of Node.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
nvm install --lts # long-term support, which is not always the latest version
```

If you installed Homebrew, we need to add `/usr/local/share/npm/bin` to our path so that npm-installed modules with executables will have them picked up. To do so, add this line to your `~/.path` file, before the `export PATH` line:

```bash
PATH=/usr/local/share/npm/bin:$PATH
```

Open a new terminal for the `$PATH` changes to take effect.

We also need to tell npm where to find the Xcode Command Line Tools, by running:

    sudo xcode-select -switch /usr/bin

### nvm usage

When you enter a project, you can install Node using NVM.

    nvm install node

Restart terminal and run the final command.

    nvm use node

Confirm that you are using the latest version of Node and npm.

    node -v
    npm -v

You can switch to another version and use it by changing to the directory where you want to use Node and run the following.

    nvm install xx.xx
    nvm use xx.xx
    
Node modules are defined in a local `package.json` file inside your project. `npm install` will download external libraries and frameworks into each project's own `node_modules` folder by default. You'll never need to actually edit files in this folder, only reference them.

## SSH

Whether it's Github or directly working on servers, SSH has become imperative for modern projects. 

[Github has excellent instructions for setting up git and connecting it to a Github account](https://help.github.com/en/github/getting-started-with-github/set-up-git)

Now you can add a little shortcut to make SSHing into other boxes easier. Paste the following block of code into your SSH config file at `~/.ssh/config`, changing the variables for any hosts that you connect to.

```ssh
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa

Host myssh
  HostName example.com
  User user
  IdentityFile ~/.ssh/key.pem
```

## ES6

Javascript libraries such as React and Vue use modern versions of Javascript starting with ECMAScript 2015 (ES6). Browser quirks that gave rise to jQuery are less problematic because web standards are regularly implemented and iterated. Thus, a golden age of Javascript is upon us.

Until browsers catch up implementing the newest features of ES6, it is recommended to use a transpiler to convert your unsupported ES6 back to ES5, which is universally supported in all modern browsers.

The most popular transpiler is Babel(https://babeljs.io/). Only install this locally into an already created project if you need ES5 support. Check your project, it might already use Babel and you can skip this step.

    npm install --save-dev babel-cli babel-preset-env

Since it's generally a bad idea to run Babel globally you may want to uninstall the global copy by running `npm uninstall --global babel-cli`

## Sass

Install your preprocessor of choice, but I recommend using either Sass (or maybe PostCSS). They all do the same thing but Sass has the most momentum since the end of the 2010s.

    npm install -g sass

Keep in mind that the utility that Sass offers is slowly being complimented and deprecated with the rise of [CSS variables](https://www.freecodecamp.org/news/everything-you-need-to-know-about-css-variables-c74d922ea855/).

## PHP and Composer

PHP is still one of the most used programming languages on the web, thanks in part to the amount of sites still using WordPress. We need a way to manage PHP scripts and packages similarly to how we manage JS dependencies using NPM.

One of the most popular PHP dependency managers is called [Composer](https://getcomposer.org/). The difference between Composer and NPM, for example, is that Composer works on a project-by-project basis, there is no global installations. So you must run and setup Composer on every new project if you want to use it.

To install Composer globally, go to the [Download page](https://getcomposer.org/download/) and run the package installer.

## Sublime Text, Atom, and VSCode

The text editor is a developer's most important tool. Everyone has their preferences, but unless you're a hardcore [Vim](https://en.wikipedia.org/wiki/Vim_(text_editor)) user, I recommend one of three editors: [Sublime Text](https://www.sublimetext.com/), [VSCode](https://code.visualstudio.com/) or [Atom](https://atom.io/).

### Sublime Text

My preferred editor is Sublime Text 3.

    brew cask install sublime-text-dev

I prefer using the [beta version of Sublime Text 3](https://sublimetext.com/3) which is usually just as stable as version 2.

Sublime Text is not free, but it has an unlimited "evaluation period". The seemingly expensive $70 price tag is worth every penny. If you can afford it, I suggest you [support](https://www.sublimetext.com/buy) this awesome editor. :)

After installing Sublime Text, add [Package Control](https://packagecontrol.io/installation). This is the most important addition you'll make to Sublime Text and it'll give you the power to install plugins, add-ons, themes, color schemes and more.

#### Colors

I recommend to change two color settings:

- **Theme** (which is how the tabs, the file explorer on the left, etc. look)
- **Color Scheme** (the colors of the code).

My favorite theme is [Material Design Darker](https://github.com/equinusocio/material-theme) and a close second to [Seti_UI Theme](https://packagecontrol.io/packages/Seti_UI). 

Go to **Tools > Command Palette** (Shift-Command-P), Highlight **Package Control: Install Package** and then search for your preferred theme, make sure it's highlighted then press Enter to install it.

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

Visual Studio Code found popularity at the end of the 2010s and has become a staple open-source code editor for many front-end developers. I use it both personally and professionally because of various built-in features like git support, terminal integration and a similar-to-Sublime-Text repository of great plugins. I recommend using VSCodium, as it strips away the tracking that Github integrates into VSCode.

    brew cask install vscodium

There's a ton of great tutorials and articles, such as [VS Code Docs](https://code.visualstudio.com/docs/introvideos/basics) and [VS Code Can Do That?](https://vscodecandothat.com/).

## Vim

It is a good idea to learn some very basic usage of [Vim](http://www.vim.org/). It is a very popular open-source editor accessed using command-line shells and is usually pre-installed on Unix systems.

For example, when you run a `git commit`, it will open Vim to allow you to type the commit message.

I suggest you read a tutorial on Vim. Grasping the concept of the two "modes" of the editor, **Insert** (by pressing `i`) and **Normal** (by pressing `Esc` to exit Insert mode), will be the part that feels most unnatural. After that it's just remembering a few important keys.

Vim's default settings aren't great, and you could spend a lot of time tweaking your configuration (the `.vimrc` file). But if you're like me and just use Vim occasionally, you'll be happy to know that [Tim Pope](https://github.com/tpope) has put together some sensible defaults to quickly get started.

First, install [pathogen.vim](https://github.com/tpope/vim-pathogen) by running:

    mkdir -p ~/.vim/autoload ~/.vim/bundle && \
    curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim

If you're brand new to Vim and lacking a vimrc, vim ~/.vimrc and paste in the following super-minimal example:

    execute pathogen#infect()
    syntax on
    filetype plugin indent on

And finally, install the Vim "sensible defaults" by running:

    cd ~/.vim/bundle && \
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

I have personally tried to move away from MAMP for my dev environment. The alternative I like is called [Vagrant](https://www.vagrantup.com/). It gives you a powerful way to create a virtual and portable dev environment! It also has built-in connection to your local OS so that you develop in macOS but the environment runs in the VM.

    brew cask install vagrant

The brilliance of vagrant is its ability to be so portable. When you have a project you work with other developers, creating and destroying the identical dev environment is very simple, by reading a local vagrant instruction file. Once created, starting this environment is as simple as typing one command.

    vagrant up

A great box to use for new projects is called [Scotch Box](https://box.scotch.io/). It is fully-featured and contains everything I need built in to get started with many projects using PHP, JS or Ruby. For a WordPress environment, [Roots](https://roots.io/) has [Trellis](https://roots.io/trellis/) which includes everything you need for a powerful VM.

## Docker

Similar to Vagrant, I increasingly use Docker for professional projects. It comes with similar benefits to Vagrant, such as portability, encapsulation for the environment within the OS, and consistent environments. Docker goes a little further because it's a container manager it's lighter in resources and file size than Vagrant.

    brew cask install docker
    
Docker can be quite powerful but complicated to set up. For this reason, I'm a fan of another project which is a wrapper around Docker called [Lando](https://lando.dev/). Originally designed for Drupal, it's increased support for many other environments including WordPress, Node.js, and Laravel.

    brew cask install lando
    
For privacy, I recommend disabling tracking. Inside of your `.lando.yml` file, add the following:

    stats:
      - report: false
        url: https://metrics.lando.dev

## TODO
- Allow apps downloaded from anywhere system preferences

## Credits

- [How to Install Xcode, Homebrew, Git, RVM, Ruby & Rails on Mac OS X](http://www.moncefbelyamani.com/how-to-install-xcode-homebrew-git-rvm-ruby-on-mac/)
- [Web development environment setup in OSX 2015](https://www.youtube.com/watch?v=yZ9TD9bmh-M)
- [macOS Development Environment](https://assortment.io/posts/macos-development-environment)
- [Setting Up A New Mac](https://www.davidculley.com/setting-up-a-new-mac/)
