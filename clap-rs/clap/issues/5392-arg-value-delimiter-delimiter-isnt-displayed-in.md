```yaml
number: 5392
title: "`#[arg(value_delimiter = ':')]` Delimiter isnt displayed in the help message."
type: issue
state: open
author: max-ishere
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2024-03-11T13:12:56Z
updated_at: 2025-02-03T17:23:02Z
url: https://github.com/clap-rs/clap/issues/5392
synced_at: 2026-01-12T16:14:17Z
```

# `#[arg(value_delimiter = ':')]` Delimiter isnt displayed in the help message.

---

_@max-ishere_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.2

### Describe your use case

Most of the arguments are already documented by clap, I didnt even have to do anything (nice). However the delimiter doesnt show up in the help message.

```rust
    /// Colon-separated list of lock types
    #[arg(required = true, value_delimiter = ':')]
    #[clap(long)]
    what: Vec<InhibitTarget>,
````
```txt
Options:
      --what <WHAT>  Colon-separated list of lock types [possible values: shutdown, sleep, ...]
```

### Describe the solution you'd like

The delimiter should be shown like this:

```txt
Options:
      --what <WHAT>  [`:` separated list of: shutdown, sleep, ...]
```

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @max-ishere on 2024-03-11 13:12_

---

_Label `A-help` added by @epage on 2024-03-11 14:28_

---

_Label `S-waiting-on-design` added by @epage on 2024-03-11 14:28_

---

_Comment by @epage on 2024-03-11 14:30_

For #4812, we are looking at including it within the example usage.  I'd be interested in exploring ways of doing that here as well.

---

_Renamed from "`#[arg(value-delimiter = ':')] Delimiter isnt displayed in the help message." to "`#[arg(value_delimiter = ':')] Delimiter isnt displayed in the help message." by @max-ishere on 2024-03-12 07:09_

---

_Renamed from "`#[arg(value_delimiter = ':')] Delimiter isnt displayed in the help message." to "`#[arg(value_delimiter = ':')]` Delimiter isnt displayed in the help message." by @max-ishere on 2024-03-12 07:09_

---

