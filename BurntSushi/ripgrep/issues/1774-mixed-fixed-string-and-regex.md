```yaml
number: 1774
title: mixed fixed string and regex.
type: issue
state: closed
author: blueforesticarus
labels:
  - enhancement
  - wontfix
assignees: []
created_at: 2021-01-06T17:06:02Z
updated_at: 2023-11-24T20:20:18Z
url: https://github.com/BurntSushi/ripgrep/issues/1774
synced_at: 2026-01-12T16:13:24Z
```

# mixed fixed string and regex.

---

_@blueforesticarus_

This is the most common issue i've had with ripgrep and similar tools.

Lets consider the following problem:
1. I have a list of absolute paths.
2. I would like to translate the list such that all sub directories of my home directory show "~/..." instead of "/home/user/..."
3. I would like it to work for any valid path-name

The initial solution would be something like this:
`fd / | rg -F "$HOME" -r '~' `

However this will fail in the following way:
`home/user/my_chroot/home/user/... -> ~/my_chroot/~/...`

To solve this we need a position specifier.
`fd / | rg  "^$HOME" -r '~' `

However, now $HOME is being interpreted as a regex string, if it contains any special regex characters it will fail.
"[.*+" might be an unlikely name for a user or directory, but it is a valid path, and we want it to work.

We could escape $HOME as a regex string, however, I suggest a better solution is incorporate constants into the command line syntax, and possibly the regex engine itself.

As an example: lets say "-C<key> <value>" defines a constant, and in the regex string "@<key>" represents the constant.
Then we could use the following:
`fd / | rg -Ch  "$HOME" "^@h" -r '~' `

The syntax is just and example and probably would be different. The regex syntax for constants would only be turned on when you define a constant, so the addition wouldn't break anything currently working.
Under the hood ripgrep could translate the constant into an escaped sequence, or constants could be implemented in the regex engine itself.





---

_Comment by @BurntSushi on 2021-01-06 17:19_

I think the simplest thing might actually be to adopt a feature from other regex engines. Some regex engines let you do things like `\Q...\E` where `...` are interpreted literally, i.e., no escape sequences are respected. See https://stackoverflow.com/questions/30776860/regular-expression-q-e for example.

Then you should be able to do:

```
$ fd / | rg "^\Q$HOME\E" -r '~'
```

