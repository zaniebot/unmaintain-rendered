```yaml
number: 4716
title: "error messages should textwrap like `--help`"
type: issue
state: open
author: cbs228
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-help
assignees: []
created_at: 2023-02-18T22:11:18Z
updated_at: 2023-02-26T02:04:45Z
url: https://github.com/clap-rs/clap/issues/4716
synced_at: 2026-01-12T16:14:16Z
```

# error messages should textwrap like `--help`

---

_@cbs228_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.1.6

### Describe your use case

I am attempting to use [`clap::Command::error()`](https://docs.rs/clap/4.1.6/clap/builder/struct.Command.html#method.error) to harmonize my crate's startup error messages. In addition to normal argument-parsing errors, I also use clap to report "*can't open file XXXX*" and similar types of startup errors. This permits them to be printed with the red `error:` prefix provided by the `color` feature.

When built with the `wrap_help` feature, the output of `--help` is automatically wrapped to the terminal size (or to `max_term_width`). I would expect this text-wrapping to also be applied to errors, but it is not.

### Describe the solution you'd like

The following minimal example reproduces one of my crate's error messages:

```rs
extern crate clap;

use clap::Command;

const ERR : &str = "cowardly refusing to read audio samples from a terminal.\n\nPipe a source of raw uncompressed audio from sox, parec, rtl_fm, or similar into this program.";

fn main() {
    Command::new("test")
        .max_term_width(91)
        .error(clap::error::ErrorKind::Io, ERR)
        .exit();
}
```

[Playground link](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=fc88a36dd627ae3e79572946c1ed5ad2)

On my terminal, which is 91-characters wide, this renders as:

```txt
error: cowardly refusing to read audio samples from a terminal.

Pipe a source of raw uncompressed audio from sox, parec, rtl_fm, or similar into this progr
am.
```

The naive line-wrapping of my terminal emulator splits a word, making it hard to read.

The output could be improved by applying the logic already used to wrap `--help` text to error messages as well. Then the output would wrap neatly at word boundaries, like this:

```txt
error: cowardly refusing to read audio samples from a terminal.

Pipe a source of raw uncompressed audio from sox, parec, rtl_fm, or similar into this
program.
```

### Alternatives, if applicable

The wrapping behavior could be implemented in a custom `ErrorFormatter`. As far as I know, the logic for detecting terminal presence, terminal width, and for performing text wrapping appears to be internal to clap. Re-implementing this logic in the client crate would probably result in several more direct dependencies (like `atty`).

### Additional Context

Rust has excellent error-handling and error tracing. When it comes to actually *reporting* the error to the CLI user, however, crates and their authors are mostly left to their own devices. I welcome other alternatives for performing this operation in a systematic, abstracted way.

EDIT: made example more concrete based on feedback in discussion.


---

_Label `C-enhancement` added by @cbs228 on 2023-02-18 22:11_

---

_Comment by @epage on 2023-02-19 01:34_

Huh, doesn't look like this has come up before.  I suspect its because most people are relying on the terminal width and just using wrapping for clean indentation for argument descriptions or if they care about max_term_width, their errors just aren't hitting that enough to notice.

How much of your example is artificaly (e.g. the text is)?  Like, is that width artificial?  What I'm interested in is ascertaining the impact of this problem.  What would be helpful to know is what you are setting the term width to, why you are setting it to that, and what your errors look like with and without it.

---

_Label `S-waiting-on-decision` added by @epage on 2023-02-19 01:34_

---

_Label `A-help` added by @epage on 2023-02-19 01:34_

---

_Comment by @cbs228 on 2023-02-19 02:14_

> Like, is that width artificial?

Yes, the width in the example is artificial. It is merely to demonstrate on Playground that `--help` is wrapped but that errors are not. In reality, I would probably use a `max_term_width` of 100 or so.

The example output stderr is

```txt
error: Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam pellentesque vehicula purus at lobortis. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. In porta facilisis commodo. Etiam consectetur dui vitae varius finibus. Nunc a urna urna. Nullam a porta tellus. Nulla eu ipsum vitae felis ornare pretium. Sed ornare, sem ac tempus vestibulum, est turpis congue lectus, nec vulputate est ipsum vitae ante.
```

Terminals will of course dutifully wrap long lines, but the wrapping is dumb and can split words.

The above example might be "better" as

```txt
error: Lorem ipsum dolor sit amet, 
consectetur adipiscing elit. Nullam 
pellentesque vehicula purus at 
lobortis. Class aptent taciti sociosqu 
ad litora torquent per conubia nostra, 
per inceptos himenaeos. In porta 
facilisis commodo. Etiam consectetur 
dui vitae varius finibus. Nunc a urna 
urna. Nullam a porta tellus. Nulla eu 
ipsum vitae felis ornare pretium. Sed 
ornare, sem ac tempus vestibulum, est 
turpis congue lectus, nec vulputate est 
ipsum vitae ante.
```

Here "better" is error text which is wrapped to the terminal width or the `max_term_width`.

Most error messages won't be long enough to need wrapping on reasonably-sized terminals (i.e., wider than half a punch-card). I'm not sure that any of clap's built-in errors get that long. In my actual use-case, my error message gets a bit long-winded because it provides advice on how to correct the error condition.

---

_Comment by @epage on 2023-02-19 02:25_

> Lorem ipsum 

FYI I'm looking for more concrete use cases.  I brought up the artifical 40 but using lorem ipsum also keeps this conversation less concrete.

> my error message gets a bit long-winded because it provides advice on how to correct the error condition.

Is there a reason you are putting the advice in the same paragraph?  Seems like that would be best to split out.  You also could format the error according to some only-documented-in-code rules and use clap's advice formatting.

Also, this is why I am looking for concrete examples, so we can look at the problem holistically and not just one narrow angle of it.

---

_Comment by @cbs228 on 2023-02-19 03:10_

> Is there a reason you are putting the advice in the same paragraph? 

It is split out into its own paragraph, but terminals do not break long lines of text neatly. The actual error message I am printing is:

```rs
const ERR : &str = "error: cowardly refusing to read audio samples from a terminal.\n\nPipe a source of raw uncompressed audio from sox, parec, rtl_fm, or similar into this program.";
```

The second line is 94 characters wide. My terminal's current width is 91 characters, which leads to ugly output:

```txt
Pipe a source of raw uncompressed audio from sox, parec, rtl_fm, or similar into this progr
am.
```

> You also could format the error according to some only-documented-in-code rules

Can you point me in the right direction for these rules?


---

_Comment by @epage on 2023-02-23 18:25_

> It is split out into its own paragraph, but terminals do not break long lines of text neatly. The actual error message I am printing is:

Thanks for the example!

> Can you point me in the right direction for these rules?

Check out the source for [`RichFormatter`](https://github.com/clap-rs/clap/blob/master/src/error/format.rs#L54) as it reads the `Error` data structure and formats the text.

---

_Comment by @cbs228 on 2023-02-25 19:16_

I've seen the `RichFormatter`, but the existing functions for [wrapping text](https://github.com/clap-rs/clap/blob/v4.1.6/src/builder/styled_str.rs#L112) are `pub(crate)`. There doesn't appear to be any way to get at them or to express the "this should be wrapped" intent.

I could write a custom formatter which performs its own terminal queries and wrapping, without clap… but at that point it would probably be more efficient to not involve clap in my error-printing process at all.

---

_Comment by @epage on 2023-02-26 02:04_

`StyledStr::wrap` will remain private for now as we continue to work out what the `StyledStr` API will be.  I've started back on improving `StyledStr`, so hopefully in the next couple of months we;'ll have a better idea of where it is at.

---
