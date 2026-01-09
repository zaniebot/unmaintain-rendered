---
number: 460
title: Customize help
type: issue
state: closed
author: hgrecco
labels:
  - A-help
assignees: []
created_at: 2016-03-26T15:33:09Z
updated_at: 2018-08-02T03:29:49Z
url: https://github.com/clap-rs/clap/issues/460
synced_at: 2026-01-07T13:12:19-06:00
---

# Customize help

---

_Issue opened by @hgrecco on 2016-03-26 15:33_

Is there a way to customize the help. In particular I am looking for a way to customize the header and footer of what is automatically generated.


---

_Comment by @kbknapp on 2016-03-26 16:35_

You can customize the footer with [`App::after_help`](http://kbknapp.github.io/clap-rs/clap/struct.App.html#method.after_help)

For header, which part do you mean, or where would you like it to go?


---

_Comment by @hgrecco on 2016-03-26 16:48_

Thanks for pointing me to `after_help`, I was not aware of it. 

I would like to change the header from:
 `{name} {version}\n{author}\n{short description}\n` 
to:
 `{name} -- {description}`

I know that I can remove the author and version easily by stripping `.version()` and `.author()` but I would also like to keep that information. Also, as shown above, I would like to change how it is organized.


---

_Comment by @kbknapp on 2016-03-27 02:50_

Currently there isn't a good way to do this. But i like how you wrote it like a template, that may be something i could add pretty easily.

Ill have to knock out some of the other issues first, then I'll try to tackle this. Thanks! :+1:


---

_Label `T: new feature` added by @kbknapp on 2016-03-27 02:52_

---

_Label `P4: nice to have` added by @kbknapp on 2016-03-27 02:52_

---

_Label `C: help message` added by @kbknapp on 2016-03-27 02:52_

---

_Label `D: intermediate` added by @kbknapp on 2016-03-27 02:52_

---

_Label `W: 2.x` added by @kbknapp on 2016-03-27 02:52_

---

_Comment by @hgrecco on 2016-03-27 03:48_

That was also my idea. I think I can give it a try if you are fine with it. But I have a few open questions:
1. Shall we have a template (A) only for the header or (B) for the whole thing?
2. Which fields should we have? name, version, author, short, about, If we go for (B) we would also need after_help (for backwards compatibilty), usage and related.


---

_Comment by @kbknapp on 2016-03-27 04:11_

Yeah that'd be awesome if you give a shot!

> Shall we have a template (A) only for the header or (B) for the whole thing?

I'd say we start simple with just the header (i.e. `{name}`, `{version}`, `{author}`,  and `{about}`), but keep in mind the ability to add other sections to this templating system later. 

For a second iteration I'd add `{usage}`, `{all-args}` (encompasses all auto-generated flags, options, etc.) and `{after_help}`. 

Then perhaps as a third iteration, break down the `{all-args}` section further into `{flags}` `{options}` `{args}` `{subcommands}`. This way we can add to the system in a non-breaking way.


---

_Comment by @hgrecco on 2016-04-03 05:03_

I have just pushed a large set of commits to my [template branch](https://github.com/hgrecco/clap-rs/commits/template). It is a major change to the way the Help is generated, including the possibility to use templates.

Look at the code and let me know what you think. If you decide to merge my branch I will add first another commit removing the old code.


---

_Comment by @kbknapp on 2016-04-04 01:50_

I'm going through all the changes, and it may take me a few days based on my free time but it looks great so far! I'm excited for this change, and I'll post back with any changes or comments about the commits.

Great work! 


---

_Comment by @kbknapp on 2016-04-08 00:04_

@hgrecco Just finished going through all the changes, and it looks great! I'm very, very excited about this feature! Whenever you're ready put in the PR! 

If you get a chance, could do a rebase and change the commit subject line format so the appropriate changes get picked up in the changelog (i.e. just the ones you'd like picked up in changelog, not all commits need this). Here's the commit format:

`TYPE(COMPONENT): MESSAGE` where `TYPE` is one of the following:
- feat - A new feature
- imp - An improvement to an existing feature
- perf - A performance related changed
- docs - Changes to documentation only
- tests - Changes to the testing framework or tests only
- fix - A bug fix
- refactor - Code functionality doesn't change, but underlying structure may
- style - Stylistic changes only, no functionality changes
- wip - A work in progress commit (Should typically be git rebase'ed away)
- chore - Catch all or things that have to do with the build system, etc
- examples - Changes to existing example, or a new example

The `COMPONENT` is optional, and may be a single file, directory, or logical component. Can be omitted if commit applies globally.

Again, thanks for all the time you've put into this!

Edit: You may also need to rebase onto the latest master :wink:


---

_Comment by @hgrecco on 2016-04-09 02:52_

Do you consider the new Help engine a new feature or a refactor or both? I think the first commits are refactoring but the last (template) is a feat?


---

_Comment by @kbknapp on 2016-05-03 02:00_

Closed with #478


---

_Closed by @kbknapp on 2016-05-03 02:00_

---