But this would be a feature of the [regex engine](https://github.com/rust-lang/regex), not ripgrep, so I'll open an issue there. Once I do that, I'll close this one.

I expect it will be some time before this is implemented though, so you'll have to work around it if you need this to work now. Here's one solution via a Rust program. Here's the `Cargo.toml`:

```toml
[package]
name = "regex-escape"
version = "0.1.0"
authors = ["Andrew Gallant <jamslam@gmail.com>"]
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
anyhow = "1"
regex-syntax = "0.6.21"
```

And `src/main.rs`:

```rust
use std::io::Write;

fn main() -> anyhow::Result<()> {
    let args: Vec<std::ffi::OsString> = std::env::args_os().collect();
    if args.len() != 2 {
        anyhow::bail!("Usage: regex-escape <pattern>");
    }
    let pattern = match args[1].to_str() {
        None => anyhow::bail!("pattern must be valid UTF-8"),
        Some(pattern) => pattern,
    };
    writeln!(std::io::stdout(), "{}", regex_syntax::escape(pattern))?;
    Ok(())
}
```

Example usage:

```
$ cargo build --release
   Compiling anyhow v1.0.37
   Compiling regex-syntax v0.6.21
   Compiling regex-escape v0.1.0 (/tmp/regex-escape)
    Finished release [optimized] target(s) in 4.49s
$ ./target/release/regex-escape '[.*+'
\[\.\*\+
```


---

_Label `enhancement` added by @BurntSushi on 2021-01-06 17:19_

---

_Comment by @blueforesticarus on 2021-03-04 05:10_

What if the variable string needed to include `\Q` or `\E` as a literal?

It seems like string constants are a simpler and more usefull feature. It's kinda bizarre that regex engines don't normally implement this.

---

_Comment by @BurntSushi on 2021-03-04 13:24_

> What if the variable string needed to include `\Q` or `\E` as a literal?

That's a good point. It makes the entire construct not 100% correct for all inputs, which does, IMO, kind of limit its value.

> It seems like string constants are a simpler and more usefull feature. It's kinda bizarre that regex engines don't normally implement this.

It's actually not bizarre at all. Almost all regex engines provide a way to escape any string such that it is fully interpreted as a literal. When you combine that with most programming language's string interpolation facilities, you essentially get what you're asking for nearly for free.

The problem here is that ripgrep isn't used inside the context of a programming language with its regex engine API exposed, so there's no easy way to escape arbitrary content beyond what I posted above. Normally this isn't a problem since `-F` is good enough for most cases.

---

_Comment by @blueforesticarus on 2021-03-04 14:39_

What is bizare is the complete lack of parameterization.

A regex `^John Smith (.*)$`, and `^Luca Brasi (.*)$`, are the **same** pattern, just a different fixed string parameter. You should be able to compile the pattern once (ie. `^{name} (.*)$`) and then provide it different parameters each time you search. 

---

_Comment by @BurntSushi on 2021-03-04 14:58_

@blueForestIcarus I suspect you're thinking that way because you aren't familiar with regex internals. It is _perhaps_ feasible in a very simplistic or toy regex engine, but production grade regex engines that prioritize performance do a substantial amount of analysis or construct things like finite state machines. You can't just interpolate a fixed string into stuff like that without doing potentially much more work.

I do not want us to chase our tails around regex internals. I would ask that you either trust me as someone who has spent years in the domain, or that you take some time to read the regex crate source code. I can assure you though that this missing functionality you're after is _not_ missing for bizarre reasons.

I'd like to note that this conversation is drifting between different types of features. For example, in ripgrep's case, we do not need to implement interpolation of string constants at regex search time. The interpolation only needs to be done at compile time because ripgrep doesn't provide the same expressive power as a programming language.

---

_Comment by @blueforesticarus on 2021-03-04 15:17_

Alright, well as far a ripgrep is concerned, without adding definitely missing and questionably  possible features to regex engine, I still think my original suggestion is better than the `'\Q...\E'` thing. 

It could be implemented by replacing  `@X` in the string with the value passed at the command line before the string is compiled as regex. It wouldn't interfere with existing patterns because it would only look for `@X` if h was given a value at the command line.

---

_Comment by @BurntSushi on 2021-03-04 15:37_

@blueForestIcarus Yes, I will have to give this some thought. I'm not yet convinced that the added complexity of this feature is really worth it, but I do understand that it solves a real problem. With that said, the work-around I posted above shouldn't be too much trouble. So I think the question is whether I'm okay forcing folks with this kind of problem into using said work-around.

---

_Comment by @learnbyexample on 2021-03-17 06:54_

`\Q...\E` is already supported by `-P` option.

```bash
$ h='/home/user/'
$ echo '/home/user/my_chroot/home/user/' | rg "$h" -r '~/'
~/my_chroot~/
$ echo '/home/user/my_chroot/home/user/' | rg -P '^\Q'"$h" -r '~/'
~/my_chroot/home/user/
```

If the variable to be protected has `\Q` or `\E`, you can use `perl` 

```bash
$ h='\Q\E123'

# no output
$ echo '\Q\E123asdb\Q\E123' | rg -P '^\Q'"$h" -r '~/'

# with perl
$ echo '\Q\E123asdb\Q\E123' | s="$h" perl -pe 's|^\Q$ENV{s}|~/|'
~/asdb\Q\E123
```

**Note** that with `rg` you'll need to use `--passthru` option if you want all input lines in the output irrespective of whether a match was found or not. And you may want to consider the case where filenames can have characters like newline.

---

_Comment by @BurntSushi on 2021-03-17 13:47_

@learnbyexample Yes, I was of course referring to the default regex engine. PCRE doesn't tend to do as well when you have a lot of regexes/words to search for:

```
$ awk 'length($0) >= 18 { print }' /usr/share/dict/words > words.18
$ wc -l words.18
233 words.18
$ time rg -c -F -f words.18 OpenSubtitles2018.raw.sample.small.en
4

real    0.076
user    0.072
sys     0.003
maxmem  34 MB
faults  0
$ time rg -c -P -F -f words.18 OpenSubtitles2018.raw.sample.small.en
4

real    1.955
user    1.951
sys     0.003
maxmem  33 MB
faults  0
```

And I actually had to use such a large word size to filter the data set, because otherwise PCRE complains that the regex is too large. I would increase the size limit, but given how slow it is on just a couple hundred words, I'm sure it wouldn't end well. :-)

The Perl example is interesting. It looks like its regex engine explicitly supports interpolation?

---

_Comment by @learnbyexample on 2021-03-17 15:29_

@BurntSushi sorry, I should have worded better to mean that `\Q..\E` can be used right now by OP for the given problem. Having this feature supported by Rust regex would be welcome too.

Perl will interpolate variables if the delimiter isn't single quote.

---

_Comment by @BurntSushi on 2021-03-17 16:16_

@learnbyexample Ah right. For some reason, I had thought the OP was trying to search for multiple patterns at once. Mea culpa.

---

_Comment by @BurntSushi on 2023-11-24 20:20_

I think I'm going to pass on this. It seems pretty complicated to support and it's not clear it's worth doing.

---

_Closed by @BurntSushi on 2023-11-24 20:20_

---

_Label `wontfix` added by @BurntSushi on 2023-11-24 20:20_

---
