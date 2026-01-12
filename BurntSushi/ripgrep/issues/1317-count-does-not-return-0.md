```yaml
number: 1317
title: "--count does not return 0"
type: issue
state: closed
author: OJFord
labels:
  - question
assignees: []
created_at: 2019-07-02T14:45:38Z
updated_at: 2020-05-08T11:38:17Z
url: https://github.com/BurntSushi/ripgrep/issues/1317
synced_at: 2026-01-12T16:13:23Z
```

# --count does not return 0

---

_@OJFord_

#### What version of ripgrep are you using?

ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

brew

#### What operating system are you using ripgrep on?

Darwin 18.6.0

#### Describe your question, feature request, or bug.

```
$ rg --count 'this does not match anything' f
$
$ rg --count 'this does match something' f
1
$
```

For scripting purposes, I'd have preferred `0` than no output.


I thought I might have to accept that it's done in pursuit of compatibility, but:
```
$ grep --count 'this does not match anything' f
0
$
```

The exit code is 1 too, which is also a break from grep, and not desirable IMO, or maybe it's just because there really was a hard error, which is also what stopped any output?

---

_Comment by @BurntSushi on 2019-07-02 14:50_

Interesting. I'm not sure this was intentional. Or at least, I can't think of any particular reason why I did this. However, if you run ripgrep over a _directory_ with the `--count` flag, then it will not print file paths with zero matches. grep does, but that divergence was intentional on my part because otherwise the output contains a lot of noise.

> The exit code is 1 too, which is also a break from grep, and not desirable IMO

An exit code of `1` for a non-match is correct and consistent with grep.

---

_Label `question` added by @BurntSushi on 2019-07-02 14:50_

---

_Comment by @OJFord on 2019-07-02 14:52_

> An exit code of 1 for a non-match is correct and consistent with grep.

~Not when `--count` is supplied, it exits `0` and with stdout '0'.~

Edit: Oh, no, sorry!

---

_Comment by @cebaa on 2019-08-27 14:00_

@BurntSushi 

Not sure if this is a right ticket to discuss this, but a short comment on this:

> but that divergence was intentional on my part because otherwise the output contains a lot of noise.

With `grep -cH needle app.log*`, you get something like:

```
app.log.2019-08-24:0
app.log.2019-08-25:3
app.log.2019-08-26:5
app.log.2019-08-27:0
```

which is visually useful for cases such as "interesting things happened on 25th and 26th, but not on 24th and 27th".

The difference is in the "not on 24th and 27th" part - without `rg` outputting zero counts it's not possible to distinguish between:

- No matches in those files
- Those files do not even exist
- You messed up a pattern and it's not matching files it should

It might be good to consider a flag to include zero counts as well. IMO it would help with sanity double-checks.


---

_Comment by @BurntSushi on 2019-08-27 14:13_

@cebaa I think that's a separate issue from this one. Might be good to open a new one.

---

_Comment by @cebaa on 2019-09-08 13:38_

@BurntSushi Roger that, opened https://github.com/BurntSushi/ripgrep/issues/1370

---

_Comment by @Backgammon- on 2019-09-25 15:41_

If I need to write shell arithmetic involving a count which can be zero (not that it matters, but I am counting connected external monitors), then the fact that ripgrep gives nothing instead of `0` is unnecessarily inconvenient and inconsistent, both with grep and with itself (compare if `wc` printed nothing when run on a single empty file).

```bash-3.2$ echo "matching string" > myfile
bash-3.2$ echo $(( 2 - $(grep -c 'matching string' myfile) ))
1
bash-3.2$ echo $(( 2 - $(grep -c 'nonmatching string' myfile) ))
2
bash-3.2$ echo $(( 2 - $(rg -c 'matching string' myfile) ))
1
bash-3.2$ echo $(( 2 - $(rg -c 'nonmatching string' myfile) ))
bash: 2 -  : syntax error: operand expected (error token is " ")
```
Working around it is easy enough (grep, or god forbid, add a conditional) but it would be nice if I didn't have to.

To preserve your intent re: noise: rather than diverging from grep by making zero counts print nothing, I think it would be better to give `0` when the count is actually zero, make `--count` over a directory omit empty counts by default, and add a flag to not omit empty counts when running over directories. This is slightly different from @cebaa's request, which just asks for the flag.

---

_Comment by @BurntSushi on 2020-05-08 11:38_

This can be mitigated with the `--include-zero` flag, which was added in #1405 (commit https://github.com/BurntSushi/ripgrep/commit/a070722ff27c7d57d47b1766de9d253fbfe5bef9) and is available in ripgrep 12.

---

_Closed by @BurntSushi on 2020-05-08 11:38_

---