_Referenced in [clap-rs/clap#5817](../../clap-rs/clap/pulls/5817.md) on 2024-11-14 15:39_

---

_Comment by @Aethelflaed on 2024-11-14 16:11_

Placing it in the usage can be similar to what was done for #1052, but then that's only visible when there's more than one value name.

I'm proposing to also add it in the help message of the option/argument

---

_Comment by @epage on 2024-11-14 18:01_

> that's only visible when there's more than one value name.

Not quite as that is conflating a couple of features which I realized in seeing  #5817 

In that PR,
```rust
            arg!(-f --fake <s> "some help")
                .required(true)
                .required(true)
                .value_names(["some", "val"])
                .action(ArgAction::Set)
                .value_delimiter(':'),
        )
```
gets rendered as
```
Usage: test [OPTIONS] --fake <some>:<val> --birthday-song <song> --birthday-song-volume <volume>

Options:
  -f, --fake <some>:<val>  some help [value delimiter: ':']
````

Seeing that is reminding of the weird interplay of
- `value_names`
- `num_args`: gets defaulted by `value_names`
- `value_delimiter`

So `--fake` actually accepts `--fake some1:some2:some3 val1:val2:val3`.  This was likely not intended for that test but slipped through the cracks as features changed through v3.  The test was likely trying to get `--fake some:val` but now we only support unbounded value delimiters.

We have the idea of `extra_values`
https://github.com/clap-rs/clap/blob/2920fb082c987acb72ed1d1f47991c4d157e380d/clap_builder/src/builder/arg.rs#L4664-L4671

If we used something like that, we could get
```
  -f, --fake <some>:... <val>:...  some help [value delimiter: ':']
```
or
```
  -f, --fake <some>[:...] <val>[:...]  some help [value delimiter: ':']
```


---

_Comment by @epage on 2024-11-14 18:07_

> I'm proposing to also add it in the help message of the option/argument

Currently we do this for
- default values
- env
- possible values (#5156 could alleviate this in some cases)

Seeing two of those rendered together, we get
```
Usage: default [OPTIONS]

Options:
      --arg <argument>  Pass an argument to the program. [default: "\n"] [possible values: normal, "
                        ", "\n", "\t", other]
  -h, --help            Print help
  -V, --version         Print version
```
Overall, I'm not thrilled with these and don't feel like they compose well together, and worry about extending them more.  Also, *if* we can find ways to include them in the usage, it becomes more natural (like I want to do with possible values).

btw a good test case for one, the other, or both options is to see what Cargo's output would look like with changing this.

---

_Comment by @Aethelflaed on 2024-11-19 09:35_

> So --fake actually accepts --fake some1:some2:some3 val1:val2:val3. This was likely not intended for that test but slipped through the cracks as features changed through v3. The test was likely trying to get --fake some:val but now we only support unbounded value delimiters.

From what I gathered, there used to be an option to require the delimiter, but without it we're left with allowing both the delimiter and the space separated values. I didn't think that an issue.

Your idea with the `extra_values` sounds great, and we could even indicate the delimiter as optional with something like:

```
  -f, --fake <some>[ : | ' ' ]... <val>[ : | ' ' ]...  some help
```

Although it gets a bit hard to read...

```
  -f, --fake <some>[[ : | ' ' ]...]
```

About the `[help message]` part, if it doesn't impact the direct usage (like a default value), then it could only be display in the long`--help` version to remove?

---

_Comment by @Aethelflaed on 2024-11-19 09:40_

> btw a good test case for one, the other, or both options is to see what Cargo's output would look like with changing this.

Any particular cargo command I can try to see interesting result?

---

_Comment by @epage on 2024-11-19 20:42_

> From what I gathered, there used to be an option to require the delimiter, but without it we're left with allowing both the delimiter and the space separated values. I didn't think that an issue.

That setting was because of a weird interaction between features and became redundant.  Just setting a value delimiter makes it so you only use the value delimiter.  You have to explicitly opt-in to `num_args)1..)` or similar to have them both active.

> `  -f, --fake <some>[ : | ' ' ]... <val>[ : | ' ' ]...  some help`

If you have both `num_args(1..)` and `value_delimiter(':')`, I'm not sure how the `value_names` should be integrated in, whether they should be shown as part of the first `std::env::args_os` entry (kind of like you showed though yours is also interleaved) or one `value_name` per entry (like what I showed)

---

_Comment by @epage on 2024-11-19 20:44_

>  Any particular cargo command I can try to see interesting result?

Hmm, the only delimited value I'm remember is `--features` and we hand-implement that because we have two delimiters.  If we had it showing in the usage, I'd change it so one of the two was done through clap so it would be rendered.

---

_Comment by @zaneduffield on 2025-02-03 01:23_

Personally, I'm a fan of the way that PostgreSQL indicates that something is a comma-separated list.
They append `[, ... ]` to whatever can be repeated in a list. An example is the docs for [sql-createtable](https://www.postgresql.org/docs/17/sql-createtable.html).

This works nicely because all throughout the PostgreSQL docs `[]` represents an optional thing, and `...` is used as a kind of inferred placeholder.

---

_Comment by @epage on 2025-02-03 17:21_

> If you have both num_args(1..) and value_delimiter(':'), I'm not sure how the value_names should be integrated in, whether they should be shown as part of the first std::env::args_os entry (kind of like you showed though yours is also interleaved) or one value_name per entry (like what I showed)

Our debug asserts do take a stance on this:
https://github.com/clap-rs/clap/blob/349bd3c6bfe4d63a340b25165af29da6d9dee55c/clap_builder/src/builder/debug_asserts.rs#L750-L762

This associates value names with `num_args`.

---

_Comment by @epage on 2025-02-03 17:23_

> Personally, I'm a fan of the way that PostgreSQL indicates that something is a comma-separated list. They append `[, ... ]` to whatever can be repeated in a list. An example is the docs for [sql-createtable](https://www.postgresql.org/docs/17/sql-createtable.html).
> 
> This works nicely because all throughout the PostgreSQL docs `[]` represents an optional thing, and `...` is used as a kind of inferred placeholder.

Yeah, I was considering that in https://github.com/clap-rs/clap/issues/5392#issuecomment-2477078558

Might be worth someone trying it out and seeing how it affects the output.

---

_Referenced in [clap-rs/clap#5995](../../clap-rs/clap/issues/5995.md) on 2025-05-09 14:30_

---
