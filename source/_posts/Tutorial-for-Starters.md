---
title: Tutorial for Starters for Coders on Mac
date: 2024-08-16 15:57:29
tags: [tutorial, mac, starter]
categories: [tutorial]
mathjax: true
---
# Tutorial for Starters
This is a tutorial for starters to learn how to code on mac.

## Install Homebrew
- Homebrew is a package manager for macOS. It is used to install software packages that are not included in macOS by default. To install Homebrew, open Terminal and run the following command:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
 
## Install Git
- Git is a version control system that allows you to track changes in your code and collaborate with others. To install Git using Homebrew, run the following command:
```Bash
brew install git
```

## Create a github account
- Github is a platform for hosting and sharing code. It allows you to collaborate with others, track changes in your code, and showcase your projects. To create a Github account, go to the Github website and sign up for an account.  
- link to github account by ssh key

## Install Python
- Python3 is already installed on M1 Mac Air by default.  
- So just ignore this step if mac is M1 or later.  
- Python is a popular programming language that is widely used for web development, data analysis, artificial intelligence, and more. To install Python using Homebrew, run the following command:
```bash
brew install python3
```

## Install Iterm2
- iTerm2 is a terminal emulator for macOS. It provides more features and customization options than the built-in Terminal app. To install iTerm2 using Homebrew, run the following command:
```bash
brew install --cask iterm2
```

## Install Zsh
- Probably zsh is already installed on mac by default.  
- So just ignore this step if mac is M1 or later.  
- Zsh is a powerful shell that provides more features and customization options than the default Bash shell. To install Zsh using Homebrew, run the following command:
```Bash
brew install zsh
```

## Install Oh My zsh
- Oh My Zsh is a community-driven framework for managing your Zsh configuration. It comes with a variety of themes, plugins, and features that make working with Zsh more productive and enjoyable. To install Oh My Zsh, run the following command:
```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## Install p10k
- Powerlevel10k is a theme for the Zsh shell that provides a customizable prompt with additional information and features. To install Powerlevel10k, run the following command:
```bash
brew install romkatv/powerlevel10k/powerlevel10k
```
 
## Install tmux
tmux is a terminal multiplexer that allows you to run multiple terminal sessions in a single window. To install tmux using Homebrew, run the following command:
```bash
brew install tmux
```
customize the nvim configuration file(Very important)

## Install Oh My Tmux
Oh My Tmux is a configuration framework for tmux that provides a variety of themes, plugins, and features to enhance your tmux experience. To install Oh My Tmux, run the following command:
```bash
git clone https://github.com/gpakosz/.tmux.git
ln -s -f .tmux/.tmux.conf
cp .tmux/.tmux.conf.local
```

## Install nvim
- Neovim is a text editor that is designed for developers. It provides more features and customization options than the built-in Vim editor. To install Neovim using Homebrew, run the following command:
```Bash
brew install neovim
```
- learn how to use Neovim by following the tutorials and documentation on the Neovim website.
  - vimtutor(quick start how to use vim)
    - type vimtutor in the terminal to start the vim tutorial
  - vim cheat sheet 
    - https://vim.rtorr.com
- customize the nvim configuration file(Very important)

## Install yabai
- yabai is a tiling window manager for macOS. It allows you to organize your windows in a more efficient way. To install yabai using Homebrew, run the following command:
```bash
brew install koekeishiya/formulae/yabai
```

## Install skhd
- skhd is a hotkey daemon for macOS. It allows you to define custom keyboard shortcuts to control your windows and applications. To install skhd using Homebrew, run the following command:
```bash
brew install koekeishiya/formulae/skhd
```

## Install Node.js
- Node.js is a JavaScript runtime that allows you to run JavaScript code outside of a web browser. To install Node.js using Homebrew, run the following command:
```bash
brew install node
```

## Install nvm
- nvm is a Node Version Manager that allows you to easily switch between different versions of Node.js. To install nvm, run the following command:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```

## Install joshuto
- joshuto is a command-line tool that allows you to quickly search for and open files and directories. To install joshuto using Homebrew, run the following command:
```Bash
- brew install joshuto
```

## Install karabiner-elements
- karabiner-elements is a powerful and flexible keyboard remapping tool for macOS. It allows you to customize your keyboard shortcuts and key mappings. To install karabiner-elements using Homebrew, run the following command:
```bash
brew install --cask karabiner-elements
```

