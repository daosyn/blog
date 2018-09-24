---
title: "Circling Back - Yubikey Setup With Surface Go"
date: 2018-09-23T06:46:44-05:00
---

A small hurdle that I have been running into recently has been setting up my Yubikey with all of the devices that I use.
<!--more-->

The Yubikey has been in my possession for a few months now.
I started using only the U2F feature with popular services such as Google, Twitter, Facebook, and GitHub, and it was quick and painless to get it set up and running.
I recently found out that my workplace also supports Yubikeys for multi-factor authentication, even though only the one-time password feature is supported.
Setting that up was also simple, and I enjoyed the ease of using the Yubikey over generating codes on a phone so much that I began looking into other ways that I could integrate the key into my workflow, for added security, of course, but also just for fun.

<figure>
    <img src="/image/yubiwall.jpg"/>
    <figcaption>
        I switched back to an old wallet from a Bellroy Slim Sleeve because it provided an easy way to carry the key.
    </figcaption>
</figure>

I have a Chromebook (Acer C771), ThinkPad (X220), Surface Go, a corporate MacBook Pro, and a desktop computer currently running Windows 10.
Using GPG on a Tails live image from my desktop to create keys and then move them to the Yubikey was fairly straightforward following the [steps from drduh](https://github.com/drduh/YubiKey-Guide).
Setting up `gpg-agent` to run properly by ensuring that other services such as `ssh-agent` did not take precedence was where I started running into trouble, but -- at least for my ThinkPad, MacBook, and desktop -- adding an environment variable or a few lines into my profile quickly resolved my issues.

My Chromebook is the device I use most for carrying around places due to its compact size and ruggedness -- it is an education model, so it even has rubber edges to prevent damage from accidental drops.
I had been using the Android app, [Termux](https://termux.com/), to learn Golang, and it was proving to be a very solid setup.
So I was fairly disappointed when I realized there is currently no way (apparent to me, at least) for Termux to access my Yubikey.
There is a flag, `#arc-usb-host`, that I tried setting to true in order to see whether that would allow Termux to see USB devices, but no dice.
Hopefully, once Linux apps support comes to my Chromebook, everything will be smooth sailing.

To preface my Surface Go setup, I previously wrote about how I preferred using WSL over PowerShell because I felt more at home in Bash.
WSL suffers from the similar problem that I ran into with my Chromebook, which is that WSL is basically a separate runtime on top of the main operating system and does not currently support external hardware out of the box.
I needed to use PowerShell if I wanted to be able to use my Yubikey to sign my Git commits and connect to GitHub via SSH, which basically meant that I had to abandon my previous workflow with WSL and reinstall all of the tools that I had running in WSL.
In addition to Go and Git, which are basically all I am using at the moment for development, I had to install PuTTY and Gpg4win.

One minor difference that created inconvenience with the Surface Go in comparison to my desktop was that the Surface Go has another smartcard reader that is used to Windows Hello.
The issue with this is that the `scdaemon` that `gpg-agent` runs to check for smartcards is not able to find the Yubikey, presumably because it checks the `Microsoft IDF 0` reader first and errors out before checking for the Yubikey.
I had to scour the web and eventually found a comment in some forum explaining this, and that I can tell the `scdaemon` to specifically check for the Yubikey by adding `reader-port Yubico Yubikey 4 OTP+U2F+CCID 0` to `scdaemon.conf` in the GPG home folder.
That is on top of enabling PuTTY support and setting `GIT_SSH` as an environment variable with its value pointing to `plink`.

<figure>
    <img src="/image/gossh.jpg"/>
    <figcaption>
        Success!
    </figcaption>
</figure>
