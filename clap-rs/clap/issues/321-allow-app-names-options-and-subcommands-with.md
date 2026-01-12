```yaml
number: 321
title: allow app names, options and subcommands with hyphens when using the macro
type: issue
state: closed
author: flying-sheep
labels:
  - C-enhancement
  - S-waiting-on-decision
assignees: []
created_at: 2015-10-27T12:41:06Z
updated_at: 2018-08-02T03:29:46Z
url: https://github.com/clap-rs/clap/issues/321
synced_at: 2026-01-12T16:14:09Z
```

# allow app names, options and subcommands with hyphens when using the macro

---

_@flying-sheep_

the following code creates a `-b`/`--group` option, not a `-g`/`--group-by` option.

``` rust
clap_app!(myapp =>
    ...
    (@arg group_by: -g --group-by +takes_value "foo")
)
```


---

_Renamed from "allow options and subcommands with hyphens" to "allow app names, options and subcommands with hyphens" by @flying-sheep on 2015-10-27 12:44_

---

_Label `T: RFC / question` added by @Vinatorul on 2015-10-27 13:01_

---

_Comment by @kbknapp on 2015-10-27 14:31_

@flying-sheep the macro is still a work in progress, so thanks for reporting this! @james-darkfox ideas?

As a hacky work-around until this bug is fixed, you should be able to use a combination of the macro and traditional builder pattern (and builder pattern is just as fast as macro performance wise, just more verbose):

``` rust
let app = clap_app!(myapp =>
    ...
    (@arg group_by: -g --group-by +takes_value "foo")
)
.arg(Arg::with_name("group_by")
    .short("g")
    .long("group-by")
    .takes_value(true)
    .help("foo"));
```


---

_Label `T: bug` added by @kbknapp on 2015-10-27 14:31_

---

_Label `P3: want to have` added by @kbknapp on 2015-10-27 14:31_

---

_Label `W: 1.x` added by @kbknapp on 2015-10-27 14:31_

---

_Label `T: RFC / question` removed by @kbknapp on 2015-10-27 14:31_

---

_Assigned to @WildCryptoFox by @kbknapp on 2015-10-27 14:31_

---

_Label `C: macros` added by @kbknapp on 2015-10-27 14:32_

---

_Comment by @flying-sheep on 2015-10-27 18:37_

ha, i think for a one-time construction like here, any performance is good enough :smile: 


---

_Comment by @kbknapp on 2015-10-28 13:06_

If parsing speed isn't a concern you can achieve a similarly less verbose using the "usage style." This isn't to say the usage strings are slow, they're still quite fast, just not quite as fast as the builder pattern (or macro which simply expands to the builder).

``` rust
.arg_from_usage("-g, --group-by [group_by] 'foo'")
```


---

_Added to milestone `1.5` by @kbknapp on 2015-10-28 13:59_

---

_Comment by @WildCryptoFox on 2015-10-28 14:36_

@flying-sheep This is a limitation of the macro **muncher** I'm using here. Please use `--group_by` or the subtle alternative of `long("group-by")` (inline replacement for `--group-by`).

@kbknapp Not a bug. Limitation in the macro. I might be able to extend the macro muncher to concat these kinds of tokens but there will need to be some distinction between `--group`, `-by`. Note that macros cannot count the characters in the identifiers it collects. This could be made strict such that `-s` must be before `--long` and any direct instances of `-` after `--long` is to be processed as the long. I may look into this with the 2.0 macro.


---

_Comment by @WildCryptoFox on 2015-10-28 14:38_

@kbknapp I've already mentioned this issue in other issues (not tracking them down right now). And the case for `-g`, `--group`, `-by` would actually expand to `.short("g").long("group").short("by")`. The double setting of the short/long should actually lead to a `panic` IMHO (likely via `debug_assert`).


---

_Label `T: enhancement` added by @WildCryptoFox on 2015-10-28 14:42_

---

_Label `W: maybe` added by @WildCryptoFox on 2015-10-28 14:42_

---

_Label `W: 2.x` added by @WildCryptoFox on 2015-10-28 14:42_

---

_Added to milestone `v2.0` by @WildCryptoFox on 2015-10-28 14:42_

---

_Removed from milestone `1.5` by @WildCryptoFox on 2015-10-28 14:42_

---

_Comment by @kbknapp on 2015-10-28 14:48_

@james-darkfox yep, I remember you saying that now, my bad! I'd totally forgotten about that. We can remove the bug label, and just leave this issue open for tracking purposes.


---

_Label `T: bug` removed by @kbknapp on 2015-10-28 14:48_

---

_Label `W: 1.x` removed by @kbknapp on 2015-10-28 14:48_

---

_Comment by @WildCryptoFox on 2015-10-28 14:49_

@kbknapp :-)


---

_Comment by @WildCryptoFox on 2015-10-28 14:52_

@kbknapp Sidenote: I could fix this for `1.0` and I might yet still do that. Feel free to jump on IRC if you'd like to discuss more about 1.0/2.0.


---

_Comment by @flying-sheep on 2015-10-29 12:12_

> but there will need to be some distinction between `--group`, `-by`

i.e. “no whitespace between them”

