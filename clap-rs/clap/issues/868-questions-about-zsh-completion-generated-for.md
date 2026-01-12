```yaml
number: 868
title: Questions about zsh completion generated for ripgrep
type: issue
state: closed
author: kastiglione
labels:
  - C-enhancement
  - A-completion
assignees: []
created_at: 2017-02-21T23:38:19Z
updated_at: 2018-08-02T03:30:02Z
url: https://github.com/clap-rs/clap/issues/868
synced_at: 2026-01-12T16:14:10Z
```

# Questions about zsh completion generated for ripgrep

---

_@kastiglione_

### Rust Version

N/A

### Affected Version of clap

2.20.5 

### Expected Behavior Summary

Case 1:

`rg <TAB>`

will either complete flags, or complete nothing (because completing a search pattern isn't feasible)

Case 2:

`rg somepattern <TAB>`

will complete a path

### Actual Behavior Summary

Case 1:

`rg <TAB>`

will produce two incorrect completions: `PATTERN`, `PATH`.

Case 2:

`rg somepattern <TAB>`

will produce flag/option completions (completion path is expected)

### Steps to Reproduce the issue

Use ripgrep's build to generate the zsh completion script.

See: https://github.com/BurntSushi/ripgrep/blob/3f515afbb4cb3d22520729453bea5af050c0eb00/build.rs

### Sample Code or Link to Sample Code

Here is the generated zsh completion for `_rg`:

https://gist.github.com/kastiglione/c9e09a89ca547d1f2288a62af7a8f439

### Debug output

N/A

### Reference

https://github.com/BurntSushi/ripgrep/issues/375

---

_Comment by @kbknapp on 2017-02-22 03:41_

I think this has to do with ZSH completion scripts in general and not specifically how clap is generating them. Of course, if I'm wrong I'm more than happy to fix or be told where the actual error lies.

What I observe (appears correct to me):

```
$ rg <tab>
PAT

$ rg -<tab>
[.. list of flags/options]

$ rg --<tab>
[ .. list of flags/options that start with --]
```

Of course, completing paths is perhaps possible, but not everyone is accepting a path as an argument. Bash *does* however fallback to path completion when nothing else fits (I'm unsure of how to do this in Zsh though).

The final bit is I'm working on allowing arbitrary Rust code to run during a completion (Issue #568) but it hasn't been implemented yet. I'm also not sure which shells will allow this and which ones won't.

---

_Label `T: RFC / question` added by @kbknapp on 2017-02-22 03:41_

---

_Comment by @kbknapp on 2017-02-22 04:03_

As for completing `$ rg somepattern <tab>`, yes, this should be either the literal `PATH` (see comments above) or list options/flags if no other positional/free args exist. However, unlike in Bash I'm not aware of a way to say, "This arg comes first, this one second, etc." in Zsh. If someone knows a way, I'd love to hear it! (Seriously. I'm not being facetious)

---

_Comment by @kbknapp on 2017-02-22 04:23_

I forgot to say it above, but

> Case 1:
> `rg <TAB>`
> will produce two incorrect completions: `PATTERN`, `PATH`.
> [ ... ] It should produce either options/flags or nothing (because completing a pattern isn't feasible)

I think comleting nothing is highly subjective, as some would then assume completion scripts are totally broken at that point. As for completing flags/options, I believe ZSH requires a leading `-` to complete flags/options (unless no positiona/free args exist, or all have been used). This isn't something clap is choosing.

The same can be seen if you try:

```
$ cp <tab>
[ .. files ]

$ cp -<tab>
[.. options/flags]
```

---

_Comment by @BurntSushi on 2017-02-22 12:06_

> As for completing $ rg somepattern <tab>, yes, this should be either the literal PATH (see comments above) or list options/flags if no other positional/free args exist.

I think I'm with you on everything except for this. It seems like the *expected* behavior from a user's perspective in this case would be to complete file paths instead? Or will it start completing file paths if you type `$ rg somepattern ./<tab>`?

---

_Comment by @kbknapp on 2017-02-22 13:55_

I totally agree, but there's not a way for clap to know, "this positional arg accepts file paths" and to therefore tell ZSH such. Right now clap is dumb and just tells ZSH to complete the arg name since it can't know what that arg actually represents. #568 would remedy that. 

There's also the possibility of adding either a special case to clap to use file path completion based on arg names or to have a standard set of completion possibilities in lieu of #568 such as `Completion::RelativeFilePath` where a particular she'll may or may not support such (in which case the shell would just ignore it).

I'm less inclined to support special casing based on arg names, but adding a specific setting that tells clap what to generate may be doable.

---

_Comment by @kbknapp on 2017-02-22 13:59_

I should also say the positional arg index issue listed above (telling ZSH which args are which indexes) would also need to be fixed...And the ZSH completion documentation is..."Interesting" to say the least.

Finally, if there's a similar method to bash of which I'm currently unaware that says to fallback file path completion, that would work as well.

---

_Comment by @kastiglione on 2017-02-23 23:16_

I'm rather unfamiliar with the context of clap, and the context of bash. But a positive I can bring to the discussion is how other similar completions behave.

The completion for `grep` does the following:

```
$ grep <tab>
[ no output / bell ]

$ grep somepattern <tab>
[ .. files ]
```

The `_rg` completion provided by [zsh-completions](https://github.com/zsh-users/zsh-completions), also follows this behavior of no completion for the pattern (first positional argument), completing files after (second positional argument).

I love that this functionality exists. Since we're looking at behavior, I'd love it if clap could do one thing that the other [rg completion](https://github.com/zsh-users/zsh-completions/blob/72af5d08f1c07507d74103af039e98a2791fccb5/src/_rg) does: complete file types for the `-t` option.

---

_Renamed from "Zsh completion generated for ripgrep is not usable" to "Questions about zsh completion generated for ripgrep" by @kastiglione on 2017-02-24 06:05_

---

_Comment by @kastiglione on 2017-02-24 06:07_

In hindsight the original title I used was a bit pompous, so I've tried to make it a little better. Sorry about that, I thought the behavior was unexpected. It's mainly the lack of file completion that led me to say it's not usable, since I had come to expect that functionality from the zsh-completions implementation.

---

_Comment by @kbknapp on 2017-02-24 14:56_

A custom made completion script will nearly always be better than a purely generic auto-generated one. There are pros and cons to both approaches. A custom script must ALWAYS be kept up to date with changes to the base program, whereas an auto-generated one doesn't have that issue at the expense of being slightly more generic.

I do think there is some things we can do to make our ZSH completions better, such as specifying *what* needs to be completed. I'm also going to start looking back into the index issue. So I'll mark this is a improvement issue and post back here with whatever I find :wink:

---

_Comment by @kbknapp on 2017-02-24 14:58_

In fact, when I originally started implementing the auto-generation scripts I intended them to be used as a base script that could then be customized a little. I.e. just get the boiler plate out of the way and I'll add the last 5% polish. But it turned out they worked good enough (minus this issue you've encountered) that adding that extra 5% polish wasn't really worth it given the drawbacks of having to stay in sync again, over what you actually gain.

I think in this case in particular we can close that gap a few more percent!

---

_Label `C: completion gen` added by @kbknapp on 2017-02-24 14:59_

---

_Label `C: positional args` added by @kbknapp on 2017-02-24 14:59_

---

_Label `D: intermediate` added by @kbknapp on 2017-02-24 14:59_

---

_Label `P3: want to have` added by @kbknapp on 2017-02-24 14:59_

---

_Label `T: enhancement` added by @kbknapp on 2017-02-24 14:59_

---

_Label `W: 2.x` added by @kbknapp on 2017-02-24 14:59_

---

_Label `T: RFC / question` removed by @kbknapp on 2017-02-24 14:59_

---

_Comment by @kbknapp on 2017-02-24 15:00_

> I love that this functionality exists. Since we're looking at behavior, I'd love it if clap could do one thing that the other rg completion does: complete file types for the -t option.

This should be totally possible if I add that `CompleterValue` type enum. Stay tuned to see how this goes!

---

_Label `C: positional args` removed by @kbknapp on 2017-02-24 15:04_

---

_Comment by @kbknapp on 2017-02-24 18:46_

@kastiglione I hadn't seen the `zsh-completions` page before and they actually have some great tips for how I could improve the generated ZSH script. So thanks for that link! 

Here's my thoughts on what I could realistically provide over the next few days (and longer):

 - [ ] Improve the positional/free argument indexes (I'd actually go so far as to say this would be a bug-fix)
 - [ ] Add some sort of `CompletionValue` enum with some generic options and be passed to a `Arg::completion_value`. Some of these values will be supported by some shells, but not others and that's OK.
 - [ ] Longer term (my time will be the limiting factor here) add #568 implemented via passing a function/closure to `Arg::complete_with` which has a different signature such as `|clap::Shell, &ArgMatches, &str| -> String` i.e. something with more context about what is trying to be completed (think completing different values based off what has already been passed to the command line in previous args)


---

_Label `W: 3.x blocker` added by @kbknapp on 2017-05-09 19:00_

---

_Label `W: 2.x` removed by @kbknapp on 2017-05-09 19:00_

---

_Referenced in [BurntSushi/ripgrep#496](../../BurntSushi/ripgrep/pulls/496.md) on 2017-05-29 20:07_

---

_Comment by @segevfiner on 2018-01-06 09:28_

The way, I think, you are supposed to handle positional arguments that you don't know how to complete is to complete nothing  but popup a message, or better yet, default to completing files (Which is what most bash completion scripts do by using `-o default`). Like this (For `_arguments`, not `_describe` that is used now):
```
:Some message that explain that you need to enter X:()

# or

:Some message that explain that you need to enter X:_files
```

This is what most completions that I know of do. Completing the metavar/placeholder is sub-optimal it's never a valid argument.

For arguments that you do know what the completions are, you should complete that, of course. If you know that an argument doesn't take files than completing nothing with a message is better of course. But if you don't know, it's best to default to completing files since that's common and often less aggravating than trying to complete a file and getting nothing.

---

_Referenced in [clap-rs/clap#1147](../../clap-rs/clap/pulls/1147.md) on 2018-01-14 05:13_

---

_Comment by @kbknapp on 2018-02-05 16:19_

This should be closed - the generated zsh script even for ripgrep is working and falls back to file completion as one would expect (this is all thanks to @segevfiner !)

---

_Closed by @kbknapp on 2018-02-05 16:19_

---

_Referenced in [BurntSushi/ripgrep#775](../../BurntSushi/ripgrep/pulls/775.md) on 2018-02-05 16:22_

---
