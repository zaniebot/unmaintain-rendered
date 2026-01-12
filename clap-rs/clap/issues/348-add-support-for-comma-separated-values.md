```yaml
number: 348
title: Add support for comma separated values
type: issue
state: closed
author: kbknapp
labels:
  - A-parsing
  - E-medium
assignees: []
created_at: 2015-11-17T19:29:33Z
updated_at: 2018-08-02T03:29:46Z
url: https://github.com/clap-rs/clap/issues/348
synced_at: 2026-01-12T16:14:09Z
```

# Add support for comma separated values

---

_@kbknapp_

Example, these should all parse to exactly the same matches

```
$ prog --opt val1 val2 val3 
$ prog --opt val1,val2,val3
$ prog --opt=val1,val2,val3
$ prog -oval1,val2,val3
```


---

_Label `C: parsing` added by @kbknapp on 2015-11-17 19:29_

---

_Label `D: easy` added by @kbknapp on 2015-11-17 19:29_

---

_Label `P3: want to have` added by @kbknapp on 2015-11-17 19:29_

---

_Label `T: new feature` added by @kbknapp on 2015-11-17 19:29_

---

_Label `W: 1.x` added by @kbknapp on 2015-11-17 19:29_

---

_Label `M: mentored` added by @kbknapp on 2015-11-17 19:32_

---

_Comment by @psyomn on 2015-11-23 01:41_

I'm going to start looking at this. I'll ping you back if I have questions!


---

_Comment by @kbknapp on 2015-11-23 04:30_

:+1:


---

_Comment by @kbknapp on 2015-11-23 05:21_

You should be able to look at the `App:::add_value_to_arg` in `src/app/mod.rs` to start


---

_Comment by @psyomn on 2015-11-30 07:05_

Hey! Sorry for the late reply, it's been a little busy for me lately. I had the chance to work on this a little this weekend. I have a few questions!

Would introducing something like this be on the right path (notice `take_list`) ?

``` rust
let matches = App::new("MyApp")
        .arg(Arg::with_name("input")
                        .help("the input file to use")
                        .takes_value(true)
                        .short("i")
                        .long("input")
                        .multiple(true)
                        .takes_list(true)
                        .required(true)
                        .conflicts_with("output"))
        .get_matches();
```

So let's say this implementation works. When we're asking for `value_of` of `input` for this example, we're expecting a `&str` right? I was thinking of returning a vector, but that doesn't seem that it would be going in the right direction. So in fact, we'd expect to get a string instead of a list, and leave the user to deal with said input.


---

_Comment by @kbknapp on 2015-11-30 07:31_

No problem! :+1: 

Assuming the user runs `MyApp -i val1,vla2,val3` calling `value_of("input")` would return `val1` which isn't super useful, but that's how it works with other multiple values so this is fine. calling `values_of("intput")` you'll get a `Vec<&str>` which contains `["val1", "val2", "val3"]`. This is actually something that needs to be changed (and I'll put the issue in for it in just a minute), it should return a `&[&str]`.

For this issue, you shouldn't have to worry about anything in `arg_matches.rs`, it should only require changes `src/app/mod.rs:add_val_to_arg` and maybe `src/args/arg_matcher.rs` where you split the value by `,`.

But you bring up a very interesting idea, that I like. Changing your `takes_list(true)` to `allow_value_delims(bool)` (and defaults to `true`). This would assume most people would be OK with `-i val1,val2,val3` parsing to 3 values, but also give them the option to parse `-i val1,val2,val3` as a single value if their application requires such. (i.e. most users would never have to use `.allow_value_delims(true)` because that's the default, they only need to use `.allow_value_delims(false)` if they want to turn that feature OFF)

Another setting that would go along very nicely with this is making the delimiter selectable but defaulting to `,` (comma). This would require something like `value_delim(char)` and allow things like `-i val1;val2;val3` if `.value_delim(';')` was used.

For the above two features though I will create two issues, instead of trying to solve all three in this one issue.


---

_Referenced in [clap-rs/clap#352](../../clap-rs/clap/issues/352.md) on 2015-11-30 07:33_

---

_Referenced in [clap-rs/clap#353](../../clap-rs/clap/issues/353.md) on 2015-11-30 07:37_

---

_Added to milestone `1.6.0` by @kbknapp on 2015-12-18 08:58_

---

_Comment by @kbknapp on 2015-12-18 08:58_

@psyomn How's it going?


---

_Comment by @psyomn on 2015-12-18 09:50_

Hey, sorry for disappearing! I haven't had a chance to work on clap these
last weeks because I'm occupied with marking, trying to publish a paper
with a teacher, and on a job hunt as well! I will have time during holidays
if that's cool. If I'm holding people back, then I'll snatch another issue
when I'm not as pressed for time.

Thank you for the extensive writeup by the way. I wanted to express my
thanks, but didn't want to add too much noise to the issue tracker.

Hope everything is well on your end!

On Fri, Dec 18, 2015 at 3:58 AM, Kevin K. notifications@github.com wrote:

> @psyomn https://github.com/psyomn How's it going?
> 
> —
> Reply to this email directly or view it on GitHub
> https://github.com/kbknapp/clap-rs/issues/348#issuecomment-165717559.


---

_Comment by @kbknapp on 2015-12-18 10:30_

No worries :) Just wanted to check up. We may go ahead and implement this, but if its still open when your time frees up feel free to keep at it and put in a PR or ask if you have questions :wink:


---

_Added to milestone `v2.0` by @kbknapp on 2016-01-23 15:49_

---

_Removed from milestone `1.6.0` by @kbknapp on 2016-01-23 15:49_

---

_Comment by @kbknapp on 2016-01-26 12:08_

Closed with #387 


---

_Closed by @kbknapp on 2016-01-26 12:08_

---