as far as i’m concerned this is a bug since it’s very surprising, undocumented, divergent from how the usage string parser works, and subtle:

if i wouldn’t have been lucky by already having a '-b' switch defined in another argument, it would have taken ages to find this.


---

_Comment by @WildCryptoFox on 2015-10-29 12:38_

@flying-sheep Again. Limitation of the way macros actually work. Whitespace is **not** a matchable token. And the solution here might be (as I said to @kbknapp, is) to make the munching work with stages such that `-s` must come before `--long` and any direct subsequent instances of `-` after `--long` to be interpreted as `--long-continued`.

Sorry for the lack of documentation. I wrote this macro to ease the consumption of the builder pattern. I did not get around to documenting it for `clap-1.0` and instead continued to create `clap-2.0` which is a complete re-write of `clap` focusing on getting the implementation done right without becoming huge or too complex to maintain (like `clap-1.0` has become).

I will take an attempt to for `2.0` to fix this issue. After doing so, I may also back-port it to the `1.0` code. Sorry for the inconvenience.

As for being divergent from the usage string. The `2.0` is closer to the actual usage string format. https://github.com/james-darkfox/clap-rs/blob/redesign/src/macros.rs#L61 **2.0 is far from being ready**


---

_Comment by @WildCryptoFox on 2015-10-29 12:45_

@flying-sheep Feel free to join `irc.mozilla.org:6697+ssl #clap-rs` if you'd like to discuss this in real-time.

**How the rust macro system sees `--group-by`**: `-` `-` `group` `-` `by`. Whitespace does not exist. The hyphens are processed purely as 'tokens' and never make their way down to the expression level where they would be processed as operators. The identifiers `group` and `by` are processed as identifiers (like variable names, but not assigned to any data). The length of the identifiers is unknown at macro level and the usage for these instances is that the identifiers are converted into `&'static str` strings so `stringify!($group_identifier)` becomes `"group"`.


---

_Comment by @flying-sheep on 2015-10-29 14:10_

ah, makes sense. hmm, that’s problematic. so the fact that “-” isn’t a valid identifier character means that they are tokens. and e.g. `-  g  -    -  group` is indistinguishable from `-g  --group`…


---

_Comment by @WildCryptoFox on 2015-10-29 14:27_

@flying-sheep Indeed. The TT muncher that my macro uses to deconstruct the usage-string influenced format looks for these kinds of patterns. Macros can be tricky at times but when you get to know and use them like I do - they aren't all that difficult to write. If you're interested in learning more about macros. I recommend the following: https://danielkeep.github.io/tlborm/ (Feel free to skip past the technical introduction if it's too difficult and return to it later if you'd like to see how the macros **actually work**).


---

_Removed from milestone `v2.0` by @kbknapp on 2016-01-23 15:52_

---

_Comment by @flying-sheep on 2016-04-12 10:17_

is there any progress in 2.0?

e.g. panicking if `short()` is called a second time?

`long("group-by")` is not the worst workaround, but i’d still enjoy the real deal :smile: 


---

_Comment by @flying-sheep on 2016-04-12 11:10_

And apparently one can also create an app like this:

``` rust
let matches = clap_app!(@app(App::new("freeform app-name ☺"))
    (author: "...")
    ...
).get_matches();
```


---

_Renamed from "allow app names, options and subcommands with hyphens" to "allow app names, options and subcommands with hyphens when using the macro" by @kbknapp on 2016-05-16 03:18_

---

_Comment by @Arnavion on 2016-11-04 08:08_

I just hit this too. Is it not acceptable to add case to the macro that handles `--$long:expr` in addition to `--$long:ident`? The latter would stringify `$long` and pass it to `$arg.long` as it does now, and the former would directly pass it to `$arg.long` (which means the expr must be a string). That way the OP's example could be written as `(@arg group_by: -g --"group-by" +takes_value "foo")`

Edit: Alas, Rust doesn't allow `tt*` after an `expr`...

Edit 2:

``` rust
    (@arg ($arg:expr) $modes:tt --($long:expr) $($tail:tt)*) => {
        clap_app!{ @arg ($arg.long($long)) $modes $($tail)* }
    };
```

works. It allows `(@arg group_by: -g --("group-by") +takes_value "foo")` to compile as expected. Acceptable?


---

_Unassigned @WildCryptoFox by @Arnavion on 2016-11-04 08:08_

---

_Referenced in [clap-rs/clap#731](../../clap-rs/clap/pulls/731.md) on 2016-11-04 09:21_

---

_Comment by @kbknapp on 2017-01-30 04:58_

This is implemented as `--("some-name")` by Arnavion

---

_Closed by @kbknapp on 2017-01-30 04:58_

---

_Comment by @g2p on 2018-06-11 20:43_

There's a fix implemented for long arguments, sure, but subcommands (and the app name) still cannot have dashes it seems. Could we have a workaround for those too? Should I open a new issue?

---

_Comment by @kbknapp on 2018-06-13 00:02_

@g2p yeah please open a new issue so it gets more visibility than this older one 👍 

---

_Referenced in [clap-rs/clap#1297](../../clap-rs/clap/issues/1297.md) on 2018-06-13 06:59_

---
