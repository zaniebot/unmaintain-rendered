---
number: 5370
title: "(clap_complete) Make `Shell` ValueEnum Parser also Accept $SHELL Output (Path Prefixed Shell Names) "
type: issue
state: closed
author: ultrabear
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-completion
assignees: []
created_at: 2024-02-20T23:39:34Z
updated_at: 2024-08-12T17:25:22Z
url: https://github.com/clap-rs/clap/issues/5370
synced_at: 2026-01-10T01:28:10Z
---

# (clap_complete) Make `Shell` ValueEnum Parser also Accept $SHELL Output (Path Prefixed Shell Names) 

---

_Issue opened by @ultrabear on 2024-02-20 23:39_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

clap v4.5.1

### Describe your use case

I am developing a CLI using clap and have added completions support under `cmdname completions <SHELL>`, this uses clap_complete::Shell ValueEnum parsing.
When trying to document how to use these completions, I tried writing:
```bash
# to add completions for common shells (bash/zsh/fish) simply write
cmdname completions $SHELL | source
```

Basically, the issue is that `Shell` only accepts the name of the shell alone, and the $SHELL env var is a path on my system (in my case its `/bin/fish`).

### Describe the solution you'd like

The solution I came up with was modifying the ValueEnum trait implementation of Shell to make from_str split out the last / (probably best done with str::rsplit_once) and then parse the value after that:
```rs
    fn from_str(mut input: &str, ignore_case: bool) -> Result<Self, String> {
        input = input.rsplit_once('/').map_or(input, |(_, after)| after);
        ... 
        // insert basically the default impl after this, though it would be nice if it didn't
        // need to be copied, trying to handwrite it to use a match is a bad experience, the 
        // root of the issue being that eq_ignore_case is a clap internal api, which is feature 
        // flagged on the unicode feature: that feature would need to be added to clap_complete 
        // to handwrite it fully. copying the current implementation is the lowest impact change
    }
```

### Alternatives, if applicable

implementing TypedValueParser or another higher level method on Shell instead, this seems like considerably more work even if it may be more correct than "hacking" the way that ValueEnum is normally meant to be used

### Additional Context

Initially I was going to write a PR for this, but after seeing the clap unicode feature (generally just handling ignore_case without copy pasting the default impl) blocked a "simple" solution I decided to write up an issue to gauge interest instead, if any of the proposed solutions looks good to maintainership I can totally write the functionality in myself as a PR (I want this feature afterall), I just didn't want to jump the gun on something that may affect maintainability depending on how its implemented

---

_Label `C-enhancement` added by @ultrabear on 2024-02-20 23:39_

---

_Label `A-completion` added by @epage on 2024-02-21 01:19_

---

_Comment by @epage on 2024-02-21 01:25_

Closing as a duplicate of #4300 which was implemented in #4447.  If after reading the background in and linked to from #4300, you think we should revisit this, I'd recommend saying so in there and we can re-evaluate opening that issue.

---

_Closed by @epage on 2024-02-21 01:25_

---

_Comment by @ultrabear on 2024-02-21 01:38_

Uh, this is not actually a duplicate of that issue? That issue adds a method to implicitly parse Shell from the env, but does not add it to the ValueEnum implementation

---

_Comment by @epage on 2024-02-21 01:40_

The PR did that but the issue was more broad and was spawned from #4296.

---

_Comment by @ultrabear on 2024-02-21 01:43_

Yeah, I see that was closed as not planned, but that was because it was reading the env, no?
The proposed implementation here would be so that users may pass $SHELL themselves, have their shell expand it, and then have eg `/bin/fish` be parsed as a valid shell by Shell::from_str (it does no env var reading, and crucially Just Works with the derive API when you mark something with the type Shell)

---

_Comment by @ultrabear on 2024-02-21 01:49_

If the conclusion here is that the implicit Shell::from_env method should be used, then thats fine, but personally I dont like reading env vars without being obvious about it, and this feature req aims to have less implicit state handled by the resulting binary, while still retaining a serviceable api

---

_Comment by @epage on 2024-02-21 02:15_

> If the conclusion here is that the implicit Shell::from_env method should be used, then thats fine, but personally I dont like reading env vars without being obvious about it, and this feature req aims to have less implicit state handled by the resulting binary, while still retaining a serviceable api

We do provide [`Shell::from_shell_path`](https://docs.rs/clap_complete/latest/clap_complete/shells/enum.Shell.html#method.from_shell_path).

---

_Comment by @epage on 2024-02-21 02:16_

> Yeah, I see that was closed as not planned, but that was because it was reading the env, no?
The proposed implementation here would be so that users may pass $SHELL themselves, have their shell expand it, and then have eg /bin/fish be parsed as a valid shell by Shell::from_str (it does no env var reading, and crucially Just Works with the derive API when you mark something with the type Shell)

Yes, the difference is in whether the application reads `SHELL` directly or gets it via the command-line.  That is probably enough distinction to keep this separate.

---

_Reopened by @epage on 2024-02-21 02:16_

---

_Label `S-waiting-on-decision` added by @epage on 2024-02-21 02:16_

---

_Comment by @epage on 2024-02-21 02:19_

This can be worked around by using `Shell::from_shell_path` inside of a value parser function.  It would take some work to also have completions work.

Whether we provide this is all or nothing, so I would like to sit on this a bit to see what input we receive to get a better idea of whether this is a good direction to go.

If we go this route, it isn't as simple as putting this in the `FromStr`.  We'd have to write a custom `TypedValueParser` impl as that is what is used for parsing the argument, not `FromStr`.

---

_Comment by @ultrabear on 2024-02-21 02:32_


> If we go this route, it isn't as simple as putting this in the `FromStr`. We'd have to write a custom `TypedValueParser` impl as that is what is used for parsing the argument, not `FromStr`.

To be clear, I meant overriding the method in ValueEnum called from_str, not FromStr (which isn't implemented for Shell regardless)
This lets us keep the value hinting, if I understand how the machinery all fits together

However, as I mentioned above, this feels a little hacky for what ValueEnum is normally meant to be used for

---

_Comment by @epage on 2024-08-12 17:25_

I've added a new way to register shell completions in #5671 and it supports parsing `$SHELL`.  I lean towards closing in favor if that being our preferred solution.  If there is a reason for us to re-evaluate this, let us know!

---

_Closed by @epage on 2024-08-12 17:25_

---
