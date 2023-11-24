---
title: Uses
date: 2023-07-13
lastmod: 2023-11-24
showtoc: true
ShowReadingTime: true
ShowShareButtons: false
ShowPostNavLinks: true
ShowBreadCrumbs: false
ShowCodeCopyButtons: true
editPost:
  URL: "https://github.com/nmdra/nmdra.github.io/tree/main/content"
  Text: "Suggest edit"
  appendFilePath: true
---
`Last Update: 2023-11-24`

## Introduction
+ In this article, I will share my personal technology setup and preferences. Inspired by [Eric Murphy's fascinating Article](https://ericmurphy.xyz/uses/). 

+ **You can explore my (messy)  [Dotfiles here](https://github.com/nmdra/Dotfiles)**. It's a repository where I store and share my configuration files for various applications and *Bash Scripts*. Feel free to check it out! 👀🔧

{{<figure src="/images/desktop.webp" caption="My Desktop With Favorite Applications" alt="My Desktop Preview" width= "100%" height="auto"  align="center" >}}

## Linux Distribution & Desktop Environment

+ 🐧 Among the various Linux distributions available, my personal choice is **[Manjaro Linux](https://manjaro.org)!** It may have received some criticism from the Linux community, but in my opinion, it's the only distribution that really hits the spot for me. 🎯👌 I've tried Ubuntu, Pop!_OS, ArcoLinux, Linux Mint, and even ventured into Arch Linux territory, but Manjaro stands out as the perfect fit for my needs and preferences.

+ 🖥️ When it comes to my desktop environment, I have found my perfect match in **[KDE](https://kde.org/)!** 🌈✨ With its feature-rich and customizable interface, KDE has become my go-to choice for a visually appealing and efficient desktop experience.

## Web browsing

+ For my web browser, I use **[Brave Browser](https://brave.com)**. Occasionally, I also use **[Firefox](https://firefox.com)** & **[Chromium](https://www.chromium.org/Home/)**.
Here are the browser extensions I rely on:
    - [Vimium](https://addons.mozilla.org/en-US/firefox/addon/vimium-ff/): for Vim-like keybindings.
    - [Bitwarden](https://bitwarden.com/): For managing my passwords securely, I rely on Bitwarden.
     - dark reader
     - read aloud

## Personal Development Environment (PDE)

+ Curious about what a PDE (Personal Development Environment) is? *Check out this informative video [youtu.be/QMVIJhC9Veg](https://youtu.be/QMVIJhC9Veg) by [TJ DeVries](https://github.com/tjdevries) to learn more and discover how it can enhance your coding experience*
.
+ I rely on **[Neovim](https://neovim.org)** as my Personal Development Environment (PDE). ⌨️😊 Despite Vim's notorious steep learning curve, I firmly believe that the journey is worth the ultimate rewards.

+ I also use [VSCode](https://code.visualstudio.com) for web development, and I've configured it with Vim keybindings.

## Terminal & Tools

+ My go-to setup for the terminal includes [Alacritty](https://github.com/alacritty/alacritty) as the emulator, [Zsh](https://www.zsh.org/) as my shell, and [Tmux](https://github.com/tmux/tmux/wiki) as a terminal multiplexer.

+ I really like using the terminal file manager [lf](https://github.com/gokcehan/lf) because it's super fast, highly customizable, and even supports image previews with a custom script. And when I need to manage files with a graphical interface, I turn to [Dolphin](https://invent.kde.org/system/dolphin).

+ To keep all my favorite web apps in check, I use [Ferdium](https://github.com/ferdium/ferdium-app). It helps me organize and manage Web apps like Whatsapp, Discord, Notion, and more. Super handy!

## Media Player and Configuration

+ I rely on **[MPV](https://github.com/mpv-player/mpv)** as my go-to media player with some custom configurations.
     
+ These are some scripts used with MPV:
    - **kde-night-color.so**: Turn off Night Color when using MPV.
    - **thumbfast.lua**: High-performance on-the-fly thumbnailer.
    - **modernx.lua**: Modern OSC UI replacement.
    - **playlistmanager.lua**: Playlist Manager.

+ You can find my [MPV Config here](https://github.com/nmdra/Dotfiles/tree/main/mpv).
    
+ I've set up an alias that allows me to enjoy YouTube without interruptions using the following command:
    
```bash
ytmusic="mpv --vo=null --video=no --pause=no --no-video --term-osd-bar --loop-playlist=inf "
```

## Theme, Font, and Color Scheme

I embrace the **[Tokyonight](https://github.com/folke/tokyonight.nvim)** color scheme for all my applications, including this website's Dark Mode. When it comes to my terminal font, I opt for **[JetBrains Mono](https://www.jetbrains.com/lp/mono/)**, specifically the Nerd Font version,pleasant coding experience. 🎨🖥️✨

## This Website

- This site is built with **[Hugo](https://gohugo.io)**, a static site generator that outputs clean HTML and CSS, avoiding the bloat commonly found in modern web development.
- This website utilizes the **[PaperMod Theme](https://github.com/adityatelange/hugo-PaperMod)** with some customizations.

