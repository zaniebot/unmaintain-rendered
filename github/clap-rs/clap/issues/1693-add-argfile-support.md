---
number: 1693
title: Add argfile support
type: issue
state: closed
author: XAMPPRocky
labels:
  - C-enhancement
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2020-02-14T11:09:33Z
updated_at: 2021-12-10T16:09:57Z
url: https://github.com/clap-rs/clap/issues/1693
synced_at: 2026-01-07T13:12:19-06:00
---

# Add argfile support

---

_Issue opened by @XAMPPRocky on 2020-02-14 11:09_

Maintainer's notes:
- Prior art:
  - [javac's documentation for accepting `@`](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/javac.html#commandlineargfile)
  - [argparse's `fromfile_prefix_chars`](https://docs.python.org/3/library/argparse.html#fromfile-prefix-chars)
  - [description of rg](https://github.com/clap-rs/clap/issues/1693#issuecomment-621182914)
  - [Microsoft Response Files](https://docs.microsoft.com/en-us/cpp/build/reference/at-specify-a-compiler-response-file?view=msvc-170)
---
### Describe your use case

I've received requests (XAMPPRocky/tokei#437) to add argfile support to `tokei`, it would be nice if this was provided in clap so that it can be shared and reused by all crates.

Argfiles aren't a specific standard, and there are varying levels of support. The best documentation I found on it was documentation on [`javac`'s CLI](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/javac.html).

### Describe the solution you'd like

### CLI

```
tokei @argfile
```

### Rust

```
App::new("My Super Program").argfile(true)
```


---

_Label `T: new feature` added by @XAMPPRocky on 2020-02-14 11:09_

---

_Label `C: args` added by @pksunkara on 2020-02-14 11:34_

---

_Label `C: parsing` added by @pksunkara on 2020-02-14 11:34_

---

_Label `P4: nice to have` added by @pksunkara on 2020-02-14 11:34_

---

_Label `D: easy` added by @CreepySkeleton on 2020-02-14 12:00_

---

_Label `good first issue` added by @CreepySkeleton on 2020-02-14 12:00_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-14 12:00_

---

_Label `M: mentored` added by @CreepySkeleton on 2020-02-14 12:12_

---

_Comment by @CreepySkeleton on 2020-02-14 12:32_

Here's a formal description for the sake of possible implementors:
* `App::new("My Super Program").global_setting(AppSettings::AllowArgfiles)` tells clap to look after **positional** arguments starting with `@`.  These args will be treated as `@argfiles`.
* `@argfiles` can only be preceding so explicit cli options override them, i.e it's not possible to do something like `app -J --foo @argfile1 bar @argfile2 baz`. This fact should simplify the implementation considerably. 
* Each `@argfile` is treated as a list of command line options/args. In fact, the args simply inserted into processing queue in place of `@argfile` occurrence.
  * One line corresponds to one arg. The following `@argfile` corresponds to three args.
  ```
  -J foo
  Bar
  "rm -rf /"
  ```
  * Each line is treated **verbatim**. Nothing is stripped, no escapes.
  * If the file end with a newline character it will be stripped (but only one!).
  * If a file is not found - it's a parsing error.

---

_Comment by @pksunkara on 2020-02-17 09:22_

So, with #1697, I rewrote how input is being parsed. That would enable us to use `@argfile`s even in the middle.

---

_Referenced in [clap-rs/clap#1697](../../clap-rs/clap/pulls/1697.md) on 2020-02-17 09:32_

---

_Comment by @xadaemon on 2020-02-26 07:18_

@pksunkara are you working on this or can I give it a go?

---

_Comment by @pksunkara on 2020-02-26 09:09_

Please go ahead.

---

_Comment by @xadaemon on 2020-02-26 15:57_

I will implement to @CreepySkeleton's spec then.

---

_Comment by @pksunkara on 2020-02-26 15:59_

If the App is taking an argfile method, then it's better if it's implemented for all subcommands instead of just at the end.

---

_Referenced in [clap-rs/clap#748](../../clap-rs/clap/issues/748.md) on 2020-03-03 13:07_

---

_Comment by @pickfire on 2020-03-07 15:47_

How about `@argfile` as a value of some commands, like `curl -F f:1=@file ix.io`?

edit: this looks different from what I thought

---

_Comment by @XAMPPRocky on 2020-03-08 00:21_

@pickfire Well since `@file` is the value in that case I would expect my app to handle that rather than the cli library. I've only seen that in the context of curl so I don't know if there's wider consensus on what it should do. It probably needs a seperate issue.

---

_Referenced in [clap-rs/clap#1519](../../clap-rs/clap/issues/1519.md) on 2020-04-09 08:06_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 08:16_

---

_Comment by @michael-smythe on 2020-04-09 13:31_

@pksunkara Glad too see this is being worked on! I just wanted to add a comment since this is closing out #1519. I think the spec described by @CreepySkeleton is a really good step and a useful starting point. One thing that I think it lacks is the ability to have comments within your config file. This would be useful if you have a large number of options that are presented by the app, and it would allow end-users to build very readable configs. I could see it being annoying to open up a config file to slightly alter how the app is running and not remembering exactly what each option does. With the ability to comment and/or use the actual `Arg::with_name("name")` to control the option/switch/flag within the config I believe it would increase the usability for the end-user. That being said any progress towards config files is good in my book, just something to consider for the long term. Thanks!

---

_Comment by @kbknapp on 2020-04-28 14:25_

Couple of thoughts:

* I like that this is gated behind an `AppSettings` variant because, although rare `@foo` does come up in CLIs. I wouldn't want us supporting argfiles to break some CLI. The cross section of CLIs that use `@foo` *and* want to use this new feature is probably so minimally niche that its fine that there is a minimal conflict here.
* Comments is probably easy enough to implement. Any line starting with `#` is ignored. Inline comments `-J=foo #this describes something` is not allowed. Having to scan each line and split them is not worth it IMO.
* Likewise blank lines should probably be ignored as well. 
* I'm also curious if anyone can think of something that breaks or gets complicated by ignoring blank lines or `#` lines?
* I'm still not sure how I feel about clap handling file I/O during parsing. My main concern is two fold:
  * Code bloat (could be handled by a cargo feature flag, but then at that point do we need the `AppSettings`? Maybe still yes, just to make the parsing more simple with a branch. I'd be fine either way.)
  * All the idiosyncrasies that pop up with files (permissions, encodings, etc.)...which in turn leads back to sub-bullet 1 (code bloat)
* Something I didn't see spelled out in the spec above is, are these `@argfile`s treated as positional arguments i.e.:
  * do they show up in the help message, and if so, how? (I'd say they go in the usage string, but nothing more)
  * Is the user expected to make a `Arg` place holder for the argfile? (I'd say no)

---

_Comment by @CreepySkeleton on 2020-04-28 18:39_

> I like that this is gated behind an AppSettings variant 

Agreed. Whatever we'll end up with, it must be opt-in.

I have a bad feeling about comments/blank lines. 

* I don't remember clearly, but a few years ago I was working on a CLI app which was receiving URL parts through command line args (among other things), like `docs.rs #anchor`. As you see, there are (possibly rare) cases where arguments are expected to start with `#`. I'd say I'm OK with this part because it's too rare to really be accounting for. Maybe "`#` plus an optional space between `#` and the comment".
* I would absolutely expect empty lines to be processed as empty values. It's pretty common - not common-common, but I saw quite a few of cases - where this kind of values is used to override previous occurrences, especially in combination with `alias`.

  Imagine I've got a `a` script in my `PATH`. This script is literally `alias a='application-with-long-name --option value'` because I want this `--option` to have `value` 99% of time, but occasionally
I want an explicitly empty string there (maybe it's some sort of annotation). Not really sure people would use `@argfile` for this kind of thing, just a thing to consider.

  * I am purposely not expressing the difference between "empty lines" (`\n\n`) and "blank lines" (`\n \t \n`, ASCII whitespace only). They should treated in the same manner - passed over "as is".

* [The link to javac spec](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/javac.html#commandlineargfile) doesn't mention empty lines and comments, too.

* These are _argfiles_, not configs. What comments can be used for there?

> All the idiosyncrasies that pop up with files (permissions, encodings, etc.)...which in turn leads back to sub-bullet 1 (code bloat)

Argfiles can be used only on command line. If the user typed `@file`, this is a sort of contract: "This file exists and accessible". If that is not so, the user made a mistake, return `ErrorKind::ArgfileInaccessible` and render it as `Can't read the '<path>' argfile: <error from OS>`.

Encoding: UTF-8 only. If you're using something else nowadays, it's either some kind of database (argfiles aren't), or this is blasphemy.  Send the possessed hard drive to Vatican, the holy priests will perform an appropriate exorcism ritual on it and erase the possessed file. Seriously, who uses non-UTF-8 encodings in files, still?

> do they show up in the help message, and if so, how? (I'd say they go in the usage string, but nothing more)
>  Is the user expected to make a Arg place holder for the argfile? (I'd say no)

Agreed on both, an `AppSetting` is enough.

---

_Comment by @kbknapp on 2020-04-28 19:41_

> [..] there are (possibly rare) cases where arguments are expected to start with `#` 

Good point on the argument values that begin with `#`. However, that would only possibly apply to positional arguments, because `--foo=#bar` wouldn't be parsed as a comment with explicit disallowing of inline comments. Pragmatically, if encountering this error, I would change the line to `"#value"` and see if that fixed the issue. Either way I think it's such a small area that this little hiccup shouldn't devalue it.

As for what value comments have in an argfile, I'd agree probably not too much but can still see the use case where a complex CLI ships a default argfile but also comments what each switch/option is doing. I know in my day job there are instances where we ship some gnarly incantations where comments would absolutely help future workers quickly understand what's there.

> I would absolutely expect empty lines to be processed as empty values. [..]

Again, that's just for positional values, so overriding a positional value isn't a good idea just in the general sense. I'd also fall back to, if this was truly the outcome I expected and my argfile wasn't doing what I thought it would, my initial guess would be to make the line `""`. Like Rust, I'd prefer explicit over implicit. Special white space (`\n\t\n` vs `\n\n`) :vomiting_face: haha

> Seriously, who uses non-UTF-8 encodings in files, still?

Windows. :cry: Dealing with UTF-16, WTF-8, and having to do things like BOM sniffing doesn't sound super appealing to an argument parser unless it's well gated and opt-in only :stuck_out_tongue_winking_eye: I think @BurntSushi (sorry for the ping) has had some very strong thoughts on dealing with encodings on Windows.

---

_Comment by @BurntSushi on 2020-04-29 12:50_

It sounds like the argfile support being discussed here is roughly where I landed for ripgrep's configuration file:

* [Specification in the guide.](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file)
* [Implementation](https://github.com/BurntSushi/ripgrep/blob/a2e6aec7a4d9382941932245e8854f0ae5703a5e/crates/core/config.rs)
* [Handling invalid UTF-8 on Unix](https://github.com/BurntSushi/ripgrep/blob/a2e6aec7a4d9382941932245e8854f0ae5703a5e/crates/core/config.rs#L129-L152)
* [Rejecting invalid UTF-8 on Windows](https://github.com/BurntSushi/ripgrep/blob/a2e6aec7a4d9382941932245e8854f0ae5703a5e/crates/core/config.rs#L155-L168)

With respect to encoding, I wanted to treat the contents of the file as an `OsStr`. Doing this on Unix is very easy, since it's just arbitrary bytes. But doing it on Windows is a bit trickier. In retrospect, I think just declaring that it must be valid UTF-8 is fine. It's true that Windows has UTF-16 files, but it also has UTF-8 files and I think it's fine to declare the encoding as UTF-8. e.g., When writing a configuration file for git on Windows, can you use UTF-16?

Otherwise, I think the single biggest downside to this approach is that the file fomat does not support environment variables. This has been reported as an annoyance, but adding support for them will require a more complicated format. (You probably need some kind of escaping at that point.)

---

_Comment by @kbknapp on 2020-04-29 14:00_

> here is roughly where I landed for ripgrep's configuration file

Thank you!

> [..] I think just declaring that it must be valid UTF-8 is fine

I think this is where I'm landing too. It's also historically how some of my decisions have gone, so I'm ok with this so long as it's documented somewhere.

> Otherwise, I think the single biggest downside to this approach is that the file fomat does not support environment variables. 

This might be slightly different in clap's case becase of the intrinsic support for env vars already. A hypothetical argfile (ignoring the need for some escape):

```
--foo=SOME_ENV_VAR
```

Is semantically equivilant to `foo_arg.env("SOME_ENV_VAR")` to the point that there is no benefit listing it in the argfile, minus maybe just being super verbose/explicit? It's also possible I'm overlooking some angle :stuck_out_tongue_winking_eye: 

---

_Comment by @BurntSushi on 2020-04-29 14:11_

That requires one to tell clap to allow using an env var though right? If you don't, users might still want to use their own env vars, or use them to build more values. e.g., `--ignore-file=$HOME/something.ignore`. For example: https://github.com/BurntSushi/ripgrep/issues/1484#issuecomment-586458477

---

_Comment by @kbknapp on 2020-04-29 14:33_

Ah yes, you're correct. I was assuming the CLI creator added the `env("..")` to an argument. In the case they didn't (which is probably the most likely assumption) then yes, an argfile would allow the CLI *user* to specify a var to pull the value from.

Hmm...

So my initial thought is while I can see the value in that, I also think it may lead to some gotchas when that env var is unset, etc. In bash it's easy to use a var or default to some value, but being honest I have little interest in trying to account for all those edge cases in  an argfile format. I think I'm leaning towards just saying supporting env vars is something we're not doing for now, and if we find some easy way in the future all the better.

Especially since if the CLI creator *did* use the `env("..")` directive on fields that are likely to be pulled from ENV vars (applications that are highly in line with 12 factor guidelines, or secrets, etc.) and the existence of various env templates or use of things like `envsubst`, it's not too much of a workaround for those that need this.

---

_Comment by @kbknapp on 2020-04-29 15:26_

When going through the ripgrep specs, it got me thinking; what are the thoughts on instead of dedicated CLI syntax `@foo`, what if we just add support to the `App` instance? Such that the `App` instance would support a list of paths in which to look for the argfile (in order presumably). This way we essentially side step all the syntax issues (help messages, conflicting with existing CLIs, etc.). In addition it would allow the clap consumer a choice of:

* Informing user of specific locations to place argfile for "automatic" handling
* Using a "partial/pre parse" idea I've been chewing on lately (...more below) so that they can use their own argument to support such argfiles if so desired `--arg-file=foo` either in place of standard locations, or in addition/overriding standard locations defined by their `App` struct.

---

### Partial/Pre Parsing

A way to declare a subset of arguments to be parsed, then return from parsing early. With the expectation that parsing will be called again, and run to full completion.

This most often comes up when you want some argument to handle some aspect of the CLI itself (such as `--color` flags to control error messages during parsing, or in the above `--arg-file` which needs to determine a file to load prior to parsing the rest of the CLI).

From a *user* stand point, nothing changes. The run the CLI and things just work. From the a clap *consumer* stand point, it essentially allows you to pull out some values from the CLI (ignoring everything else), change something about the CLI and continue parsing.

```rust
// .. already have some App struct
let pre_matches = app.get_partial_matches("color"); // Only parse the `--color` option, ignore all else
app = if let Some("never") = pre_matches.value_of("color") {
    app.setting(AppSettings::ColorNever)
} else {
    app
};

let matches = app.get_matches();
```

Maybe it's biting off more than we want to chew for now, but this issue seems to come up time and time again where we want to parse some particular subset of arguments, and use those values to inform how the CLI itself behaves.

---

_Referenced in [clap-rs/clap#1880](../../clap-rs/clap/issues/1880.md) on 2020-04-29 15:38_

---

_Referenced in [epage/clapng#138](../../epage/clapng/issues/138.md) on 2021-12-06 19:11_

---

_Label `C: args` removed by @epage on 2021-12-08 20:28_

---

_Removed from milestone `3.1` by @epage on 2021-12-08 20:28_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:15_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:15_

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 20:53_

---

_Label `D: easy` removed by @epage on 2021-12-09 20:53_

---

_Label `E-medium` removed by @epage on 2021-12-09 20:53_

---

_Label `E-help-wanted` removed by @epage on 2021-12-09 20:53_

---

_Label `E-easy` removed by @epage on 2021-12-09 20:53_

---

_Label `S-waiting-on-mentor` added by @epage on 2021-12-09 20:53_

---

_Label `S-waiting-on-mentor` removed by @epage on 2021-12-09 20:53_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 20:53_

---

_Comment by @epage on 2021-12-09 21:09_

@argfiles are a common enough pattern that I feel we should support it as-is rather than creating our own conventions.

From an API perspective, I like that in Python you enable it by specifying a `fromfile_prefix_char`.  This makes it opt-in and gives users control over the prefix in case `@` doesn't work for them.

There is some open question of if we should do anything special with indices or error reporting since things can get quite large :).

Another part to figure out (probably based on javac, python, etc) is precedence: does `@` always win or if you do `--takes-value @file`, does `takes-value` get populated with `"@file"`.

Generally when I see these being used, its due to path length limits or for other processing purposes, I don't think comments are a priority.

Some interesting notes from [`argparse`](https://github.com/python/cpython/blob/main/Lib/argparse.py#L2113)
- `@` is the highest precedence
- Its recursive
- It literally maps each line to an arg index but allows you to override `convert_arg_line_to_args`
  - This avoids the issue of quoting, escaping, etc that javac probably has to deal with
  - Still allows you to override the behavior to get ripgrep or javac behavior

Notes from [javac](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/javac.html#commandlineargfile)
- Explicitly not recursive
- It allows separating with spaces or newlines, requiring quoting for arguments with spaces
- They still handle globs
- They do no special CWD handle for paths in the file

Overall, I prefer python's simplicity mixed with flexibility.

---

_Comment by @lzybkr on 2021-12-09 22:49_

Note many Microsoft tools have support for [response files](https://docs.microsoft.com/en-us/cpp/build/reference/at-specify-a-compiler-response-file?view=msvc-170) which are more like `javac` than `argparse`.

---

_Comment by @epage on 2021-12-10 00:54_

@lzybkr Thanks for that link! I've dealt with response files but forgot they were an official thing.

---

_Closed by @epage on 2021-12-10 16:04_

---

_Comment by @epage on 2021-12-10 16:06_

Looking at argparse's code gave me the idea to make argfile handling a pre-processor, like [wild](https://crates.io/crates/wild) is for cross-platform glob support.

Benefits
- There doesn't need to be a One True Way of supporting it
- Flexibility without clogging up clap's API
- Can be used with other argument parsers
- People pay for what they use, without bloating our list of features
- Faster iteration, independent of clap's breaking change cycle
- Faster to get in people's hands because we don't have to lock ourselves into a specific policy

If anyone has concerns about going this route, feel free to speak up!

---

_Comment by @epage on 2021-12-10 16:06_

Forgot the link: https://github.com/rust-cli/argfile

---

_Referenced in [sigp/lighthouse#2808](../../sigp/lighthouse/pulls/2808.md) on 2022-01-06 19:58_

---
