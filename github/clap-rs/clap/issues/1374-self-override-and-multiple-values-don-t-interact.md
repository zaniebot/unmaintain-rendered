---
number: 1374
title: "Self override and multiple values don't interact well together"
type: issue
state: closed
author: Elarnon
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2018-11-08T00:39:21Z
updated_at: 2021-08-14T09:41:40Z
url: https://github.com/clap-rs/clap/issues/1374
synced_at: 2026-01-07T13:12:19-06:00
---

# Self override and multiple values don't interact well together

---

_Issue opened by @Elarnon on 2018-11-08 00:39_

### Affected Version of clap

master and v3-master (did not try crates.io versions)

### Bug or Feature Request Summary

Arguments overriding themselves with multiple values have... surprising behavior.  Consider the sample code below:

### Sample Code or Link to Sample Code

```rust
extern crate clap;
use clap::{App, Arg};

fn main() {
    let matches = App::new("MyApp")
        .arg(
            Arg::with_name("input")
                .takes_value(true)
                .long("input")
                .overrides_with("input")
                .min_values(0),
        ).get_matches();

    if let Some(vs) = matches.values_of("input") {
        println!("{:?}", vs.collect::<Vec<_>>());
    }
}
```

### Expected Behavior Summary

One would expect this output, where each new occurrence of the `--input` argument overrides the preceding one:

```shell
$ ./target/debug/example --input a b c --input d
["d"]
$ ./target/debug/example --input a b --input c d
["c", "d"]
```

### Actual Behavior Summary

However on current clap master 33f908df9b1eba26aadb8634104a135e58ea6912 there is no difference between those two invocations:

```shell
$ ./target/debug/example --input a b c --input d
["c", "d"]
$ ./target/debug/example --input a b --input c d
["c", "d"]
```

Interestingly, Clap v3 seems to have changed *something*, since on v3-master eaa0700e7ed76f37d245a6878deb3b8e81754d6c I get this:
```shell
$ ./target/debug/example --input a b c --input d
["d"]
$ ./target/debug/example --input a b --input c d
["d"]
```

which is unfortunately not much better.

### Consideration and thoughts

This seem to be caused by the self-override code being written with the use case of arguments with a single value in mind.  This also causes other issues, for instance if I change `min_values(0)` to `number_of_values(2)` in the above code I get:

```shell
$ ./target/debug/example --input a b --input c d
error: Found argument 'd' which wasn't expected, or isn't valid in this context
```
on master and:

```shell
$ ./target/debug/example --input a b --input c d
error: The argument '--input <input> <input>' requires 2 values, but 1 was provided
```

on v3-master.

The offending code is here:

https://github.com/clap-rs/clap/blob/33f908df9b1eba26aadb8634104a135e58ea6912/src/args/arg_matcher.rs#L66-L75

Interestingly, it somehow gets called twice (before parsing the overriding argument, and after parsing it), which is why we need `--input a b c --input d` to trigger the bug.  Doing only `--input a b --input c` would properly remove the `a` and `b` (if you are wondering: yes, `--input --input a b` only prints `["b"]`).

The corresponding code in v3 is here, and it now pops off and only keeps the final argument value (hence the behavior change).  It is also now called only once, after the overriding argument has been parsed: 

https://github.com/clap-rs/clap/blob/eaa0700e7ed76f37d245a6878deb3b8e81754d6c/src/parse/parser.rs#L1328-L1338

I am not familiar with the code at all, but from my understanding properly fixing this would require a way of knowing how much arguments were parsed in the current occurrence, in order to only keep that amount.  Unfortunately, there doesn't seem to be a way of getting that information currently.

One solution would be to add an `occurrences` vector to the `MatchedArg` which keeps track of the position in the `vals` vector when a new occurrence is encountered, and can be used to properly pop off all values but the ones associated with the last occurrence when an override occurs.  This can be extended by also keeping track of the position in the `indices` vector, since currently self-overrides "leak" through the `indices` (i.e. the indices of the overridden occurrences are kept), and can be used as a basis to solve #1026.  This may also be the opportunity to change the behavior of `min_values` and `max_values` in clap 3 (or add a setting) to be per-occurrence instead of sum-of-occurrences (at least for me, that would be more intuitive -- but this would probably require discussion beforehand).

I have a proof-of-concept implementation of the above which I can make into a proper PR if the proposed solution seems reasonable, or I can try and implement another suggestion :)

---

