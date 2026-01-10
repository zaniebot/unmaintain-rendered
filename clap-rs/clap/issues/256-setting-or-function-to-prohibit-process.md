---
number: 256
title: Setting or function to prohibit process termination
type: issue
state: closed
author: Drakulix
labels:
  - A-parsing
assignees: []
created_at: 2015-09-17T20:37:30Z
updated_at: 2015-10-29T07:02:08Z
url: https://github.com/clap-rs/clap/issues/256
synced_at: 2026-01-10T01:26:26Z
---

# Setting or function to prohibit process termination

---

_Issue opened by @Drakulix on 2015-09-17 20:37_

I would like to use clap's easy syntax for different stages in my program then just command line argument parsing. E.g. to parse remote commands with arguments read from sockets. I can already utilize clap using get_matches_from. But even get_matches_from_safe terminates my program when --help or --version gets passed to. I would like to have either an even safer _safe method or an app-setting, that completely blocks any terminate inside the clap crate and some kind of indication, if parsing did succeed or it should have terminated.

Would that be possible?

Best regards,
Drakulix


---

_Comment by @kbknapp on 2015-09-17 22:30_

This is possible, we'll have to look at the best naming and such in order to implement. The work-around is simple, but somewhat hacky at the moment.

Simply define your own `--help` and `--version` flags as part of your valid arguments. If you provide a flag with the long of `help` or `version` then `clap` will not auto-generate those and leave the implementation up to you (i.e. you decide whether to terminate or not, and what should be done). 

The same applies to subcommands with the name of `help`.

Hope this answers your question. We'll update this issue as decisions are made and discussed about adding a "safer" safe :smile: 


---

_Label `T: RFC / question` added by @kbknapp on 2015-09-17 22:31_

---

_Label `T: new feature` added by @kbknapp on 2015-09-17 22:31_

---

_Label `P4: nice to have` added by @kbknapp on 2015-09-17 22:31_

---

_Label `C: args` added by @kbknapp on 2015-09-17 22:31_

---

_Label `D: easy` added by @kbknapp on 2015-09-17 22:31_

---

_Label `C: parsing` added by @kbknapp on 2015-09-17 22:31_

---

_Renamed from "[Feature Request] setting or function to prohibit process termination" to "Setting or function to prohibit process termination" by @kbknapp on 2015-09-17 22:31_

---

_Label `C: args` removed by @kbknapp on 2015-09-17 22:33_

---

_Comment by @Drakulix on 2015-09-17 22:50_

Thanks for looking into this. That workaround will probably be enough for the moment.


---

_Comment by @kbknapp on 2015-09-17 22:54_

