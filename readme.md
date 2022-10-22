# Front-End Development Setup on a Mac

This document assumes you're running a fresh and updated copy of **macOS "formerly known as OS X"**.

The following workflow assumes a clean installation of macOS. While it's okay to have third-party software installed, the installation process will be more streamlined and less convoluted with a new macOS.

- [Command Line Interface](#command-line-interface)
- [System update and Disk Encryption](#system-update-and-disk-encryption)
- [System tweaks](#system-tweaks)
- [Projects Directory](#projects-directory)
- [Homebrew](#homebrew)
- [Privacy](#privacy)
- [Sublime Text and VSCode](#sublime-text-and-vscode)
- [Vim](#vim)
- [ZSH](#zsh)
- [SSH](#ssh)
- [Git](#git)
- [Node.js](#nodejs)
- [Python](#python)
- [Ansible](#ansible)
- [Composer](#composer)
- [VirtualBox](#virtualbox)
- [Laravel Valet](#laravel-valet)
- [Docker](#docker)

## Command Line Interface

Throughout this document, you will encounter examples like this that contain one or more of the arguments listed:

```bash
sudo command -flag --flag directory file.extention # Comments are behind pound signs
```

Anytime you see this, it is referring to your CLI of choice, whether it's the built-in Terminal.app or a third-party terminal like [iTerm2](https://iterm2.com/).

Front-end development has increasingly moved towards an open-source driven, command-line interface (CLI) dependent workflow. Setting up a command-line shell to your liking is a good idea.

## System update and Disk Encryption

Step One - Update the system!
**Apple Icon > System Preferences > Software Updates**

Step Two - Turn on FileValut
**Apple Icon > System Preferences > Security & Privacy > FileVault**

On a brand new machine or macOS installation, it shouldn't take long depending on the size of your drive.

Alternatively, you can use a third-party encryption software like [Veracrypt](https://en.wikipedia.org/wiki/VeraCrypt/), which is open-source and well regarded in the security community.

### Why Enable FileVault Encryption

Why do you want [full-disk encryption](https://en.wikipedia.org/wiki/Disk_encryption)? Theft.

You're most likely using a portable device of some kind. If you lose it, the laptop gets stolen or someone tries to hack into it, your personal data is at risk. Using full-disk encryption is an extra layer of security to keep your mind at ease in case of potential intrusion.

Two main caveats:
- **Do not misplace or forget your FileVault recovery key or login password**. Losing this password means you cannot log in and without the recovery key everything on your computer is inaccessible if you can't decrypt the files during a recovery. iCloud is one option to store the Filevault password. The other option is downloading it and storing it yourself. Using iCloud, Apple Support will be able to assist you with recovering data. On Apple's servers, iCloud isn't fully encrypted. So, while iCloud is convenient, it's less secure.
- If macOS gets corrupted and you need to download files from the drive after accessing the drive from an external case, it's not possible without the laptop password and FileVault recovery key. Make sure you're both backing up using [Time Machine](https://support.apple.com/en-us/HT201250) on an external drive or a NAS, and a cloud backup provider like [Backblaze](https://www.backblaze.com/).

### Enable Firewall

This is for online protection when you're not in your home network or not behind a router.

**Apple Icon > System Preferences > Security & Privacy > Firewall**

or 

```bash
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate on
```

## System Tweaks

Apple's default system settings are limiting and don't show a lot of information. Let's change the settings for better usability around the system.
Note: keep in mind you probably have to re-open Finder or Terminal to see these changes

### Unhide the Library folder

    chflags nohidden ~/Library
    
Alternatively, open Finder, press `⇧⌘H`, `⌘2`, `⌘J` and check “Show Library Folder”. Unhiding this folder could be useful for manual backup, but it's not necessary.

### Set fast keyboard key repeat rate
```bash
defaults write NSGlobalDomain KeyRepeat -int 0
```

### Show filename extensions by default
```bash
defaults write NSGlobalDomain AppleShowAllExtensions -bool true
```

### Show Path Bar and Status Bar in Finder

```bash
defaults write com.apple.finder ShowPathbar -bool true
defaults write com.apple.finder ShowStatusBar -bool true
```

### Show Full Path in Finder Title Bar

```bash
defaults write com.apple.finder _FXShowPosixPathInTitle -bool true
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

Another update I prefer is to show all filename extensions in Finder. 

**Finder > Preferences > Advanced > Show all filename extensions**

### System Path

One very important part of using Terminal in my workflow is being able to quickly type commands that run services. Sometimes, you might install something and it is still not available in the Terminal.

    echo $PATH

Running this command will return a long string, separated by `:`, of various paths to services you can reference using line commands. If you install one of the services like Node and typing `npm` doesn't work, for example, use the above command to check to make sure you see a reference to the path to Node.


## Projects Directory

If you don't already have one, create a projects directory somewhere on your machine. I like to use `~/Sites/project-name`. I prefer my Sites folder to exist with the rest of my user profile folders.

```bash
cd # Go to home directory
mkdir -p ~/Sites
```

Depending on the type of projects you work on, this might not be necessary or preferable.

## Homebrew

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for macOS is [Homebrew](http://brew.sh/).

(If you're using an M1 Mac in 2022, you'll probably have issues with installation. There are multiple articles to help you add commands to the system path and then you can run `brew doctor` to find more advice)

### Install

    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

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

### Installing multiple applications

Here's a list of my favorite apps that I need for development on a regular basis (modify the list below as you need)

    brew install firefox brave-browser tor-browser slack visual-studio-code vscodium sequel-ace imageoptim imagealpha vlc vnc-viewer signal skype virtualbox authy appcleaner vagrant tunnelblick mullvadvpn iterm2 kap libreoffice wireguard-tools zoom scroll-reverser homebrew/cask/docker

To explicity install software applications with a GUI, it's good to use the flag `--cask`. The example below is necessary because `transmission` can default to a CLI version instead of the GUI-based app.

    brew install transmission --cask
    
Don't use `brew` to install *Node.js*, we'll do that below using `nvm`.

### Xcode Command Line Tools

An optional but nice-to-have add-on is **Command Line Tools** for **Xcode**. These include compilers that will allow you to build things from source.

Note: if you already installed Homebrew above, it's likely that Xcode was installed for you so you can skip this section.

Xcode weighs something ~2GB and is useful for the iOS simulator but is not necessary unless you're developing iOS or Mac apps. Good news is Apple provides a way to install only the Command Line Tools, without Xcode.

Using Terminal, install the Xcode Command Line Tools:

    xcode-select --install

There's not a straightforward way to update Xcode Command Line Tools, so we have to remove the existing tools to reinstall from scratch.

    sudo rm -rf /Library/Developer/CommandLineTools
    xcode-select --install

### Older OSes
Go to [http://developer.apple.com/downloads](http://developer.apple.com/downloads), and sign in with your Apple ID.

Once you reach the downloads page, search for "command line tools", and download **Command Line Tools for Xcode**. Open the **.dmg** file once it's done downloading, and double-click on the **.mpkg** installer to launch the installation. When it's done, you can unmount the disk in Finder.

## Privacy

I think now is the time to briefly let you know that macOS communicates with remove Apple services by default. Apple collects data on how you use the operating system through a process called [Differential Privacy](https://www.wired.com/2016/06/apples-differential-privacy-collecting-data/). With this process, Apple know how many people or devices use what and how often. Apple also knows about their user’s habits as a collective, not individuals. [There's not a lot of transparency](https://www.schneier.com/blog/archives/2016/06/apples_differen.html) about what's going on but there are many free and open source applications that help us shut down and block as many as we know about.

First, I recommend you look through [PrivacyGuides.org](https://www.privacyguides.org/). There's a ton of valuable software and links to consume.

One of the more popular network monitors and script blockers is called [Little Snitch](https://www.obdev.at/products/littlesnitch/index.html), which I don't personally use. It will keep applications from reporting back stats that can compromise privacy and security.

## Sublime Text and VSCode

The text editor is a developer's most important tool. Everyone has their preferences, but unless you're a hardcore [Vim](https://en.wikipedia.org/wiki/Vim_(text_editor)) user, I recommend one of two editors: [Sublime Text](https://www.sublimetext.com/) or [VSCode](https://code.visualstudio.com/).

### Sublime Text

I split my time starting here, for older projects. It's a solid code editor with lots of extensibility.

    brew install sublime-text-dev

I prefer using the [alpha version of Sublime Text 4](https://sublimetext.com/) which is just as stable as version 2 and 3. I had to join the discord channel to find a link to download version 4.

Sublime Text is not free, but it has an unlimited "evaluation period". The seemingly expensive $70 price tag is worth every penny. If you can afford it, I suggest you [support](https://www.sublimetext.com/buy) this awesome editor. :)

After installing Sublime Text, add [Package Control](https://packagecontrol.io/installation). This is the most important addition you'll make to Sublime Text and it'll give you the power to install plugins, add-ons, themes, color schemes and more.

#### Colors

I recommend to change two color settings:

- **Theme** (which is how the tabs, the file explorer on the left, etc. look)
- **Color Scheme** (the colors of the code).

I prefer darker themes: [Material Design Darker](https://github.com/equinusocio/material-theme) and [Seti_UI Theme](https://packagecontrol.io/packages/Seti_UI). 

Go to **Tools > Command Palette** (Shift-Command-P), Highlight **Package Control: Install Package** and then search for your preferred theme, make sure it's highlighted then press Enter to install it.

Then go to **Sublime Text > Preferences > Settings - User** and add the following two lines (using Seti UI) and restart Sublime:

    "theme": "Seti.sublime-theme",
    "color_scheme": "Packages/Seti_UI/Scheme/Seti.tmTheme",

#### Settings

Let's configure our editor a little. Go to **Sublime Text > Preferences > Settings - User** and paste this code from [my Preferences.sublime-settings file](https://gist.github.com/asuh/67586e056eba7757330f).

Feel free to tweak these to your preference. When done, save the file and close it.

I can also open a file with `$ subl myfile.ext` or start a new project in the current directory with `$ subl .`. Pretty cool!

### VSCode

Here's where I split the rest of my time.

Visual Studio Code found popularity at the end of the 2010s and has become a staple open-source code editor for many front-end developers. I use it both personally and professionally because of various built-in features like git support, terminal integration, live sharing your code with another developer, and a similar-to-Sublime-Text repository of great plugins. 

    brew install visual-studio-code

I recommend using VSCodium, as it strips away the telemetry and tracking that Github integrates into VSCode. However, [VSCodium has issues with extensions](https://github.com/VSCodium/vscodium/wiki/Extensions-Compatibility) like Live Share, so keep that in mind.

    brew install vscodium

There's a ton of great tutorials and articles, such as [VS Code Docs](https://code.visualstudio.com/docs/introvideos/basics) and [VS Code Can Do That?](https://vscodecandothat.com/).

## Vim

It is a good idea to learn [Vim](http://www.vim.org/). It is a popular open-source editor accessed using command-line shells and is often pre-installed on Unix  and Linux systems.

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

## ZSH

macOS 10.15 and newer come with zsh as the default shell, replacing Bash. Install [Oh My Zsh!](http://ohmyz.sh/) for extra help and nice defaults. 

```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Don't forget to customize ZSH!

[Themes](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes) are available.
[Autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) and [Syntax Highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) will improve ZSH user experience too.

[Sign up and follow the videos recorded by Wes Bos](http://commandlinepoweruser.com/) to learn a ton more about ZSH and why it's so powerful. Or a [free 80 minute video on YouTube by Karl Hadwen](https://www.youtube.com/watch?v=MSPu-lYF-A8).

## SSH

SSH is imperative, just like git and node as you'll see.

[Github has excellent instructions for setting up git and connecting it to a Github account](https://help.github.com/en/github/getting-started-with-github/set-up-git). This will help you to install the repos to your computer from Github as well as set up keys that you'll need to connect git and github.

Now you can add a little shortcut to make SSHing into other boxes easier. Paste the following block of code into your SSH config file at `~/.ssh/config`, changing the variables for any hosts that you connect to.

```ssh
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa
```

Below the above, you can add other sites as needed.

```ssh
Host myssh
  HostName example.com
  User user
  IdentityFile ~/.ssh/key.pem
```

With the above code, you can now run the alias `myssh` to connect.

    ssh myssh

## Git

What's a developer without [Git](http://git-scm.com/)?

    brew install git
    git config --global user.name "Your Name Here"
    git config --global user.email "your_email@youremail.com"

**Note**: It is important to remember to add `.DS_Store` (a hidden system file that's put in folders) to your `.gitignore` files. You can take a look at this repository's [.gitignore](https://github.com/nicolashery/mac-dev-setup/blob/master/.gitignore) file for inspiration.

### Git aliases

Less keystrokes is better, so let's add some sensible shortcuts to a global Git config file.

    touch ~/.gitconfig
    
Pick and choose any of these aliases to help you.

    [user]
      name   = Firstname Lastname
      email  = you@example.com
    [github]
      user   = username
    [alias]
      a      = add
      ca     = commit -a
      cam    = commit -am
      cm     = commit -m
      s      = status
      pom    = push origin master
      pog    = push origin gh-pages
      puom   = pull origin master
      puog   = pull origin gh-pages
      cob    = checkout -b
      co     = checkout
      fp     = fetch --prune --all
      l      = log --oneline --decorate --graph
      lall   = log --oneline --decorate --graph --all
      ls     = log --oneline --decorate --graph --stat
      lt     = log --graph --decorate --pretty=format:'%C(yellow)%h%Creset%C(auto)%d%Creset %s %Cgreen(%cr) %C(bold blue)%an%Creset'

With the above aliases, I can run `git s` instead of `git status` or `git ca` instead of `git commit -a` when I have a bunch of file updates.

## Node.js

For modern Javascript programming, Node.js is required. Using [Node Version Manager (nvm)](https://github.com/nvm-sh/nvm/) to install Node allows you to easily switch between Node versions and is useful for projects on different versions of Node.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.2/install.sh | bash
```

### nvm usage

When you enter a project, you can install Node using NVM.

    nvm install node

Restart terminal and run the final command.

    nvm use node
    
Set a default version of Node.

    nvm alias default xx.xx

Confirm that you are using the default version of Node and npm.

    node -v && npm -v

You can switch to another version and use it by changing to the directory where you want to use Node and run the following.

    nvm install xx.xx
    nvm use xx.xx
    
Node modules are defined in a local `package.json` file inside your project. `npm install` will download external libraries and frameworks into each project's own `node_modules` folder by default. You'll never need to edit files in this folder, only reference them.

Update NVM

    nvm install node --reinstall-packages-from=node

### Auto invoke NPM

Automating NPM to switch to the right Node is a nice little time saver. [Add some code to your shell to allow this auto switch capability](https://github.com/nvm-sh/nvm#deeper-shell-integration).

Put this into your $HOME/.zshrc (using latest MacOS):

```bash
# place this after nvm initialization!
autoload -U add-zsh-hook
load-nvmrc() {
  local node_version="$(nvm version)"
  local nvmrc_path="$(nvm_find_nvmrc)"

  if [ -n "$nvmrc_path" ]; then
    local nvmrc_node_version=$(nvm version "$(cat "${nvmrc_path}")")

    if [ "$nvmrc_node_version" = "N/A" ]; then
      nvm install
    elif [ "$nvmrc_node_version" != "$node_version" ]; then
      nvm use
    fi
  elif [ "$node_version" != "$(nvm version default)" ]; then
    echo "Reverting to nvm default version"
    nvm use default
  fi
}
add-zsh-hook chpwd load-nvmrc
load-nvmrc
```

## Python

Even if you don't use Python in your day to day, it's likely you'll encounter something that requires it. MacOS includes Python 2.x by default. Version 2 is years outdated and almost all python can and should run with a minimum of version 3. Let's update python!

If you didn't already install it with `brew` above, install the latest python with Homebrew.

    brew install python

Now see what we installed

    ls -l /usr/local/bin/python*

You'll now see a few lines like this:

    lrwxr-xr-x  1 user  admin  40 Jan 01 00:00 /usr/local/bin/python3.9 -> ../Cellar/python@3.9/3.9.5/bin/python3.9

We'll use one of these lines to update the symlink so that using your terminal to run `python` will run the latest version by default.

    ln -s -f /usr/local/bin/python3.9 /usr/local/bin/python

Check it out. (you'll have to restart terminal)

    python --version

### Pip

With `python` now running the latest version, it's a good idea to install Pip, which is a package manager for Python.

    curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
    python get-pip.py --user


## Ansible

If you're working with virtual machines or remote servers, it's possible you'll need a way to automate/manage tasks on/with them. Ansible is your answer to this.

    python -m pip install --user ansible

## Composer

PHP is still one of the most used programming languages on the web, thanks in part to the amount of sites still using WordPress. We need a way to manage PHP scripts and packages similarly to how we manage JS dependencies using NPM.

One of the most popular PHP dependency managers is called [Composer](https://getcomposer.org/). The difference between Composer and NPM, for example, is that Composer works on a project-by-project basis, there is no global installations. So you must run and setup Composer on every new project if you want to use it.

To install Composer globally, go to the [Download page](https://getcomposer.org/download/) and run the package installer.

## Virtualbox

There are several ways to setup a local development environment, whether it's [using the built in *AMP stack](http://coolestguidesontheplanet.com/get-apache-mysql-php-phpmyadmin-working-osx-10-9-mavericks/), installing a package like [MAMP](https://www.mamp.info/en/) or [XAMPP](https://www.apachefriends.org) or using virtual machines like [Parallels](https://www.parallels.com/products/desktop/) or [VMWare Fusion](https://www.vmware.com/products/fusion.html) which give you isolated environments.

The free, open-source alternative that I often use is called [Virtualbox](https://www.virtualbox.org/). This gives you a basic but very capable virtual machine host for any operating system that supports virtual installations.

    brew install virtualbox

Once installed, you can easily install many [versions of Internet Explorer from the Microsoft's VM site](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/) if you are still supporting legacy browsers in your project.


## Laravel Valet

In theory, I prefer a completely isolated environment that's self-contained like Docker. Practically, these environments can be bloated, inefficient, and using a lot of CPU processing or Memory.

Since the late 2010s, I've grown to appreciate a project from Laravel called [Valet](https://laravel.com/docs/master/valet). Valet is free, very lightweight, and is relatively easy to set up. I pair it with [DBngin](https://dbngin.com/), a free and open-source database hosting application.

These two paired together leave a small footprint and don't 


## Docker

I increasingly use Docker for professional projects because of its portability and shared environment for projects with multiple developers.

In order to install the desktop application, we have to use the full homebrew path to Docker desktop.

    brew install homebrew/cask/docker
    
Docker can be quite powerful but complicated to set up. For this reason, I'm a fan of another project which is a wrapper around Docker called [Lando](https://lando.dev/). Originally designed for Drupal, it's increased support for many other environments including WordPress, Node.js, and Laravel.

    brew install lando
    
For privacy, I recommend disabling tracking. Inside of your `.lando.yml` file, add the following:

    stats:
      - report: false
        url: https://metrics.lando.dev


## Video walk-through
If you like to watch and see by example, some of what I've covered (and more) is in a video that swyx.io-fame Shawn made. I don't agree with or recommend everything he does, but I think much of it is good to see in real time.
https://www.swyx.io/new-mac-setup-2021/

## Credits

- [How to Install Xcode, Homebrew, Git, RVM, Ruby & Rails on Mac OS X](http://www.moncefbelyamani.com/how-to-install-xcode-homebrew-git-rvm-ruby-on-mac/)
- [Web development environment setup in OSX 2015](https://www.youtube.com/watch?v=yZ9TD9bmh-M)
- [macOS Development Environment](https://assortment.io/posts/macos-development-environment)
- [Setting Up A New Mac](https://www.davidculley.com/setting-up-a-new-mac/)
- [macOS Monterey: Setting up a Mac for Development](https://www.taniarascia.com/setting-up-a-brand-new-mac-for-development/)
- [How to set Python3 as a default python version on MacOS?](https://dev.to/malwarebo/how-to-set-python3-as-a-default-python-version-on-mac-4jjf)
