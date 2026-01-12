```yaml
number: 235
title: Add ability to easily create interactive CLI
type: issue
state: closed
author: kbknapp
labels:
  - A-parsing
assignees: []
created_at: 2015-09-07T20:57:48Z
updated_at: 2022-02-09T16:26:41Z
url: https://github.com/clap-rs/clap/issues/235
synced_at: 2026-01-12T16:14:08Z
```

# Add ability to easily create interactive CLI

---

_@kbknapp_

As discussed in #228 this issue is to track progress and discuss implementation.

Off the top of my head, this seems decently simple. We'd just need a non-consuming `.get_matches_from_safe()`.

The idea being something like:

``` rust
fn main() {
    // build App like normal...

    // Start the interactive CLI
    loop {
        print!(">>> ");
        io::stdout().flush().unwrap();
        let mut s = String::new();
        io::stdin()
            .read_line(&mut s)
            .ok()
            .expect("failed to read line");
        let mut argv: Vec<_> = s.trim().split(" ").collect();
        // you have to insert the binary name since clap expects it
        argv.insert(0, "prog");
        match app.get_matches_from_safe_borrow(argv) {
            Ok(matches) => execute(matches),
            Err(e) => {
                // handle error, display message, etc.
                continue
            }
        }
    }

}

fn execute(m: ArgMatches) {
    // do stuff
}
```

**EDIT:** Just tested this and works perfectly.


---

_Label `T: new feature` added by @kbknapp on 2015-09-07 20:58_

---

_Label `P4: nice to have` added by @kbknapp on 2015-09-07 20:58_

---

_Label `D: intermediate` added by @kbknapp on 2015-09-07 20:58_

---

_Label `C: parsing` added by @kbknapp on 2015-09-07 20:58_

---

_Comment by @WildCryptoFox on 2015-09-08 00:30_

Depending on the application, the current mock won't work with a single thread. Anything that has networking or other events, will need another thread. Might need to make sure that whatever the implementation ends up being, is suitable to fit with `mio`. I.e. Wake up on input. Also needs to be able to clear the prompt, insert messages and re-write the progress on the prompt. (Otherwise, messages will break input as the user types)


---

_Comment by @kbknapp on 2015-09-08 01:16_

Ah yeah, I'm purely thinking single threaded. I'm pushing some changes which make the current mock up work in a single threaded environment, but beyond that it'll take a bit more massaging.


---

_Comment by @kbknapp on 2015-09-08 01:19_

#236 is what I was talking about


---

_Comment by @kbknapp on 2015-09-08 01:20_

So maybe this is a base...or at least some building blocks to allow further work on this topic. Using with `mio` is an interesting idea or even across a network stream. You've piqued my interest :wink: 


---

_Comment by @WildCryptoFox on 2015-09-08 01:45_

I'm pretty sure it would just need to hook `mio` through using `stdin` as an `Evented` input; throwing all input into a buffer. When required forcefully delete the line, do so and print the respective information.

This logging could even hook `env_logger` to capture these messages to write with this policy too. My `scoped_log` crate already hooks `log` for tracking hierarchy in `mio`'s thread.

My expectation will be just some `struct Terminal` which the `mio` based application would include in it's server `struct` and just interact with it accordingly.

**Note**: clap would at most need to implement `mio::Handler`, and have the consuming application map some token to our `struct Terminal`, who owns the stdin file descriptor.


---

_Referenced in [clap-rs/clap#259](../../clap-rs/clap/issues/259.md) on 2015-09-19 10:37_

---

_Comment by @wdv4758h on 2016-07-29 15:14_

Using `mio` looks interesting. How does it go on now ?


---

_Comment by @kbknapp on 2017-04-05 18:34_

Closed due to inactivity. I don't personally see myself working on this anytime soon with so much else to work on. Perhaps this will come back up someday.

---

_Closed by @kbknapp on 2017-04-05 18:34_

---

_Referenced in [clap-rs/clap#1471](../../clap-rs/clap/issues/1471.md) on 2020-01-10 18:24_

---

_Comment by @benaryorg on 2022-02-09 16:26_

Only just now found this issue by chance and wanted to point over at [benaryorg/dsa-cli](https://github.com/benaryorg/dsa-cli) which does implement something of sorts, albeit with clap 2.x and I'm not sure anything about that project is either production grade *or* current, but if anyone stumbles upon this, well, there's *something*.

---
