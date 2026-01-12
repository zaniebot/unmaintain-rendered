```yaml
number: 1150
title: Single-line mode anchoring $ to end of stream
type: issue
state: closed
author: mqudsi
labels: []
assignees: []
created_at: 2018-12-30T06:35:22Z
updated_at: 2018-12-31T01:06:53Z
url: https://github.com/BurntSushi/ripgrep/issues/1150
synced_at: 2026-01-12T16:13:23Z
```

# Single-line mode anchoring $ to end of stream

---

_@mqudsi_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

`cargo`

#### What operating system are you using ripgrep on?

FreeBSD 12

#### Describe your question, feature request, or bug.

Thanks for adding multiline support to `rg`. I could very well be missing something, but I can't seem to find how to trigger proper single-line support. Even with `(?s)`, `$` always matches the end of a line, instead of the end of the document.

Context: I would like to find trailing blank lines in a document.

---

_Comment by @BurntSushi on 2018-12-30 12:25_

Please provide an example. Please include a command, input, actual output and expected output.

---

_Comment by @mqudsi on 2018-12-30 14:51_

foo.txt:
```
hello1
hello2
```

command executed:
```
rg -U --multiline-dotall 'hello\d$'
```

also

```
rg -U --multiline-dotall '(?s)hello\d$'
```

output:
```
foo.txt
1:hello1
2:hello2
```

expected/desired output:
```
foo.txt
2:hello2
```

Some platforms use `(?s)` to also mean that `^` and `$` match the start and end of the input, respectively (rather than the start and end of each line). It seems `rg` does not. It would be desirable to have the ability to trigger such behavior for matching against document patterns rather than line patterns.

---

_Comment by @BurntSushi on 2018-12-30 15:09_

Thanks for the example. In the future, please include examples like that in your initial issue so that we can avoid the back and forth. Examples help to remove guesswork on my part and overall make it easier for me to respond.

> Some platforms use `(?s)` to also mean that `^` and `$` match the start and end of the input, respectively 

Can you show an example of said platform? The `s` flag means that `.` matches any character, including `\n`. It has nothing to do with the meaning of `^` or `$`. I'm not aware of any regex engine that supports the `s` flag with a different meaning off the top of my head.

To clarify, `^` and `$` typically only match the start or end of input. Many regex engines support a *multi line* flag, `m`, which modifies their behavior to _additionally_ match at the beginning or end of lines, respectively. This multi line flag is _orthogonal_ to ripgrep's feature known as "multi line search."

ripgrep always enables the `m` flag by default because it is a line oriented searcher, and therefore, it makes sense for `^`/`$` to operate in line oriented mode. This is true even when `-U/--multiline` is given, for consistency. However, you can disable the multi-line flag like anything else, e.g.,

```
$ rg -U '(?-m)hello\d$'
$
```

However, while this does not produced your desired output, it is actually the expected and correct output for this particular case. In particular, here are the binary contents of the file I created based on your input:

```
$ xxd foo.txt
00000000: 6865 6c6c 6f31 0a68 656c 6c6f 320a       hello1.hello2.
```

In this case, my editor added a trailing new line to the end of the file. If I remove that trailing new line, then I get the desired output:

```
$ rg -U '(?-m)hello\d$'
foo.txt
2:hello2
```

Alternatively, modify your regex to handle the optional trailing new line:

```
$ rg -U '(?-m)hello\d\n?$'
foo.txt
2:hello2
```

---

_Comment by @mqudsi on 2018-12-30 15:53_

Thanks for the response. Fwiw, the trailing new line was not supposed to be there, it must have been an artifact of the copy-and-paste, perhaps on my end.

From what I recall, at least Tcl uses `(?s)` to imply no multiline. I didn't realize that `rg`'s own default multiline behavior was implemented as a regex mode, I was under the impression it simply handled each line as a separate input before passing it along to the engine. The `(?-m)` does indeed do the job quite handily.

However I do wonder if it is worth either making it easier to access or at least documenting it in the `--multiline`/`-U` section of the help, or am I just seeing things through a very narrow perspective at the moment?

---

_Comment by @BurntSushi on 2018-12-30 16:16_

This page suggests that Tcl uses the same meaning for `s` and `m` as everything else: https://www.regular-expressions.info/tcl.html

