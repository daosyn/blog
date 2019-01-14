---
title: "switching terminal focus with chunkwm"
date: 2018-12-18T20:22:37-06:00
---

A quick little trick I solved for recently was switching between multiple iTerm2 windows that I would have open during a normal work day.

My workflow consists of using `chunkwm` across three different desktops to manage all of my windows without much effort on my part:

- Desktop 1 (left) is solely for my terminal windows, which are tiled.
- Desktop 2 (center) contains my web browser and any other non-IDE tool/app that I may need to use, in monocle format.
- Desktop 3 (right) is where I occasionally do some development using an IDE or Visual Studio Code, which is also set to monocle.

I prefer to stay in desktops 1 and 2 as much as possible, using `vim` as often as I can.

```shell
RED="\[\e[31m\]"
YELLOW="\[\e[33m\]"
NOCOLOR="\[\e[00m\]"

PS1="($((${ITERM_SESSION_ID:1:1}+1))) $RED\u$YELLOW [\W]$NOCOLOR: "
```

I opted to not use a hotkey daemon (namely `skhd`) because I only wanted one specific feature, and that is to be able to easily change focus to different terminal windows.
`cmd` plus the terminal window number allows me to do that with iTerm2, but the problem was that since I have window bars turned off, I was not able to see the number unless I turned them back on (and for the sake of aesthetics, I chose not to).

So, to solve this I simply output the iTerm2 session ID to my Bash prompt incremented by one (because the ID indexing starts at 0).
It ends up looking something like this:

```shell
(3) daosyn [~]: _
```
I can now easily switch to that terminal window without having to guess by pressing `cmd+3`.

Read more about `chunkwm` [here](https://koekeishiya.github.io/chunkwm/).