One other thing to note (although I haven't  tested it) is _also_ setting the `help` and `version` flags to hidden (if you'd rather not see them, or they aren't applicable to your program).


---

_Comment by @sru on 2015-09-18 00:37_

How about having an option for default help or version?


---

_Comment by @kbknapp on 2015-09-18 00:46_

Or the opposite of having an option to disable default help and version?
Since I'd assume most would want it.

On Thu, Sep 17, 2015, 8:37 PM SungRim Huh notifications@github.com wrote:

> How about having an option for default help or version?
> 
> —
> Reply to this email directly or view it on GitHub
> https://github.com/kbknapp/clap-rs/issues/256#issuecomment-141296225.


---

_Comment by @sru on 2015-09-18 13:23_

That works too. I took a brief look at the `app/app.rs` file, but I couldn't find where the code is adding the help and version, and where the code is terminating.


---

_Comment by @WildCryptoFox on 2015-09-18 14:00_

@sru Also a brief look at the same file. Grep style.

## Instances of 'exit' in src/app.rs
- https://github.com/kbknapp/clap-rs/blob/master/src/app/app.rs#L1949 << "get_matches_from"
- https://github.com/kbknapp/clap-rs/blob/master/src/app/app.rs#L2324
- https://github.com/kbknapp/clap-rs/blob/master/src/app/app.rs#L2753
- https://github.com/kbknapp/clap-rs/blob/master/src/app/app.rs#L2761
- https://github.com/kbknapp/clap-rs/blob/master/src/app/app.rs#L2783
- https://github.com/kbknapp/clap-rs/blob/master/src/app/app.rs#L2795

```
~kbknapp/clap-rs $ grep -in exit src/app/app.rs                                                                                                                                               
346:    /// Allows specifying that if no subcommand is present at runtime, error and exit gracefully
505:    /// Specifies that the help text sould be displayed (and then exit gracefully), if no
630:    /// exiting
659:    /// Specifies that the help text sould be displayed (and then exit gracefully), if no
669:    /// still be displayed and exit. If this is *not* the desired result, consider using
1146:    /// failure (graceful exit).
1210:    /// failure (graceful exit).
1842:    // Prints the version to the user and exits if quit=true
1949:                process::exit(1);
2324:                            process::exit(0);
2575:                    e.exit();
2753:                process::exit(0);
2761:                process::exit(0);
2783:            process::exit(0);
2795:            process::exit(0);
```

## Source of help and version
- https://github.com/kbknapp/clap-rs/blob/master/src/app/app.rs#L2747 "check_for_help_and_version" and callees.

---

The help and version do not appear to be added to the App. Instead, just assumed in the runtime.

---

Spelling mistakes detected at 505, 659. :wink: Also. Wow this file is large :ghost: 


---

_Comment by @sru on 2015-09-19 01:54_

@james-darkfox Wow. I didn't think of using `grep`. I should try to actually use these things.

@kbknapp I have several other ideas as well concerning the general process termination (not the help and version specific).

Just like the title of this issue, we could have a setting to prohibit process termination and wrap every `process::exit(...)` with `if`s.

I am a bit wary with the `process::exit()` because the [documentation](https://doc.rust-lang.org/std/process/fn.exit.html) says:

> Note that because this function never returns, and that it terminates the process, no destructors on the current stack or any other thread's stack will be run.

So, another idea is that let the user handle the error entirely. In this case, we could delegate the error message to `ClapError` or add helper methods in `ClapError` to simulate same functionality.


---

_Comment by @WildCryptoFox on 2015-09-19 03:50_

@sru I also vote towards `ClapError`. +1


---

_Referenced in [clap-rs/clap#259](../../clap-rs/clap/issues/259.md) on 2015-09-19 10:37_

---

_Comment by @Drakulix on 2015-09-20 21:48_

I also vote for ClapError. I do not need to disable the help and version flags and returning a ClapError instead of Process-Termination is just what I need to handle that special case.


---

_Comment by @kbknapp on 2015-09-20 23:06_

We could move it to `ClapError`, the only downside I see is if the user uses `ClapError::exit()` it will get printed to `stderr`. Perhaps using a private bool `ClapError::is_fatal()` to determine if it shoudl go to stderr or stdout similiar to what I've seen in some of BurntSushi's projects?

Thoughts?


---

_Comment by @sru on 2015-09-21 01:19_

@kbknapp Is there an example in `clap` on non-fatal errors?


---

_Comment by @kbknapp on 2015-09-21 01:41_

`is_fatal()` is from the BurntSushi examples, we'd probably adopt it to something like `use_stderr()`


---

_Comment by @sru on 2015-09-21 06:37_

@kbknapp I meant to say how does clap determine if the error should go to stderr or stdout. Or is it like user chooses where the error output should go?


---

_Comment by @WildCryptoFox on 2015-09-21 06:48_

@sru One fatal context I've noticed is `SubcommandRequiredElseHelp`. I'm pretty sure the usage here goes to stderr. However `fake --help` should go to stdout.


---

_Comment by @kbknapp on 2015-09-21 22:59_

@james-darkfox yep that's the intention I had. 


---

_Added to milestone `1.4.4` by @kbknapp on 2015-09-30 18:48_

---

_Added to milestone `1.4.5` by @kbknapp on 2015-10-03 17:25_

---

_Removed from milestone `1.4.4` by @kbknapp on 2015-10-03 17:25_

---

_Added to milestone `1.4.6` by @kbknapp on 2015-10-06 19:26_

---

_Removed from milestone `1.4.5` by @kbknapp on 2015-10-06 19:26_

---

_Comment by @kbknapp on 2015-10-28 13:59_

@Drakulix sorry this has taken literally forever - #326 implements this. See the PR for details :wink: 


---

_Assigned to @kbknapp by @kbknapp on 2015-10-28 14:00_

---

_Comment by @Drakulix on 2015-10-28 17:31_

Thanks. This is exactly the behavior I expected at first, when using the safe-function. Hopefully this will  be also more intuitive and useful to other users as well. Looking forward to the actual merge!


---

_Closed by @kbknapp on 2015-10-29 07:02_

---
