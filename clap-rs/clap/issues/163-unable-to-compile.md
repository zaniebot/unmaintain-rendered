```yaml
number: 163
title: Unable to compile
type: issue
state: closed
author: sirideain
labels:
  - C-bug
  - A-builder
assignees: []
created_at: 2015-07-18T05:38:56Z
updated_at: 2018-08-02T03:29:41Z
url: https://github.com/clap-rs/clap/issues/163
synced_at: 2026-01-12T16:14:08Z
```

# Unable to compile

---

_@sirideain_

I get these errors whenever I try to compile any version after 0.11.0. I tried with Rust 1.1.0, 1.2.0, and 1.3.0.

After some more trial and error, I was able to get it to compile if I removed the default-features line below.

``` toml
[dependencies.clap]
version = "*"
default-features = false
features = [ "suggestions" ] # Color can't be used with Windows as it throws odd characters in there
```

```
   Compiling strsim v0.4.0
   Compiling clap v1.1.2
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1714:49: 1714:70 error: the trait `core::fmt::Display` is not implemented
 for the type `T` [E0277]
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1714                                                 Format::Warning(&arg
),

                                                            ^~~~~~~~~~~~~~~~~~~~
~
note: in expansion of format_args!
<std macros>:2:26: 2:57 note: expansion site
<std macros>:1:1: 2:61 note: in expansion of format!
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1712:59: 1716:86 note: expansion site
note: in expansion of if let expansion
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1710:33: 1723:34 note: expansion site
note: in expansion of if let expansion
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1709:29: 1724:30 note: expansion site
note: in expansion of if let expansion
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1708:25: 1725:26 note: expansion site
note: in expansion of if let expansion
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1699:21: 1771:22 note: expansion site
note: in expansion of if let expansion
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1698:17: 1772:18 note: expansion site
note: in expansion of while let expansion
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1689:9: 1944:10 note: expansion site
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1714:49: 1714:70 help: run `rustc --explain E0277` to see a detailed expl
anation
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1714:49: 1714:70 note: `T` cannot be formatted with the default formatter
; try using `:?` instead if you are using a format string
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1714                                                 Format::Warning(&arg
),

                                                            ^~~~~~~~~~~~~~~~~~~~
~
note: in expansion of format_args!
<std macros>:2:26: 2:57 note: expansion site
<std macros>:1:1: 2:61 note: in expansion of format!
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1712:59: 1716:86 note: expansion site
note: in expansion of if let expansion
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1710:33: 1723:34 note: expansion site
note: in expansion of if let expansion
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1709:29: 1724:30 note: expansion site
note: in expansion of if let expansion
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1708:25: 1725:26 note: expansion site
note: in expansion of if let expansion
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1699:21: 1771:22 note: expansion site
note: in expansion of if let expansion
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1698:17: 1772:18 note: expansion site
note: in expansion of while let expansion
C:\Users\Charlie\.cargo\registry\src\github.com-0a35038f75765ae4\clap-1.1.2\src\
app.rs:1689:9: 1944:10 note: expansion site
error: aborting due to previous error
Could not compile `clap`.
```


---

_Comment by @kbknapp on 2015-07-18 12:38_

Thanks for taking the time to file this! I'll start looking into it! 

I'll post back here with anything I find. 


---

_Label `T: bug` added by @kbknapp on 2015-07-18 12:39_

---

_Label `P1: urgent` added by @kbknapp on 2015-07-18 12:39_

---

_Label `C: app` added by @kbknapp on 2015-07-18 12:39_

---

_Comment by @kbknapp on 2015-07-18 13:27_

Also, I forgot to ask, which version of Rust are you using? (`rustc -V` should spit it out).


---

_Comment by @sirideain on 2015-07-18 13:31_

I tried it with three versions of Rust.

rustc 1.1.0 (35ceea399 2015-06-19)
rustc 1.2.0-beta.2 (c8bab9d06 2015-07-09)
rustc 1.3.0-nightly (e4e93196e 2015-07-14)


---

_Comment by @kbknapp on 2015-07-18 13:32_

Ok, good to know, thanks! :) 

I may also just disable colorized output on windows, because if it just spits out crazy characters no reason to make windows devs have to manually exclude the dep.


---

_Comment by @sirideain on 2015-07-18 13:35_

Here is what the output looks like on Windows.

←[1;31merror:←[0m The argument '←[33m-a←[0m' isn't valid


---

_Comment by @kbknapp on 2015-07-18 13:36_

Yep, I was able to reproduce this error, as well as the wonky colorized output. This should be an easy fix, and I should have a working 1.1.3 up for you soon ;)


---

_Referenced in [clap-rs/clap#164](../../clap-rs/clap/pulls/164.md) on 2015-07-18 14:34_

---

_Comment by @kbknapp on 2015-07-18 14:37_

Once #164 passes Travis I'll merge into master and publish v1.1.3 on crates.io

Also note with that PR, you no longer need to specify

``` toml
default-features = false
features = ["suggestions"]
```

It will still compile `ansi_term` but it won't use it. Now if you want to keep your dep list short, or just don't want to compile `ansi_term` you can still turn off the `"color"` feature like before ;)


---

_Closed by @kbknapp on 2015-07-18 14:57_

---

_Comment by @kbknapp on 2015-07-18 15:05_

This issue has been fixed with v1.1.3 on crates.io or master here. @classicboss302 please let me know if  you have any further issues! And thanks again for taking to time to report this and assist! :+1: 


---

_Referenced in [clap-rs/clap#3122](../../clap-rs/clap/issues/3122.md) on 2021-12-09 16:16_

---
