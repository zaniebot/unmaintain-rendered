---
number: 1050
title: "Using `number_of_values` and `default_value` together"
type: issue
state: closed
author: Frederick888
labels:
  - C-bug
  - A-parsing
  - E-medium
assignees: []
created_at: 2017-09-19T09:31:11Z
updated_at: 2018-08-02T03:30:11Z
url: https://github.com/clap-rs/clap/issues/1050
synced_at: 2026-01-10T01:26:42Z
---

# Using `number_of_values` and `default_value` together

---

_Issue opened by @Frederick888 on 2017-09-19 09:31_

Found this issue with `clap 2.26.2`

It seems that currently the default value is implemented by appending it to the existing matches. But it causes issues if a combination of  `number_of_values` and `default_value` is used.

For instance,

```rust
extern crate clap;

use clap::{App, Arg};

fn main() {
    let m = App::new("hello_world")
        .arg(
            Arg::with_name("exit-code")
                .long("exit-code")
                .required(true)
                .takes_value(true)
                .number_of_values(1)
                .default_value("0"),
        )
        .get_matches();
    println!(
        "{:?}",
        m.values_of("exit-code").unwrap().collect::<Vec<_>>()
    );
}
```
```
⇒  ./hello_world --exit-code
["0"]
⇒  ./hello_world --exit-code 1
error: The argument '--exit-code <exit-code>' requires 1 values, but 2 were provided

USAGE:
    hello_world --exit-code <exit-code>

For more information try --help
```

Changing `number_of_values(1)` to `number_of_values(2)` would make it work if the argument is correctly given but the error message is still off when it's not provided:

```
⇒  ./hello_world --exit-code  
error: The argument '--exit-code <exit-code> <exit-code>' requires 2 values, but 1 was provided

USAGE:
    hello_world --exit-code <exit-code> <exit-code>

For more information try --help
```

---

_Referenced in [kbknapp/cargo-outdated#63](../../kbknapp/cargo-outdated/issues/63.md) on 2017-09-19 09:32_

---

_Comment by @Frederick888 on 2017-09-20 09:27_

```diff
diff --git a/src/app/parser.rs b/src/app/parser.rs
index 6ec72034..35eb3f9b 100644
--- a/src/app/parser.rs
+++ b/src/app/parser.rs
@@ -1733,15 +1733,8 @@ impl<'a, 'b> Parser<'a, 'b>
                 if let Some(ref val) = $a.v.default_val {
                     if $m.get($a.b.name).map(|ma| ma.vals.len()).map(|len| len == 0).unwrap_or(false) {
                         $_self.add_val_to_arg($a, OsStr::new(val), $m)?;
 
-                        if $_self.cache.map_or(true, |name| name != $a.name()) {
-                            arg_post_processing!($_self, $a, $m);
-                            $_self.cache = Some($a.name());
-                        }
-                    } else {
-                        $_self.add_val_to_arg($a, OsStr::new(val), $m)?;
-
                         if $_self.cache.map_or(true, |name| name != $a.name()) {
                             arg_post_processing!($_self, $a, $m);
                             $_self.cache = Some($a.name());
                         }
```
...fixes it for me. But I'm not really clear about the purpose of the codes here.

---

_Comment by @kbknapp on 2017-10-04 03:14_

Yep, I just saw this when going through #1056 

I'm not sure why it's doing the same thing in both branches :confused: 

---

_Referenced in [clap-rs/clap#1056](../../clap-rs/clap/issues/1056.md) on 2017-10-04 03:15_

---

_Label `C: parsing` added by @kbknapp on 2017-10-04 03:15_

---

_Label `D: easy` added by @kbknapp on 2017-10-04 03:15_

---

_Label `M: mentored` added by @kbknapp on 2017-10-04 03:15_

---

_Label `P2: need to have` added by @kbknapp on 2017-10-04 03:15_

---

_Label `T: bug` added by @kbknapp on 2017-10-04 03:15_

---

_Label `W: 2.x` added by @kbknapp on 2017-10-04 03:15_

---

_Added to milestone `2.27.0` by @kbknapp on 2017-10-12 21:36_

---

_Referenced in [JP-Ellis/mathematica-notebook-filter#1](../../JP-Ellis/mathematica-notebook-filter/issues/1.md) on 2017-10-21 06:39_

---

_Closed by @kbknapp on 2017-10-25 01:49_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-11-13 01:59_

---
