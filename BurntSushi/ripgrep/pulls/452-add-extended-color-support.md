```yaml
number: 452
title: Add extended color support
type: pull_request
state: closed
author: tiehuis
labels: []
assignees: []
base: master
head: issue-261
created_at: 2017-04-18T06:44:30Z
updated_at: 2018-01-29T23:27:07Z
url: https://github.com/BurntSushi/ripgrep/pull/452
synced_at: 2026-01-12T18:23:13Z
```

# Add extended color support

---

_@tiehuis_

Fixes #261.

 - Adds support for 256 and rgb color codes. Format is simply comma separated values.
 - Mixes fine with other styles such as bold/nobold. I did write a brief thing about this in the issue but doesn't seem necessary after writing the documentation.
 - Windows falls back to white in the presence of one of these new color codes.
 - This currently uses a fixed buffer for formatting the color codes instead of the `format!` macro. Its more complicated and I'm not entirely sure the benefit but it can always be changed back if the extra allocations are favored over the more complex code.

---

_@tiehuis reviewed on 2017-04-18 06:46_

---

_Review comment by @tiehuis on `termcolor/src/lib.rs`:957 on 2017-04-18 06:46_

Some eyes on this would be good if its preferred over `format!`. I've added some test cases here but could have missed something.

---

_Comment by @tiehuis on 2017-04-18 10:46_

@scottchiefbaker just highlighted a problem with the color parsing (no surprises there). I've just squashed the fix into this commit and made a quick test script which can be used to check if the parsing code matches visually.

https://gist.github.com/tiehuis/15f6a85c1b8fe691008c10b4d325a7c3

---

_Comment by @scottchiefbaker on 2017-04-18 17:08_

I can confirm that I've tested this branch and it does everything I want for this feature. Colors are accurate and simple to specify. This pull-request gets my 👍 for merge. Good work @tiehuis 

---

_Comment by @scottchiefbaker on 2017-04-20 03:38_

@tiehuis I don't know enough (any) rust to speak the quality of this code, but I think the implementation and output are great.

What are your thoughts on this PR?

---

_Comment by @tiehuis on 2017-04-20 10:40_

I've added my notes about this in the initial post. It's up to BurntSushi whether it gets merged or not and if any changes need to be made.

I know he has a lot of other things to tend to as well so I usually don't want to be too overbearing. He does a great job at managing his various projects so just give him a bit of time and this'll get looked at eventually.

---

_Comment by @scottchiefbaker on 2017-05-01 16:22_

@BurntSushi any thoughts on this PR? It's working pretty well for me.

---

_Comment by @leafgarland on 2017-05-11 23:21_

I just wanted to add that the Windows 10 console has supported 256 ansi colours for a while and since Creators Update supports 24 bit RGB.
https://msdn.microsoft.com/en-us/library/windows/desktop/mt638032(v=vs.85).aspx

Other commonly used terminal emulators such as mintty and ConEmu also support 24 bit colours.

Since ripgrep already has a flag for ansi colours, please don't just disable by default on Windows. Windows users can opt in/out depending on what they are using. As an aside, ripgrep is roughly twice as fast on windows with `--color=ansi`.

---

_Comment by @BurntSushi on 2017-05-11 23:37_

> Since ripgrep already has a flag for ansi colours, please don't just disable by default on Windows.

@leafgarland Can you please elaborate? What does your comment have to do specifically with this PR?

---

_Comment by @BurntSushi on 2017-05-11 23:37_

@tiehuis Sorry I've been dragging my feet looking at this! Thanks so much for doing it! At first glance, this looks pretty good but I'll want to do a more thorough review before merging.

---

_Comment by @leafgarland on 2017-05-11 23:40_

@BurntSushi I was commenting because the description mentioned that this PR was defaulting to white on Windows.

---

_Comment by @tiehuis on 2017-05-11 23:44_

Can't have a long conversation right now but just had a quick look and I think this should handle ansi codes correctly on windows if `--color=ansi` is specified.

The only thing that should be updated potentially is the `should_ansi` function in termcolor which determines if the `AnsiWriter` is used instead of the Windows one. Not sure if this does a thorough enough check for a Windows 10 terminal. I think the description in the initial message would be better described as "Fall back to white on windows where ANSI is not supported (i.e. cmd.exe)".

Haven't tested at all on Windows however, so wanted a minimal change incorporating the functionality first. It should be simple to add support for Windows later if needed with no real trouble.

Would be great if you could try out the branch to confirm though!


---

_Comment by @BurntSushi on 2017-05-11 23:44_

@leafgarland That's only when printing using the console APIs. It looks like this PR doesn't do anything special with the ANSI codes when those are used on Windows (which seems correct to me).

---

_Comment by @leafgarland on 2017-05-11 23:45_

Ok, thanks. Sorry, I should have looked at the code!

---

_Comment by @BurntSushi on 2017-05-11 23:45_

> The only thing that should be updated potentially is the should_ansi function in termcolor which determines if the AnsiWriter is used instead of the Windows one. Not sure if this does a thorough enough check for a Windows 10 terminal

Yeah, this is potentially future work. Def out of scope for this PR. :-) AFAIK, you actually need to make an API call to enable ANSI support in Windows 10 terminals, so it requires some Windows knowledge.

---

_Comment by @weiznich on 2017-06-15 14:57_

What is the current state of this pull request? What needs to be done to get it merged?
(I would like to implement a feature based on this in cargo)

---

_Comment by @BurntSushi on 2017-06-15 15:23_

@weiznich It's waiting on me giving it a more careful review. I'll try to look soon.

---

_Comment by @scottchiefbaker on 2017-06-15 15:26_

@weiznich I looked at your PR for Rust, and I'm a little confused. What feature does that implement that's related to this? Or would it be a new feature for ripgrep you're waiting to implement?

---

_Comment by @weiznich on 2017-06-15 15:33_

@alexcrichton moved the coloring in cargo to termcolor with [this commit](https://github.com/rust-lang/cargo/commit/f8fb0a022853ab3f423889d442c695511ece3692). After that it is only possible to use the 8 predefined color codes. This may result in a colors that are barley readable(See my [old PR](https://github.com/rust-lang/cargo/pull/3873) for an example).
This PR introduces two possibilities to define custom colors. Based on this it should be possible to allow configuring the colors through an environment variable as proposed in my PR to cargo.

---

_Comment by @scottchiefbaker on 2017-08-22 21:10_

@BurntSushi have you had a chance to review this at all?

I'd really like to see this landed. 

---

_Comment by @scottchiefbaker on 2018-01-08 17:39_

@BurntSushi any movement on this? I'd really like to see better colors in ripgrep.

---

_Comment by @BurntSushi on 2018-01-29 23:27_

Thanks @tiehuis for this! I'm going to close this PR in favor of #766, which is based on your work here. I wanted to just get this merged so I did a few fixups.

---

_Closed by @BurntSushi on 2018-01-29 23:27_

---
