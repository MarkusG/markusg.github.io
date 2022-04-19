---
layout: post
title: "Creating GitHub Releases From the Command Line"
date: 2020-05-18
draft: false
---

I grew tired of having to navigate GitHub's webpages in order to create releases on my repositories, so I decided to automate the process. It started as an [80 line bash script](https://gist.github.com/MarkusG/d3782b90d0f79754e76acf1f58334c63) specifically for my last project, [shshare](https://markgross.me/2019/07/19/first-aur-package), but then I got the itch to generalize it and release it to the public. I also wanted a way to show off my C knowledge on my GitHub profile, so [hubrelease](https://github.com/MarkusG/hubrelease) was born.

## Humble Beginnings

This project began as an 80 line bash script, and only 13 of those lines ended up being reimplemented in hubrelease. The process was simple - make a POST request to GitHub's API to create a release, upload any release assets the user might have, and return the URL of the newly created release to the user. While this could be accomplished for a specific repository and GitHub user in a few lines of bash quite easily, generalizing it so that it could work with *any* repository/user in C took a little bit more effort.

## Moving From Bash to C

In the original bash script, I used `curl` to make HTTP requests and `jq` to parse their responses. I needed to find C libraries for both of these tasks, which proved to be an easy task. I used `libcurl` for HTTP requests because it's already provided by `curl` and it's earned a reputation as the go-to HTTP client, and I used `jansson` for parsing JSON responses because it was one of the first results I found when I searched for "c json library."

## Project Structure

Given its small size, it should be no surprise that hubrelease has a straightforward project structure. Its directory structure is as follows:

```
hubrelease
├── doc
│   ├── hubrelease.1.in
│   └── meson.build
├── hubrelease
│   ├── options.c/h  # handles command-line options
│   ├── curl.c/h     # holds a callback function for our HTTP requests
│   ├── git.c/h      # handles our interactions with the git repository
│   ├── github.c/h   # handles generating GitHub tokens and stripping remotes
│   ├── common.h     # holds some convenience macros and constants
│   ├── main.c       # main program logic
│   └── meson.build
└── meson.build
```

In addition to being my first C project, hubrelease was also my first experience using a build system - Meson in this case. I'm a big fan of Meson's `subdir()` command, allowing me to have separate build files in each directory to keep different build targets organized. The top-level `meson.build` handles project-wide information like the project's name, version, and release date, while the other build files handle their respective targets. `doc/meson.build` handles the configuration and building of the project's manpage, and `hubrelease/meson.build` handles the building of the executable itself.

I decided to put the call to the `executable()` command in the `hubrelease/meson.build` file instead of the top-level one. This keeps the executable's build logic out of the top-level build file, but it creates a `hubrelease` subdirectory in the build directory. The other option is to only have the `hubrelease/meson.build` file configure the source files and call `executable()` in the top-level build file. This would keep most of code that has to do with building the executable in `hubrelease/meson.build`, and output the executable in the build directory instead of a subdirectory. I think this is the best option for executable projects, as I like to name my source directory `src` instead of `$projectname`, and having a `src` subdirectory in the build directory doesn't make much sense. However, I didn't come to that decision until after I was done working on hubrelease, so you won't see that until my next project.

## Closing Thoughts

Overall, this project proved to be a great opportunity to practice my C programming skills with a fully fledged project. It's not perfect by any means - the `main()` function is quite a monolith at 421 lines long - but it's not bad either. It feels good to have a C project under my belt, as I now have the confidence that I can write somewhat decent C code. Finally, I started this project to make my life easier, and I ended up accomplishing that. I'll undoubtedly use hubrelease to create releases for my future projects, and hopefully other people will too.

Also, in case you were wondering why this post was written half a year after hubrelease's first release, it's because it never occurred to me to do a writeup for the project until just recently, when I changed my site's theme to a minimalistic one and realized just how little content I have on here.
