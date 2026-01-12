```yaml
number: 2528
title: \K does not appear to work with --multiline
type: issue
state: open
author: jtojnar
labels:
  - bug
assignees: []
created_at: 2023-06-06T19:21:35Z
updated_at: 2023-06-06T21:41:23Z
url: https://github.com/BurntSushi/ripgrep/issues/2528
synced_at: 2026-01-12T16:13:24Z
```

# \K does not appear to work with --multiline

---

_@jtojnar_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

NixOS package.

#### What operating system are you using ripgrep on?

NixOS unstable

#### Describe your bug.

When I use PCRE 2 regex with `\K` in multiline mode, it will print the whole line instead only the part after `\K`.

#### What are the steps to reproduce the behaviour?

Run

```
printf "foo\nbar" | rg --multiline --pcre2 'foo\nb\K(a)'
```

#### What is the actual behaviour?

```ShellSession
$ printf "foo\nbar" | rg --debug --multiline --pcre2 'foo\nb\K(a)'
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
bar
```

#### What is the expected behavior?

It should print just `ar`.


---

_Comment by @jtojnar on 2023-06-06 19:26_

If I add `--only-matching `, it will even preserve the part after the expression (`ar` instead of just `a`):

```ShellSession
$ printf "foo\nbar" | rg --multiline --pcre2 --only-matching 'foo\nb\K(a)'
bar
```

`--only-matching ` does work if I remove `\K` but then it will also include the extra context (`foo\nb`):

```ShellSession
$ printf "foo\nbar" | rg --multiline --pcre2 --only-matching 'foo\nb(a)'
foo
ba
```

In some cases it could be worked around with negative look-behind but not always – in my full code I am getting:

“lookbehind assertion is not fixed length”


---

_Comment by @jtojnar on 2023-06-06 19:28_

Looks like `\K` also breaks another workaround using `--replace`:

```ShellSession
$ printf "foo\nbar" | rg --multiline --pcre2 'foo\nb\K(a)' --replace '$1'
bar
```

Fortunately, here I can just drop `\K` and use `--replace` in addition `--only-matching`:

```ShellSession
$ printf "foo\nbar" | rg --multiline --pcre2 --only-matching 'foo\nb(a)' --replace '$1'
a
```

---

_Comment by @BurntSushi on 2023-06-06 21:14_

So at a high level, for this specific case, the `-r/--replace` flag is really what you ought to be using here IMO. And in your example, you don't even need PCRE2:

```
$ printf "foo\nbar" | rg -Uor '$1' 'foo\nb(a)'
a
```

Otherwise, `\K` really messes with things. Your very first example is correct and intended behavior:

```
$ printf "foo\nbar" | rg --multiline --pcre2 'foo\nb\K(a)'
bar
```

Unless you give ripgrep a flag to tell it to do otherwise, it always prints matching lines. `bar` is a matching line, so the whole line is printed. `\K` does and shouldn't have any impact on that.

But the output of `--only-matching` is wrong:

```
$ printf "foo\nbar" | rg --multiline --pcre2 --only-matching 'foo\nb\K(a)'
bar
```

That really should just print `a`.

The problem here is that `\K` breaks an invariant that ripgrep assumes to be true about regexes: that the regex engine can be re-run (roughly) starting at the beginning of a match and reproduce the matches found. But because of look-behind (which is basically what `\K` is), you actually need to make the contents (potentially all of them) before the match available for the regex engine to see, otherwise the regex may not match because of look-around. This has led to kludges like this in ripgrep:

https://github.com/BurntSushi/ripgrep/blob/4fcb1b2202b97c5a21894672232700225223a138/crates/printer/src/util.rs#L387-L442

At a more fundamental level, I fucked up the abstraction boundary between printing and searching, which makes this hard to fix properly. That abstraction boundary basically needs to go through a re-think.

That means I'll classify this as a bug, but don't hold your breath for it getting fixed any time soon.

The bug with `--replace` not working correctly with `\K` is "just" a downstream effect of this.

> “lookbehind assertion is not fixed length”

This is a PCRE2 limitation. Nothing I can do about that.

---

_Label `bug` added by @BurntSushi on 2023-06-06 21:14_

---

_Comment by @jtojnar on 2023-06-06 21:35_

> Otherwise, `\K` really messes with things. Your very first example is correct and intended behavior:
> 
> ```
> $ printf "foo\nbar" | rg --multiline --pcre2 'foo\nb\K(a)'
> bar
> ```

You are right. The issue here is actually that the match is not highlighted.

I started with `--only-matching` but then noticed that it does not also do any match highlighting without `--only-matching` and forgot to mention that and did not update the expectations accordingly. Normally, the matched part would be highlighted in bold red.

Thanks for the informative response. Will use `--only-matching --replace` without `\K` since it works okay.

---

_Comment by @BurntSushi on 2023-06-06 21:41_

Ah yeah, the lack of highlights are indeed caused by the same underlying problem plaguing `--only-matching` and `--replace`.

---
