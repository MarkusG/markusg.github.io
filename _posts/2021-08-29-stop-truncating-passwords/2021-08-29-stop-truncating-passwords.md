---
layout: post
title: "Start Validating Passwords Instead of Truncating Them"
date: 2021-08-29
draft: false
---

I reset one of my passwords today, and ran into an issue that's becoming more important as more people make the switch to a password manager. The workflow for creating or resetting a password with a password manager is roughly the following:

* Select the password field on the website
* Open the password manager
* Generate a password and copy it to the clipboard
* Paste the password into the password field

If the site's password field stops accepting more characters without notifying the user, their generated password will be truncated if it's longer than the site's maximum length (which, frankly, shouldn't be that short anyway). The user will create or reset their password as if nothing's the matter, but won't be able to log in with the password that they just created. The site's login form will, for whatever reason, accept a password of any length, and then hash it and compare it to the hash on record. If the user's password was truncated when they created/reset it, their full password will certainly have a different hash. It took me a couple of tries to figure out that this was happening, and I imagine many others have had the same experience. Here are a few solutions that come to mind:

### Make your site's maximum password length quite long.

This is the best option. Longer passwords are more secure anyway, and making your maximum length longer than most password managers' default length will ensure that this problem doesn't happen.

### Validate password length, just like all your other silly requirements.

I'm already carefully crafting my password around what kinds of different numbers, capital letters, lowercase letters, Egyptian hieroglyphics, and arcane runes must be present (curly braces are not allowed, though). Why not make length another one of the attributes of my password that's validated client-side? Let me paste as many characters as I want into the password field, and then tell me it's too many, rather than hiding your terrible policy behind the input field's maximum length.

### Make the login form's password field have the same maximum as the creation/reset fields.

This is the worst option. It'll at least let the user log in, but the password in their password manager still won't match their actual password on the site. In my case, I was resetting my password for an e-mail account and then logging into it through Thunderbird, so this still wouldn't have solved the problem.

In sum, websites should start designing their registration and login forms with password managers in mind. The easier it is for people to use their password manager, the easier it'll be for them to keep their accounts secure.
