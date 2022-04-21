---
layout: post
title: "I Found $17,000 Worth of Bitcoins on Pastebin"
date: 2019-09-28
draft: false
---

## Amendment 2020-08-19
At the time I did this, I was careless and ignorant regarding the legal and ethical implications of my actions. I considered taking this post down, but decided to leave it up for transparency's sake. You should never access a computer that you are not authorized to, even if you think it's being used for a malicious purpose. If you want to explore and experiment with this type of thing, check out [OverTheWire](https://overthewire.org/wargames/) and [Hack The Box](https://www.hackthebox.eu/).

--

Sometimes, when I'm bored, I like to go over to [Pastebin](https://www.pastebin.com/) and keep clicking on newly submitted pastes to see if anything fun comes up. Normally, I don't find anything interesting - logs from someone's Minecraft server, a meaningless chat log, etc, but today, I found something much more interesting. Well, technically, my friend found it. I was alternating between clicking on pastes and playing a game when I heard my friend say, "Dude, you have to look at what I just found." He found a paste containing a username, password, SSH port, and hostname. Now, obviously, I wasn't just going to let this opportunity to poke around a server pass by, so I logged in to see what I could find. I was greeted with the following:

![A Bitcoin balance equivalent to 16,946.73USD, and a menu with options to deposit, withdraw, refresh exchange rate, view history, change the password, and exit](https://github.com/MarkusG/markusg.github.io/raw/master/_posts/2019-09-28-pastebin-bitcoins/terminal-screenshot.png)

My first thought was that I had just found $17,000. The first thing I did was report the paste to Pastebin, as I genuinely thought that somebody had leaked their $17,000 Bitcoin wallet to anyone who wanted it. The paste got taken down pretty quickly, and I decided to poke around to see if I could learn more about what I'd just found. My friend did the same, and I had also sent the paste to a few trusted people before it got taken down.

The first thing I did was try to withdraw 0.001 Bitcoins (about 8USD) to the user's own Bitcoin address. I figured that I could see if an attacker could actually withdraw funds without causing any real damage. As I found out, I couldn't transfer any Bitcoins to an address unless the current account had previously received a minimum of 0.01 Bitcoins (about 80USD) from that address. I didn't have $80 to test this with, but a friend of a friend did, and as you may have assumed by now, he didn't end up turning $80 into $17,000.

It was clear to me at this point that this was not a genuine Bitcoin wallet, so I decided to be a little more destructive in my investigation. As you may have noticed in the initial screenshot, there was an option to change the account's password. I tried a few times and learned that a new password must be both longer than and not similar to the original password. I eventually succeeded in resetting it to a dummy password. This confused me a bit;  Why would a scam like this allow me to change the password, cutting it off from any future targets? I had no idea what was under the hood of this menu, so I can only assume that the new password I gave it wouldn't just be passed to the Unix `passwd` command and I would go about my day. If an attacker was dumb enough to use the same password for everything, and they changed this account's password to that password, then our scammers would have owned the attacker's password.

This was a fun adventure and a great mental workout. What I found the most interesting about this particular "scam" was its target: users who are savvy enough to know the basics of SSH, yet dumb enough to give their password and/or 80USD to an entity they know nothing about. The moral of this story? If something seems too good to be true, it probably is. Oh, and don't give money or sensitive information to strangers, but you shouldn't need a story to teach you that.
