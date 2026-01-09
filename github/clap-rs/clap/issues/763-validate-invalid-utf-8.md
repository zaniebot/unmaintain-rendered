---
number: 763
title: Validate invalid UTF-8?
type: issue
state: closed
author: sector-f
labels:
  - E-medium
  - A-validators
assignees: []
created_at: 2016-12-03T07:15:00Z
updated_at: 2018-08-02T03:29:57Z
url: https://github.com/clap-rs/clap/issues/763
synced_at: 2026-01-07T13:12:19-06:00
---

# Validate invalid UTF-8?

---

_Issue opened by @sector-f on 2016-12-03 07:15_

Having custom validators is very useful, but (as far as I can tell) it's impossible to validate things that aren't UTF-8.

Specifically, the signature is
````rust
fn validator<F>(self, f: F) -> Self where F: Fn(String) -> Result<(), String> + 'static
````

The validator function must take a String rather than an OsString. Doesn't this mean it will panic if you pass an argument that isn't valid UTF-8?

---

_Comment by @kbknapp on 2016-12-03 16:42_

This is true, yes. The problem with having validators for invalid UTF-8 is OsStings are harder to work with if you only want valid UTF-8. So by allowing validators which could handle invalid UTF-8 you make it harder for the majority case.

Now having said that I'm not against adding a `Arg::validator_os` which does exactly what you're asking.

---

_Comment by @sector-f on 2016-12-04 00:26_

>So by allowing validators which could handle invalid UTF-8 you make it harder for the majority case.

Well, since `validator` and `validator_os` are two separate functions, couldn't the majority simply use `validator`?

---

_Comment by @kbknapp on 2016-12-04 00:52_

Yes absolutely. But `validator_os` doesn't exist yet. I meant that *prior to* making a `validator_os` those assumptions would be true. :wink:

---

_Label `C: validators` added by @kbknapp on 2016-12-04 00:52_

---

_Label `D: easy` added by @kbknapp on 2016-12-04 00:52_

---

_Label `M: mentored` added by @kbknapp on 2016-12-04 00:52_

---

_Label `P4: nice to have` added by @kbknapp on 2016-12-04 00:52_

---

_Label `T: new feature` added by @kbknapp on 2016-12-04 00:52_

---

_Label `W: 2.x` added by @kbknapp on 2016-12-04 00:52_

---

_Added to milestone `2.20.0` by @kbknapp on 2016-12-04 00:53_

---

_Comment by @glowing-chemist on 2016-12-08 12:22_

if i'm looking to start contributing do i understand it correctly that validator would check to see if its arguments conformed to a specification but wasn't necessarily valid UTF-8?

---

_Comment by @kbknapp on 2016-12-10 04:47_

@glowing-chemist Correct. You can pretty much copy/paste the `Arg::validator` code into an `Arg::validator_os` (in `src/args/arg.rs`) except use an `OsString` instead of `String`

You'll also need to make the appropriate additions to `src/args/arg_builder/valued.rs` which is the basis for all args which contain values.

Next you'll have to update `src/args/any_arg.rs` which is a trait for querying information about any kind of arg. By adding a `validator_os` you'll have to add the implementation in four places:

 * `src/args/arg_builder/{option, positional}.rs` which could actually contian vlaidators 
 * `src/args/arg_builder/flag.rs` which can't, so it simply returns `None`
 * `src/app/mod.rs` which is just like `flag.rs` and can't contain a validator, so it just returns `None` as well.

Finally, you'll have to update `src/app/parser.rs` which is where the actual parsing and validation is done. Look for where `validators` are checked and applied, and simply do the same for any `validator_os`s.

Let me know if you get stuck :wink:

---

_Comment by @glowing-chemist on 2016-12-25 18:15_

@kbknapp Thanks, sorry it took so long but I should be putting in a pull request now.

---

_Comment by @kbknapp on 2016-12-31 03:41_

Closed with #784 

---

_Closed by @kbknapp on 2016-12-31 03:41_

---
