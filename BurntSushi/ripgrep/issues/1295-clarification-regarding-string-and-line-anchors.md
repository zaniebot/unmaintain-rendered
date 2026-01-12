```yaml
number: 1295
title: Clarification regarding string and line anchors
type: issue
state: open
author: learnbyexample
labels: []
assignees: []
created_at: 2019-06-07T06:06:35Z
updated_at: 2021-06-01T12:13:51Z
url: https://github.com/BurntSushi/ripgrep/issues/1295
synced_at: 2026-01-12T16:13:23Z
```

# Clarification regarding string and line anchors

---

_@learnbyexample_

#### What version of ripgrep are you using?

```bash
$ rg --version
ripgrep 11.0.1 (rev 1f1cd9b467)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)
```

#### How did you install ripgrep?

Using https://github.com/BurntSushi/ripgrep/releases/download/11.0.1/ripgrep_11.0.1_amd64.deb

#### What operating system are you using ripgrep on?

Ubuntu 16.04.1

#### Describe your question, feature request, or bug.

From https://docs.rs/regex/1.1.6/regex/#syntax

>Note that ^ matches after new lines, even at the end of input:

and I'm guessing this applies to `$` anchor as well

```bash
$ printf 'one\ntwo\nthree\n' | rg '^' -r '* '
* one
* 
* two
* 
* three
* 

$ printf 'one\ntwo\nthree\n' | rg '$' -r ' ;'
one ;
 ;
two ;
 ;
three ;
 ;
```

The above results are consistent in behavior if each input line is processed with newline intact. As a user, this is a bit unexpected as `printf 'one\ntwo\nthree\n' | wc -l` or `printf 'one\ntwo\nthree\n' | rg -c '^'` or `rg -c '$'` would give `3` as result. But the above use of line anchors with `-r` option results in 6 output lines. The use case I'm looking here is using `rg` as alternative to `sed` (search and replace only, not other features of sed)

I thought using string anchors would help here, but I'm not sure how `\z` works with ripgrep:

```bash
$ printf 'one\ntwo\nthree\n' | rg '\A' -r '* '
* one
* two
* three

$ # no output
$ printf 'one\ntwo\nthree\n' | rg '\z' -r ' ;'
$ # this is the workaround I was able to find
$ printf 'one\ntwo\nthree\n' | rg -U '\n' -r ' ;'
one ;
two ;
three ;
```

Some more examples I tried with `\z` to understand how to use it, could you explain what is happening here?

![rg_string_anchor](https://user-images.githubusercontent.com/17766317/59083619-b70e5880-8915-11e9-92bd-d14c8841a291.png)

Some examples with `-o` option:

```bash
$ printf 'one\ntwo\nthree\n' | rg -o 'e\z'
one
three
$ printf 'one\ntwo\nthree' | rg -o 'e\z'
one
e
```


---

_Comment by @BurntSushi on 2021-06-01 00:06_

So I believe this wound up being a duplicate of #1739 (or rather, it was a duplicate of the problems witnessed in your initial examples). The main problem was that while ripgrep treats a line as its content _without_ the line terminator, its printer was not being careful about making the same assumption when re-running the regex engine for colors or replacements. It looks like the issues go away once the line terminator was stripped from the haystack. So I believe all your examples work more sensibly now. And in particular, it was making the output involving `\z` particularly inscrutable.

But I think there are still bugs lurking with `\z` in particular. In essence, `\z` is only supposed to match at the very end of the haystack. So when searching a file, I suppose that really should be EOF. But you could also say that it should match at the end of a line. So this looks reasonable:

```
$ printf 'one\ntwo\nthree' | rg 'e\z'
one
three
```

But the fact that `one` is matched is actually an unintended consequence of the fact that `e` is searched as a literal on its own, and then a match is confirmed by searching just the bounds of the line that contains `e`. So the regex engine sees the end of the line (without the line terminator) as the end of the haystack. But, if you provide a regex that isn't susceptible to literal optimizations...

```
$ printf 'one\ntwo\nthree' | rg '\w\z'
three
```

Oops. Now only `three` matches. And this kind of "feels" like a correct result. And indeed, if a line terminator followed `three`, then there is no match:

```
$ printf 'one\ntwo\nthree\n' | rg '\w\z'
$
```

But now I kind of think that's only a fluke and a result of where the roll buffer is positioned.

My thinking is that I'm not sure `\A` and `\z` can be meaningfully supported... It seems doomed to sub-optimal semantics that are manifest by the specific searching strategy. 

---

_Comment by @learnbyexample on 2021-06-01 11:23_

Thanks for the detailed response. I finally installed Rust and was able to build a local copy to test the new changes.

If I may split my issue into two parts:

1. `rg '^' -r '* '` and `rg '$' -r ' ;'` are now working as I would expect (this was my main issue which led to the next point)
2. Use of string anchors

I'd feel the two suggestions given below to be better than, for example, having different output for `rg 'e\z'` and `rg '\w\z'`. 'Line' can be separated by newline or NUL or CRLF. And hopefully, same behavior can be ensured when `-P` is also used (which has `\Z` as well). Given that multiline mode is disabled:

* Display a warning or error when string anchors `\A` and `\z` are used. Line anchors can be used instead.
* Make `\A` and `\z` behave exactly as line anchors. I wouldn't expect `\A` and `\z` to act on whole input content since only single 'line' mode is active.

---

Also, I get no output for these two commands (with **12.1.1** and the new changes):

```bash
$ printf 'one\ntwo\nthree\n' | rg '\z'
$ printf 'one\ntwo\nthree\n' | rg -U '\z'
```


---

_Comment by @BurntSushi on 2021-06-01 12:13_

So I think `rg '\z'` in that case not printing anything is correct. The `rg -U` case I'm less sure about. I note that if you change the input to remove the trailing line terminator, then:

```
$ printf 'one\ntwo\nthree' | rg -U '\z'
three
```

It kind of seems like if that matches, then your example should too. Interestingly, `pcre2grep` has the same behavior as ripgrep when the line terminator is present. But when you remove it...

```
$ printf 'one\ntwo\nthree' | pcre2grep -M '\z'
^C
```

It gets stuck in what appears to be an infinite loop. Neat.

> Given that multiline mode is disabled:
> 
>     * Display a warning or error when string anchors `\A` and `\z` are used. Line anchors can be used instead.
> 
>     * Make `\A` and `\z` behave exactly as line anchors. I wouldn't expect `\A` and `\z` to act on whole input content since only single 'line' mode is active.

Yeah both of those thoughts cross my mind as well. For the default regex engine, I can do either one without much fuss I believe. But PCRE2 doesn't expose a parser over its syntax, so doing syntactically correct transformations like this is kinda hard. I suppose we could do a heuristic, but it won't be able to cover all cases. (Since `(?-m:$)` and `\z` are equivalent, for example.)

---
