---
author: Nimendra
title: "Ghostty First Impression"
description: "I've tried Ghostty, and here are my quick review."
date: 2024-12-28
lastmod: 2024-12-28
tags: ["terminal","linux"]
categories: ["terminal"]
showtoc: false
TocOpen: false
ShowReadingTime: true
ShowPostNavLinks: true
ShowBreadCrumbs: true
ShowCodeCopyButtons: true
editPost:
  URL: "https://github.com/nmdra/nmdra.github.io/tree/main/content"
  Text: "Suggest edit"
  appendFilePath: true
---

Recently, **[Ghostty](https://ghostty.org)** released its first public version, and many are hyped about it. Ghostty is written in the Zig programming language. 

At the time of writing this article, Ghostty is still in the unstable branch on Manjaro.
I used AUR to install it and noticed that it requires the `pandoc-cli` package to build Ghostty. 
`pandoc-cli` has over 100 Haskell dependencies. Instead, install the `pandoc-bin` package before building Ghostty.

*Here are the features I liked*:

- Ghostty has a built-in terminal multiplexer feature.
- It supports tabs.
- It has ligature support.
- The font rendering is excellent.
- It supports image rendering (though I personally don’t use this feature).

The main advantage is that Ghostty works without requiring much time to configure using a configuration file.([Ghostty zero-configuration philosophy](https://ghostty.org/docs/config#zero-configuration-philosophy))

Here is my configuration to replicate my Alacritty terminal setup:  
(*you can find my alacritty config [here](https://github.com/nmdra/Dotfiles/blob/main/alacritty/alacritty.toml).*)


```toml
theme = "catppuccin-mocha"
font-family = "JetBrainsMono Nerd Font"
font-size = 13
gtk-titlebar = false
```
{{<figure src="/images/ghostty.png" caption="Ghostty with Neovim and Htop" alt="ghostty preview" width= "100%" height="auto"  align="center" >}}

---

Overall, it’s a great terminal. However, I’m still using Alacritty with Tmux because I’m already familiar with that setup and feel it’s faster than Ghostty (although I haven’t run any benchmarks). Alacritty doesn’t support image rendering, ligatures, or native terminal multiplexing. **If you prefer a feature-rich, zero-configuration terminal, I think Ghostty is the best choice**.

Links:
- [ghostty.org/download](https://ghostty.org/download)
- [github.com/ghostty-org/ghostty](https://github.com/ghostty-org/ghostty)
- [alacritty.org](https://alacritty.org/)
- [My Tmux Config](https://github.com/nmdra/Dotfiles/blob/main/tmux/tmux.conf)

