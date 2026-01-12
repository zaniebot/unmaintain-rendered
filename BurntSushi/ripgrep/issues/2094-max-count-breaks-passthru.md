```yaml
number: 2094
title: "`--max-count` breaks `-passthru`"
type: issue
state: closed
author: balupton
labels:
  - bug
assignees: []
created_at: 2021-12-02T05:24:14Z
updated_at: 2025-10-12T16:46:19Z
url: https://github.com/BurntSushi/ripgrep/issues/2094
synced_at: 2026-01-12T16:13:24Z
```

# `--max-count` breaks `-passthru`

---

_@balupton_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

n/a

#### What operating system are you using ripgrep on?

```
Darwin redacted 21.1.0 Darwin Kernel Version 21.1.0: Wed Oct 13 17:33:23 PDT 2021; root:xnu-8019.41.5~1/RELEASE_X86_64 x86_64
```

#### Describe your bug.

``` bash
echo $'a\nb\nc\na\nb\nc' | rg --no-line-number --color never --multiline -m 1 --passthru 'b' --replace 'B'
```

outputs

```
a
B
```

not the expected output of:

```
a
B
c
a
b
c
```

This breaks the beneficial usage of `--passthru`, which is often used like so:

``` bash
rg --multiline --passthru --max-count 1 "$pattern" --replace "$replace" "$file" | sponge "$file"
```

In my use case, I want to replace the first match with something, and trim away the remaining matches:

``` bash
random="$RANDOM$RANDOM$RANDOM$RANDOM"
if rg --multiline --passthru --max-count 1 "$pattern" --replace "$random" "$file" | sponge "$file"; then
    rg --multiline --passthru "$pattern" --replace '' "$file" | sponge "$file" || :
    rg --multiline --passthru "$random" --replace "$replace" "$file" | sponge "$file"
fi
```

#### What are the steps to reproduce the behavior?

Run the code snippet earlier.

#### What is the actual behavior?

Run the code snippet earlier.

#### What is the expected behavior?

Documented earlier.


---

_Comment by @BurntSushi on 2021-12-02 15:09_

This is an interesting use case. It's not clear to me that "breaks" is the right word here... If the behavior were as you wanted, someone else might say that "`--passthru` breaks `--max-count` because ripgrep continues searching even after it hits the maximum matches." But, I do tend to think that your suggestion is probably the right one. So I'll mark this as a bug.

---

_Label `bug` added by @BurntSushi on 2021-12-02 15:09_

---

_Comment by @balupton on 2021-12-03 14:41_

As an alternative, a fresh experience that allows one to apply different replacements for different occurrence ranges would be nice, that way I could simplify the use case code prior to something like:

``` bash
if rg --quiet "$pattern" "$file"; then
    # replace the second and more occurrences with nothing
    rg --multiline --passthru --occurrence=2, "$pattern" --replace '' "$file" | sponge "$file" || :
    # as that leaves only the first occurrence, we don't need an occurrence range anymore
    rg --multiline --passthru "$pattern" --replace "$replace" "$file" | sponge "$file"
fi
```

I prosed something similar at https://github.com/greymd/teip/issues/27 and here https://github.com/chmln/sd/issues/105#issuecomment-984363282

Other ranges that could be added, but aren't part of my use case could be:

- `,5` for occurrences 1-5 (inclusive).
- `5,9` for occurrences 5-9 (inclusive).
- I am not sure if `5-9` is also a convention in other libraries, and I am not sure if the wider convention is for `,` to be inclusive and `-` to be exclusive, or if it is the other way round, or if there is no convention.

Re conventions:

- maths, non-programming:  https://stackoverflow.com/a/4396303/130638  / https://en.wikipedia.org/wiki/Bracket_(mathematics)#Intervals
- rust `..` exclusive `..=` inclusive: https://github.com/rust-lang/rust/issues/37854
- scala avoids the issue by using verbage instead http://allaboutscala.com/tutorials/chapter-2-learning-basics-scala-programming/scala-tutorial-learn-use-range-inclusive-exclusive/
- ruby `...` exclusive, `..` inclusive: https://www.reddit.com/r/ruby/comments/15omrb/why_is_ruby_range_syntax_this_way_15_exclusive/

[Rust regex manual](https://docs.rs/regex/latest/regex/#repetitions) has this:

```
x{n,m}    at least n x and at most m x (greedy)
x{n,}     at least n x (greedy)
x{n}      exactly n x
x{n,m}?   at least n x and at most m x (ungreedy/lazy)
x{n,}?    at least n x (ungreedy/lazy)
x{n}?     exactly n x
```

[cut manual](https://www.man7.org/linux/man-pages/man1/cut.1.html) has this:

```
  Use one, and only one of -b, -c or -f.  Each LIST is made up of
       one range, or many ranges separated by commas.  Selected input is
       written in the same order that it is read, and is written exactly
       once.  Each range is one of:

       N      N'th byte, character or field, counted from 1

       N-     from N'th byte, character or field, to end of line

       N-M    from N'th to M'th (included) byte, character or field

       -M     from first to M'th (included) byte, character or field
```

So seems `,` and `-` have been interchangeable as inclusive for the most part.

---

For the current functionality, say only caring about passthru up until an occurance. I think that would be better suited by a regex like `^.*\nthen what one actually wants` without `-passthru`.

---

_Closed by @BurntSushi on 2025-10-12 16:46_

---