_Referenced in [clap-rs/clap#1376](../../clap-rs/clap/pulls/1376.md) on 2018-11-08 19:51_

---

_Comment by @Elarnon on 2018-11-08 19:51_

I ended up going ahead and creating #1376 

---

_Referenced in [clap-rs/clap#1391](../../clap-rs/clap/pulls/1391.md) on 2019-10-30 08:41_

---

_Label `not-sure` added by @CreepySkeleton on 2020-02-01 15:35_

---

_Comment by @CreepySkeleton on 2020-02-06 09:56_

Hey @Elarnon , kudos for writing this detailed step-by-step bug report! It's a real pleasure reading well-written explanations in simple fluent language that instantly lands in your brain without a need to parse "what that person meant". Bonus points for the attempt to fix it, I promise we'll get to it eventually.

---

_Label `cc: CreepySkeleton` removed by @CreepySkeleton on 2020-02-06 10:00_

---

_Label `C: args` added by @CreepySkeleton on 2020-02-06 10:00_

---

_Label `C: options` added by @CreepySkeleton on 2020-02-06 10:00_

---

_Label `C: parsing` added by @CreepySkeleton on 2020-02-06 10:00_

---

_Label `D: intermediate` added by @CreepySkeleton on 2020-02-06 10:00_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-06 10:00_

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-02-06 10:00_

---

_Label `T: bug` added by @CreepySkeleton on 2020-02-06 10:00_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-06 10:00_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-02-06 10:00_

---

_Label `C: asserts` added by @pksunkara on 2020-04-09 07:39_

---

_Label `help wanted` removed by @pksunkara on 2020-04-09 07:39_

---

_Label `C: asserts` removed by @pksunkara on 2020-04-09 07:39_

---

_Referenced in [clap-rs/clap#2297](../../clap-rs/clap/pulls/2297.md) on 2021-01-27 17:28_

---

_Referenced in [clap-rs/clap#2171](../../clap-rs/clap/issues/2171.md) on 2021-01-27 17:38_

---

_Closed by @pksunkara on 2021-02-07 15:11_

---

_Comment by @ldm0 on 2021-02-16 03:16_

Nope, this behaviour is not correct. As the [`override_with`'s documentation](https://docs.rs/clap/3.0.0-beta.2/clap/struct.Arg.html#method.overrides_with) states, the override bahviour is disabled when one of the Multiple* enabled. While min_values introduces MultipleValues, the correct behaviour is both of them returning `["a", "b", "c" "d"]`.

Why currently it works? This is because currently the min_value's MultipleValue is only happens on positional arguments, which is definitely a hack(and why flag with min_value still works, it's because after self overriding, it ca still pass the multiple argument usage checking).

---

_Reopened by @ldm0 on 2021-02-16 03:16_

---

_Referenced in [clap-rs/clap#2350](../../clap-rs/clap/pulls/2350.md) on 2021-02-16 04:11_

---

_Closed by @pksunkara on 2021-02-18 21:07_

---

_Comment by @pksunkara on 2021-06-16 04:08_

@ldm0 I am recently looking at this part of the code. How do I achieve the following scenarios (assuming `number_of_values(3)`):

```
// All occurrences should build to 3 values and they should be additive
$ prog -o 1 2 -o 3
[1, 2, 3]
$ prog -o 1 -o 3
error
```

```
// All occurrences should have 3 values and they should override
$ prog -o 1 2 3 -o 4 5 6
[4, 5, 6]
$ prog -o 1 2
error
$ prog -o 1 2 3 -o 4
error
```

---

_Reopened by @pksunkara on 2021-06-16 04:08_

---

_Comment by @ldm0 on 2021-08-08 08:21_

> @ldm0 I am recently looking at this part of the code. How do I achieve the following scenarios (assuming `number_of_values(3)`):
> 
> ```
> // All occurrences should build to 3 values and they should be additive
> $ prog -o 1 2 -o 3
> [1, 2, 3]
> $ prog -o 1 -o 3
> error
> ```
> 
> ```
> // All occurrences should have 3 values and they should override
> $ prog -o 1 2 3 -o 4 5 6
> [4, 5, 6]
> $ prog -o 1 2
> error
> $ prog -o 1 2 3 -o 4
> error
> ```

Sorry for the long delay, I haven't seen this @ until now. The first one can be easily achieved by:
```rust
fn main() {
    let m = clap::App::new("hello")
        .arg(
            clap::Arg::new("foo")
                .long("foo")
                .required(true)
                .multiple_values(true)
                .multiple_occurrences(true)
                .number_of_values(3),
        )
        .get_matches();
    dbg!(m);
}
```

But we cannot get the second behaviour now. Check: https://docs.rs/clap/3.0.0-beta.2/clap/struct.Arg.html#method.overrides_with . It's disabled when multiple* is on. But I think we can achieve this by adding a new setting now(or making a breaking change to make it work when multiple* is on).

---

_Assigned to @ldm0 by @ldm0 on 2021-08-08 09:29_

---

_Comment by @pksunkara on 2021-08-09 02:05_

I was thinking the same which is why I opened the issue. Definitely need to be resolved for 3.0

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Comment by @ldm0 on 2021-08-14 07:07_

> I was thinking the same which is why I opened the issue. Definitely need to be resolved for 3.0

@pksunkara I think making a breaking change is better. Previously, self override doesn't working when multiple* is on seems like a technical limitation. Currently we have grouped values, I think the behaviour of self override can be: Working when multiple values is enabled, stop working when multiple occurrences is enabled. This is more intuitive I think.

---

_Referenced in [clap-rs/clap#2696](../../clap-rs/clap/pulls/2696.md) on 2021-08-14 08:03_

---

_Closed by @pksunkara on 2021-08-14 09:41_

---
