---
layout: post
title: "First AUR Package"
date: 2019-07-19
draft: false
---

Today marks the first time I've publicly released something since [Quack.Net](https://www.nuget.org/packages/Quack.Net/), which was almost 2 years ago. I am proud to present: [shshare](https://github.com/MarkusG/shshare)!

## What?

shshare is a shell script (technically two) that can capture a screenshot or screen recording and automatically upload it to a destination of the user's choosing. It's contained in two files: `shshare` - the core script that handles options and the capturing of data, and `uploader.sh`, a script intended to be modified by the user that provides the functionality of taking that data to its final destination.

## Why?

Before creating shshare, I used a collection of about 5 shell scripts to capture screenshots and screencasts. I decided I wanted to centralize all of that into one script (although it turned out being two), and shshare was born.

At first, I had the capture and upload functionality contained in the same script. I quickly realized that this would make handling custom upload destinations a hassle, so I decided to move it all to a second `uploader.sh` script. This way, if a user wants to use a file host other than the default, they can simply copy the default script in `/etc/shshare/` to their home config directory and modify it to their liking. This way, the user can upload to wherever they want, and I don't have to worry about their wacky file hosts.

### Bonus Points

Separating shshare into two scripts also helps it follow the Unix philosophy. `shshare` is only concerned with capturing data, and `uploader.sh` is only concerned with uploading it.

## How?

`shshare`, as the name suggests, is a shell script. It's written completely in bash and uses `maim` to capture screenshots and `ffmpeg` to capture screen recordings. `uploader.sh` is installed to the `/etc/shshare/` directory, but can be copied to `$XDG_CONFIG_HOME` and modified to the user's liking. By default, it uploads both screenshots and screen recordings to imgur.

## What did I learn?

This project involved learning the fundamentals of bash scripting - namely variables, proper `test` syntax, traps, and `case` statements. The most unfamiliar part of this project was creating a PKGBUILD and releasing it on the Arch User Repository. Understanding what exactly the `prepare()`, `build()`, and `package()` functions were for - and that I only needed one of them, as well as understanding how the source tarball should be structured tripped me up at first, but all became clear as I looked through other AUR packages and dissected the build process myself to see where I was going wrong.

## Conclusion

Overall, I'm happy with the outcome of this project. It's not as feature-complete as I'd like it to be, and I'm sure it's easy to break if not used in the exact way I tested it, but that's what prereleases are for, right? v0 is the crazy college years of any piece of software, so I'm not going to sweat it too much.
