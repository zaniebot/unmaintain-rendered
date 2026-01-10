---
number: 970
title: Aliases the other way around (POSIX style)
type: issue
state: closed
author: ericbn
labels:
  - C-enhancement
assignees: []
created_at: 2017-05-27T00:35:12Z
updated_at: 2018-08-02T03:30:07Z
url: https://github.com/clap-rs/clap/issues/970
synced_at: 2026-01-10T01:26:40Z
---

# Aliases the other way around (POSIX style)

---

_Issue opened by @ericbn on 2017-05-27 00:35_

Clap 2.24.1 and rust 1.12 are currently being used.

Suppose I have defined these:

    .arg(flag("foo"))
    .arg(flag("no-foo").overrides_with("foo"))
    .arg(flag("bar").overrides_with("bar").takes_value(true).possible_values(&["0", "1"]))

And I want to have two other options that work as aliases for the previous ones:

* `--baz` = `--foo --bar=1`
* `--no-baz` = `--no-foo --bar=0`

and they would also override the individual options in POSIX style. So for example:

* `--baz --bar=0` yields only foo, and bar with value 0
* `--no-baz --foo` also yields only foo, and bar with value 0
* `--no-foo --baz` yields only foo, and bar with value 1
* `--bar=1 --no-baz` yields only no-foo, and bar with value 0

Any way to accomplish this with current clap implementation just by using the args builder?

(Note for maybe another issue: Not sure if `flag("bar").overrides_with("bar")` is valid. The idea is that only the last provided value should be used. E.g. `--bar=0 --bar=1` should yield bar with value 1. Curiously, it works when at least 2 of the same option are given. But it fails with `The argument '--bar' was provided more than once, but cannot be used multiple times` when 3 or more are given...) 

---

_Comment by @kbknapp on 2017-05-29 19:27_

This accomplishes 95% of what you're looking for:

```rust
        .arg(flag("foo"))
        .arg(flag("no-foo").overrides_with("foo"))
        .arg(flag("bar")
             .takes_value(true)
             .possible_values(&["0", "1"])
             .default_value("0")
             .default_value_if("baz", None, "1"))
        .arg(flag("baz"))
        .arg(flag("no-baz").overrides_with_all(&["baz", "foo"]))
```

The `"bar"` overrides with `"bar"` is the part that doesn't work at all with the above solution and I would say is a separate bug if you wouldn't mind filing it.

The the above solution meet what you're looking for, or are there additional details I'm missing?

---

_Label `T: RFC / question` added by @kbknapp on 2017-05-29 21:02_

---

_Comment by @ericbn on 2017-05-29 22:20_

