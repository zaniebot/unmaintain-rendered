---
number: 1472
title: "`long_about` used instead of `about` for \"-h\""
type: issue
state: closed
author: romanows
labels:
  - C-bug
  - A-help
  - E-help-wanted
assignees: []
created_at: 2019-05-14T23:01:01Z
updated_at: 2020-10-22T18:00:02Z
url: https://github.com/clap-rs/clap/issues/1472
synced_at: 2026-01-10T01:26:55Z
---

# `long_about` used instead of `about` for "-h"

---

_Issue opened by @romanows on 2019-05-14 23:01_

### Rust Version
1.33.0 and playground 1.34.1

### Affected Version of clap
2.32.0 and 2.33.0

### Bug or Feature Request Summary
Show `about` text for `-h` and `long_about` text for `--help`.

### Expected Behavior Summary
The following should print out different text for `app.print_help()` and `app.print_long_help()` corresponding to my actual use case of calling my app with "-h" versus "--help".  I expect to see "foo" in the first case corresponding to "-h" and "bar" in the second case corresponding to "--help".

    extern crate clap; // 2.33.0;
    use clap::App;
    
    fn main() {
        let mut app = App::new("app")
            .about("foo")
            .long_about("bar");
    
        println!("");
        println!("== -h ==");
        app.print_help();  // Should show "foo" not "bar"?
        
        println!("");
        println!("");
        println!("== --help ==");
        app.print_long_help();  // Should show "foo" and "bar" or maybe just "bar"?
    }

### Actual Behavior Summary
The same text is printed when the app is called with "-h" and "--help" and it is the `long_about` text in both cases.  (Not including the slightly different formatting for the for the flags section.)  I see "bar" and "bar" in both "-h" and "--help" cases.

### Sample Code or Link to Sample Code
[Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=14b8a0566fff939fd0e42a46b9f6b3fa)

---

_Comment by @romanows on 2019-05-17 18:50_

If you could confirm what exactly is supposed to happen with both `about` and `long_about` are defined, I could try my hand at debugging and patching this.  The documentation isn't explicit and maybe I'm off-base here, but I would think that one of these two cases is what one would want to have happen:

- "-h" gets you the `.about` text and "--help" gets you just the `.long_about` text
- "-h" gets you the `.about` text and "--help" gets you the `.about` text, a newline, then the `.long_about` text

I'd be happy with the second case because my `long_about` text just gives additional info to what I plan to write in the `about` text.  However, I think that the first case is ultimately more flexible, because I could see some applications wishing to restate the content in their short `about` text if they know the user is explicitly requesting more verbose help.

---

_Referenced in [TeXitoi/structopt#173](../../TeXitoi/structopt/issues/173.md) on 2019-06-27 18:33_

---

_Comment by @TheLostLambda on 2019-12-25 16:10_

Hi, I've just run into the same issue via structopt. I actually think that the first option would be better (just printing the long_about) because, as I understand it, structopt includes the short about text within the long_about text. Producing code like this:
```
clap::App::new("<name>")
    .about("Hi there, I'm Robo!")
    .long_about("Hi there, I'm Robo!\n\n\
                 I like beeping, stumbling, eating your electricity,\
                 and making records of you singing in a shower.\
                 Pay up or I'll upload it to youtube!")
// args...
```
from Doc comments like this:
```
/// Hi there, I'm Robo!
///
/// I like beeping, stumbling, eating your electricity,
/// and making records of you singing in a shower.
/// Pay up, or I'll upload it to youtube!
```

---

_Comment by @CreepySkeleton on 2019-12-25 16:24_

> I actually think that the first option would be better (just printing the long_about)

The distinction between `-h` and `--help` is pretty clear - a short summary and a long description. If you want only the long message, use `long_about` only.



>  structopt includes the short about text within the long_about text.

This is how `structopt` handles doc comments, the most common case. If you want only `long_about` to be called, use a raw method.

---

_Comment by @TheLostLambda on 2019-12-25 17:42_

This functionality that I was seeking was actually already in the 2.33 version of Clap, but was just commented out.

It's in the `write_default_help` method in the `app/help.rs` file. It just checks if `self.use_long` is set, and prefers `long_about` if it is.

I'll probably push that to my fork and I think that's a good way to do things, calling `long_about` for `long_help` and `about` for `help`.

---

_Referenced in [clap-rs/clap#1612](../../clap-rs/clap/pulls/1612.md) on 2019-12-26 01:07_

---

_Label `C: help message` added by @CreepySkeleton on 2020-02-01 13:48_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-01 13:48_

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-02-01 13:48_

---

_Label `T: bug` added by @CreepySkeleton on 2020-02-01 13:48_

---

_Label `W: 2.x` added by @CreepySkeleton on 2020-02-01 13:48_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-01 13:48_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-02-01 13:48_

---

_Closed by @pksunkara on 2020-02-13 12:36_

---

_Comment by @zetok on 2020-10-22 13:42_

This is still not fixed in clap v2.33.3, and going by the tags assigned to this issue it should have been. Reopen?

---

_Comment by @pksunkara on 2020-10-22 16:59_

It's fixed in master which will be released as v3

---

_Comment by @zetok on 2020-10-22 17:07_

The 3.0 milestone has an awfully long list open issues attached, and no due date set. Combined with how long it has been open, it seems really unlikely to be released anytime soon. Based on the amount of open attached issues, I'd say that it would take months if not years until it gets released.

In meanwhile, the latest stable release of clap will continue to have this bug.

So the question here is: how long exactly are you planning to keep the buggy behaviour in stable release of clap?

---

_Comment by @pksunkara on 2020-10-22 18:00_

We have pre releases for v3 out. The v2 is effectively frozen unless it's a security patch.

---
