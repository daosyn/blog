---
title: "Go[lang] and [Surface] Go"
date: 2018-09-07T17:25:26-05:00
---

I am currently learning Go on the side for fun.
I really appreciate the conciseness of every facet of the language, from its built-in testing package, a formatter that standardizes coding style, an enforced but sensible workspace structure, and the sheer simplicity of the language's syntax.
<!--more-->

Recently, I unintentionally obtained a Surface Go for myself.
I had purchased one online for my younger sister, but since it did not arrive in time, I grabbed another one from a physical store.
She was thoroughly satisfied with the size and functionality of the tiny thing, so I was convinced to keep the extra one that arrived a week later.

In my opinion, Microsoft has been doing a lot of commendable things recently.
They are focusing on building their brand in such a way that caters to developers by releasing products like the Windows Subsystem for Linux (WSL) and Visual Studio Code, and also by continuing to support the open source community after acquiring GitHub. 
Azure is one of the biggest competitors to the powerhouse that is Amazon Web Services, and the Surface line is becoming more and more enticing as Apple continues to churn out less-than-optimal MacBooks with shallow keyboards, stripped down ports, and that Touch Bar that seems more gimmicky than it is practical.

My experience in the work place has really made me come to understand the usefulness of command-line interfaces (CLI) over using a graphical user interface to perform actions.
With a simple `-h` I can specify the hostname that I want to connect to instead of scrolling through a list of available hosts and clicking on the right one when I finally find it.
I still intend and want to learn PowerShell at some point, but one of the biggest reasons why most developers used MacBooks had been because MacBooks provided a Unix shell right out of the box.
PowerShell might be useful, but the tech industry currently depends on containers and servers running Linux to deploy their software.
And now with WSL, macOS and Windows are back on equal footing for developers to consider.

Anyway, as much as I like Vim, I typically develop using Visual Studio Code because that is what my teammates use at work to code in Node.js and Angular.
It is slightly easier to show someone how to find and replace patterns with Ctrl+F in VS Code than it is to explain how to use `:substitute` in Vim.
I have multiple machines that I use Go on, and I prefer using the same `$GOPATH` across all of them.
With WSL, however, I am not able to interact with my normal Go path outside of WSL, i.e., in VS Code.
To circumvent this, I make it so that my actual Go workspace resides in a folder accessible to Windows and create a symbolic link to it where my `$GOPATH` would normally be.
Essentially, I run these commands:

```sh
# /mnt/c/ is how C:\ is represented in WSL
mv $GOPATH /mnt/c/Users/<username>/<whatever>

# link new location back to $GOPATH
ln -s /mnt/c/Users/<username>/<whatever> $GOPATH

# run this in any Go project tracked by git
git config core.filemode false
```

The last command is needed because the symbolic link that gets created, for reasons I still need to understand, always shows permissions as `777`.
Setting that git configuration ensures that I do not commit any changes related to the permissions of a file.
In VS Code, I set `"terminal.integrated.shell.windows": "C:\\WINDOWS\\System32\\wsl.exe"` in my `settings.json` so that I get my WSL shell when working in the editor.

So that is my current configuration. I do not know if this is the most optimal way to go about doing it, but it works.