> I didn't realize that rg's own default multiline behavior was implemented as a regex mode, I was under the impression it simply handled each line as a separate input before passing it along to the engine.

I'm not sure how to untangle this, so I'm just going to throw a bunch of facts at you. :-)

* The regular expression engine supports a number of flags, e.g., `i` for case insensitive matching, `s` for causing `.` to match `\n` in addition to every other character, `m` to cause `^`/`$` to match at the start/end of lines, and more.
* The special switches `\A` and `\z` match the beginning and end of the input and their behavior is never modified by any flags.
* ripgrep sets [many of these flags](https://github.com/BurntSushi/ripgrep/blob/17ef4c40f32af337c1b10c1a9fdd1df4241391b3/src/args.rs#L583-L588) for you. Some are "sane" defaults (e.g., enabling the `m` flag) while others are inferred from command line flags.
* In all cases, any regular expression flag can be overridden inside the pattern itself. e.g., `(?s).(?-s).` will match any character (including a new line) followed by any character (excluding a new line). Another way of writing the same regex: `(?s:.).`. If `--multiline-dotall` is enabled, then your pattern needs to be inverted to have the same behavior, e.g., `.(?-s:.)`.
* ripgrep searches **as if** reading one line at a time by default. While ripgrep can fall back to "read each line, and then match the given regex on each line," this is actually quite slow. ripgrep has other means of searching in line oriented mode that is faster, but requires some assumptions. [Please consider reading this section in my blog post on ripgrep.](https://blog.burntsushi.net/ripgrep/#anatomy-of-a-grep)
* ripgrep supports a multi-line search mode. This has _nothing_ to do with the regex engine's "multi line" flag. Multi-line search changes how ripgrep searches for matches. Instead of searching as if reading one line at a time, ripgrep will search as reading one entire file at a time. The point of this is to enable matches that span multiple lines. In this case, ripgrep never reads one line at a time because it's incompatible with searching the entire file.

If you can think of better generic docs for the `-U/--multiline` flag, then that would be great. But I don't think it's worth specifically calling out the `m` flag. It is working exactly as it's intended to work. I don't think there are any gotchas here, other than [reading the docs for regex flags](https://docs.rs/regex/1.1.0/regex/#grouping-and-flags).

---

_Comment by @mqudsi on 2018-12-30 23:45_

Thanks for the explanation. I was confused by the two different "multi line modes" and had assumed that `rg` *previously* used to split the input into lines without using the regex multiline mode option and now added support for reversing that. It's why it didn't occur to me to modify `(?m)` or `(?-m)` because I assumed switching between `rg` and `rg -U` did just that (enabled the *regex* multiline mode).

> This page suggests that Tcl uses the same meaning for s and m as everything else: https://www.regular-expressions.info/tcl.html

For what it's worth, my reading of the same page you linked to suggests differently. In particular:

> `(?s)` selects "non-newline-sensitive matching", which is the default.  The "s" stands for "single line".  In this mode, the dot and negated character classes match all characters, including newlines.  **The caret and dollar match only at the very start and end of the subject string.**

Also from the same site but a different page: https://www.regular-expressions.info/modifiers.html

> `(?s)` for "single line mode" makes the dot match all characters, including line breaks. Not supported by Ruby or JavaScript. In Tcl, `(?s)` also makes the caret and dollar match at the start and end of the string only.

Not that any of this matters, it's an intellectual sidebar at best.

---

_Comment by @BurntSushi on 2018-12-31 01:06_

OK, yes, the [official Tcl docs](https://www.tcl.tk/man/tcl8.4/TclCmd/re_syntax.htm#M59) add a bit more clarity, and in particular, in the "matching" section near the bottom. It seems that `s` is equivalent `sm` in ripgrep, where as `p` is equivalent to `s` and `m` is equivalent to `m-s`. Interesting history lesson. To validate my own sanity, I confirmed that both PCRE and [Perl](https://perldoc.perl.org/perlre.html#Modifiers) behave as ripgrep does. I _think_ Spencer's regex engine inside of Tcl pre-dated Perl's, so I wonder who was responsible for opting for the conceptually simpler model of making `m` and `s` orthogonal flags.

I am going to close this since I think everything is working as it should.

---

_Closed by @BurntSushi on 2018-12-31 01:06_

---
