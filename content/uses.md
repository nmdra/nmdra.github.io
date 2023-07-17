---
title: Uses
date: 2023-07-13
lastmod: 2023-07-14
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
`Last Update: 2023-07-14 21:40`

+ In this article, I will share my personal technology setup and preferences. Inspired by [Eric Murphy's fascinating Article](https://ericmurphy.xyz/uses/). I have carefully selected the tools and configurations that suit my needs and enhance my productivity.

+ **You can explore my (messy)  [Dotfiles here](https://github.com/nmdra/Dotfiles)**. It's a repository where I store and share my configuration files for various applications and *Bash Scripts*. Feel free to check it out! 👀🔧

{{<figure src="/images/desktop.webp" caption="My Desktop With Favorite Applications" alt="My Desktop Preview" width= "100%" height="auto"  align="center" >}}

## Linux Distribution & Desktop Environment

+ 🐧 Among the various Linux distributions available, my personal choice is **[Manjaro Linux](https://manjaro.org)!** It may have received some criticism from the Linux community, but in my opinion, it's the only distribution that really hits the spot for me. 🎯👌 I've tried Ubuntu, Pop!_OS, ArcoLinux, Linux Mint, and even ventured into Arch Linux territory, but Manjaro stands out as the perfect fit for my needs and preferences.

+ 🖥️ When it comes to my desktop environment, I have found my perfect match in **[KDE](https://kde.org/)!** 🌈✨ With its feature-rich and customizable interface, KDE has become my go-to choice for a visually appealing and efficient desktop experience. 😊🚀

## Web browsing

+ For my web browser, I use **[Firefox](https://firefox.com)** with some custom configurations. Occasionally, I also use **[Brave Browser](brave.com)**.While Brave is decent, it tends to be a bit bloated 😑.
Here are the browser extensions I rely on:
    - [uBlock Origin](https://ublockorigin.com/): Adblocker.
    - [Vimium](https://addons.mozilla.org/en-US/firefox/addon/vimium-ff/): for Vim-like keybindings.
    - [Bitwarden](https://bitwarden.com/): For managing my passwords securely, I rely on Bitwarden.

## Personal Development Environment (PDE)

+ Curious about what a PDE (Personal Development Environment) is? *Check out this informative video [youtu.be/QMVIJhC9Veg](https://youtu.be/QMVIJhC9Veg) by [TJ DeVries](https://github.com/tjdevries) to learn more and discover how it can enhance your coding experience*
.
+ I rely on **[Neovim](https://neovim.org)** as my Personal Development Environment (PDE). ⌨️😊 Despite Vim's notorious steep learning curve, I firmly believe that the journey is worth the ultimate rewards. 🚀💪

+ Additionally, I also use **[VSCode](https://code.visualstudio.com)** with Vim keybindings.

## Terminal & Tools

+ I rely on the terminal file manager **[lf](https://github.com/gokcehan/lf)** for its impressive speed, customization capabilities, and support for image previews using a custom script. For GUI file management, I turn to **[Dolphin](https://invent.kde.org/system/dolphin)**, a versatile file manager that offers a user-friendly graphical interface. 🖥️📂

+ I use **[Alacritty](https://github.com/alacritty/alacritty)** as my terminal emulator, along with **[Zsh](https://www.zsh.org/)** as my shell and **[Tmux](https://github.com/tmux/tmux/wiki)** as a terminal multiplexer. 🔥💻⚡

+ I use **[Ferdium](https://github.com/ferdium/ferdium-app)** to organize and manage my favorite web applications like Discord, Notion, and more.

## Media Player and Configuration

+ I rely on **[MPV](https://github.com/mpv-player/mpv)** as my go-to media player, enhanced with a convenient [yt-dlp](https://github.com/yt-dlp/yt-dlp) hook, allowing seamless playback and stream youtube videos without nonsense 🎥🎵. You can found my [MPV-Config in here](https://github.com/nmdra/Dotfiles/tree/main/mpv).

I've added the following alias to my `.zshrc` file to easily listen to YouTube videos:

```bash
ytmusic="mpv --vo=null --video=no --pause=no --no-video --term-osd-bar --loop-playlist=inf "
```

This alias allows me to enjoy YouTube without interruptions, using the `$ ytmusic <YouTube Url>` command. 🎵🎧


## Theme, Font, and Color Scheme

I embrace the **[Tokyonight](https://github.com/folke/tokyonight.nvim)** color scheme for all my applications, including this website's Dark Mode. When it comes to my terminal font, I opt for **[JetBrains Mono](https://www.jetbrains.com/lp/mono/)**, specifically the Nerd Font version,pleasant coding experience. 🎨🖥️✨

## This Website

- This site is built with **[Hugo](https://gohugo.io)**, a static site generator that outputs clean HTML and CSS, avoiding the bloat commonly found in modern web development.
- This website utilizes the **[PaperMod Theme](https://github.com/adityatelange/hugo-PaperMod)** with some customizations.
- Hosting for this site is provided by **[GitHub Pages](https://github.io)**.