The `flag("bar").default_value_if("baz", None, "1"))` will yield a bar with value 0 if `--bar=0 --baz` are provided, but I would expect the last to override the first, so to have a bar with value 1 (because baz is an "alias" for `--foo --bar=1`.

Also, if I just provide the option `--baz`, it will only yield the bar with value 1, but no foo (and I could not use default_value_if() with foo, because foo does not take values). I would expect both foo, and bar with value 1 to be present.

And if I just give the `--no-baz`, I would expect both the no-foo, and baz with value 0 to be present. But none would be yield.

Maybe other scenarios would fail too... Not sure if my description of the problem was clear enough.

The idea is that:
* baz works as an alias for `--foo --bar=1`
* no-baz works as an alias for `--no-foo --bar=0`
* last options override previous ones, even the "aliased" ones

 I sure didn't provide all possible scenarios in the examples I gave before. Let me try a complete list now:

| Options provided | foo | no-foo | bar |
|---|---|---|---|
| --foo | ✔️ | | |
| --no-foo | | ✔️ | |
| --bar=0 | | | 0 |
| --bar=1 | | | 1 |
| --baz | ✔️ | | 1 |
| --no-baz | | ✔️ | 0 |
| --no-foo --baz | ✔️ | | 1 |
| --bar=0 --baz | ✔️ | | 1 |
| --no-foo --bar=0 --baz | ✔️ | | 1 |
| --baz --no-foo | | ✔️ | 1 |
| --baz --bar=0 | ✔️ | | 0 |
| --baz --no-foo --bar=0 | | ✔️ | 0 |
| --foo --no-baz | | ✔️ | 0 |
| --bar=1 --no-baz | | ✔️ | 0 |
| --foo --bar=1 --no-baz | | ✔️ | 0 |
| --no-baz --foo | ✔️ | | 0 |
| --no-baz --bar=1 | | ✔️ | 1 |
| --no-baz --foo --bar=1 | ✔️ | | 1 |

(empty means option is not present)


---

_Comment by @ericbn on 2017-05-29 23:08_

As for the separate issue, I opened #976.

---

_Comment by @kbknapp on 2017-05-30 00:35_

I greatly appreciate the additional details! 👍 

The parts that jump out at me are going to be things like `--baz` implying `--foo`. Currently that's not possible and happens purely in user code. I.e. If I'm designing a CLI and I know `--baz` implies foo, I just add that check manually when I'm checking for `foo`

```rust
struct Args {
    foo: bool,
    bar: i32 // for simplicity
}

let matches = /* skpped */.get_matches();
let a = Args {
    foo: matches.is_present("foo") || matches.is_present("baz"),
    bar: matches.value_of("bar").unwrap() // all the encoding in clap made this unwrap safe
};
```

That isn't to say I *can't* make a feature to imply another arg, as requiring already works. Right now, if one requires a flag it arbitrarily `panic!`s simply because at the time I had more opinionated views on CLI design. I'm willing to relax that though if people find it useful.

The way requirements work, if a required arg is missing it throws an error. But it'd be super easy to add a check if the required arg is actually a flag, and just include it as if it was used.

---

_Comment by @ericbn on 2017-05-30 13:10_

In a usual [POSIX implementation](http://pubs.opengroup.org/onlinepubs/9699919799/functions/getopt.html) I would be able to do so with something like (replaced foo, no-foo, bar, baz, no-baz by a, b, c, d, e respectively):

```c
int c, errflg = 0;
bool foo = FALSE; // no-foo is the default
char* bar = NULL;
while ((c = getopt(argc, argv, ":abc:de")) != -1) {
  switch(c) {
  case 'a': // foo
    foo = TRUE;
    break;
  case 'b': // no-foo
    foo = FALSE;
    break;
  case 'c': // bar
    bar = optarg;
    break;
  case 'd': // baz
    foo = TRUE;
    bar = "1";
    break;
  case 'e': // no-baz
    foo = FALSE;
    bar = "0";
    break;
  case ':': // -c without operand
    fprintf(stderr, "Option -%c requires an operand\n", optopt);
    errflg++;
    break;
  case '?':
    fprintf(stderr, "Unrecognized option: '-%c'\n", optopt);
    errflg++;
  }
}
```

The loop iterating the options in order guarantees that later options will override previous ones, and I'll get the result from the table above, in the previous comment, for each case.

I could not figure out a way to achieve the same in clap. Declaring `foo: matches.is_present("foo") || matches.is_present("baz")`, for example, will not guarantee that a later baz replaces a foo that came before (or the other way around if I inverted the or expression operands), since this does take in account the order where the options appeared, just if they appeared at all...

---

_Referenced in [BurntSushi/ripgrep#499](../../BurntSushi/ripgrep/pulls/499.md) on 2017-06-01 23:38_

---

_Referenced in [BurntSushi/ripgrep#491](../../BurntSushi/ripgrep/issues/491.md) on 2017-06-02 00:00_

---

_Comment by @kbknapp on 2017-06-16 14:33_

> since this does take in account the order where the options appeared, just if they appeared at all...

This was the piece of information I was missing. In my mind, a flag being either present or not doesn't really matter *when* it appeared in the argv, just that it either did or didn't. What you're saying that it matters *where* it appears, as to whether or not it should be counted as present (i.e. if another flag overrides it, etc.)

This change is doable by me relaxing the requirements for flags. Let me play with some implementations and I'll get this knocked out.


---

_Label `C: flags` added by @kbknapp on 2017-06-16 14:34_

---

_Label `D: intermediate` added by @kbknapp on 2017-06-16 14:34_

---

_Label `P4: nice to have` added by @kbknapp on 2017-06-16 14:34_

---

_Label `T: enhancement` added by @kbknapp on 2017-06-16 14:34_

---

_Label `W: 2.x` added by @kbknapp on 2017-06-16 14:34_

---

_Label `T: RFC / question` removed by @kbknapp on 2017-06-16 14:34_

---

_Comment by @kbknapp on 2018-02-15 14:43_

This should be good now with 2.30.0. Feel free to re-open if there is something we're missing.

---

_Closed by @kbknapp on 2018-02-15 14:43_

---
