```yaml
number: 134
title: Program name (bin_name) should be configurable
type: issue
state: closed
author: DanielKeep
labels:
  - C-enhancement
assignees: []
created_at: 2015-05-30T15:01:51Z
updated_at: 2015-05-30T18:03:13Z
url: https://github.com/clap-rs/clap/issues/134
synced_at: 2026-01-12T16:14:08Z
```

# Program name (bin_name) should be configurable

---

_@DanielKeep_

The first line of the `--help` message for my program is as follows:

``` text
cargo-script.exe-script 0.1.1
```

The obvious issue is that this is hideously ugly, and also misleading.  I'm writing a Cargo subcommand and, as such, the user is _never_ going to invoke this program as "cargo-script".  I need to tell clap that the program should identify itself as "cargo".  _Ideally_, I'd be able to tell it to lie when using the "script" subcommand, but not when it isn't provided.

I thought about just altering the arguments clap is parsing, but there doesn't appear to be any way to do this.

Also, it would help to not have the file extension present in the name.


---

_Comment by @kbknapp on 2015-05-30 16:36_

This shouldn't be too hard, let me play with an implementation idea and I'll post back here. Thanks for taking the time to file this!


---

_Label `enhancement` added by @kbknapp on 2015-05-30 16:36_

---

_Assigned to @kbknapp by @kbknapp on 2015-05-30 16:36_

---

_Comment by @DanielKeep on 2015-05-30 16:52_

No problem.  If it's of any use, the code in question is in [cargo-script/v0.1.1](https://github.com/DanielKeep/cargo-script/tree/v0.1.1).


---

_Comment by @kbknapp on 2015-05-30 16:56_

Thanks! I was just about to ask. So ideally, you'd like to be able to say the `bin_name` of the parent command is `cargo` and you've got a subcommand of `script` which in the help/version should be dispalyed as:

``` sh
$ cargo script -v
cargo-script v1.0
```

But in usage strings should be displayed as:

``` sh
USAGE:
    cargo script <script>
```

Correct?

If that's the case I should have this working in about 10 minutes ;)


---

_Comment by @DanielKeep on 2015-05-30 17:01_

Pretty much, yeah.  So long as it doesn't call itself `cargo-script-script` or `cargo.exe script`, it's all good.  :)


---

_Comment by @kbknapp on 2015-05-30 17:03_

Aright, I've got it working tested with this scenario. Uploading now, use 0.10.2 on crates.io
The relevant piece for you is on the `App` struct set the `.bin_name("cargo")` which allows you to override the system determined binary name with whatever you want. You could also use `cargo.exe` if you prefer, it could be anything.


---

_Closed by @kbknapp on 2015-05-30 17:15_

---

_Comment by @kbknapp on 2015-05-30 17:21_

It's up on crates.io now, you can also use the master branch here.

off topic, `cargo script` looks awesome! I'm going to have to keep an eye on this!


---

_Comment by @DanielKeep on 2015-05-30 18:03_

Thanks; updated.


---
