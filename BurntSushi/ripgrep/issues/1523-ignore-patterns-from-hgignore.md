```yaml
number: 1523
title: Ignore patterns from .hgignore
type: issue
state: closed
author: naryl
labels:
  - duplicate
assignees: []
created_at: 2020-03-19T07:58:34Z
updated_at: 2021-02-26T09:56:50Z
url: https://github.com/BurntSushi/ripgrep/issues/1523
synced_at: 2026-01-12T16:13:23Z
```

# Ignore patterns from .hgignore

---

_@naryl_

#### What version of ripgrep are you using?

ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Funtoo main repository.

#### What operating system are you using ripgrep on?

Funtoo 1.4

#### Describe your question, feature request, or bug.

I'd like ripgrep to process ignore patterns from .hgignore just is it does for .gitignore. It's the only feature I use that The Silver Searcher has but ripgrep doesn't. The formats are a bit different so simply copying/linking all .hgignore files to .gitignore in every repo won't work as a workaround.

---

_Label `duplicate` added by @BurntSushi on 2020-03-19 10:06_

---

_Comment by @BurntSushi on 2020-03-19 10:16_

Duplicate of #6. 

> It's the only feature I use that The Silver Searcher has but ripgrep doesn't. The formats are a bit different so simply copying/linking all .hgignore files to .gitignore in every repo won't work as a workaround.

I'm sorry, but this makes me chuckle a bit. If The Silver Searcher's support for hgignore is good enough for you, then incidentally, symlinking `.hgignore` to `.gitignore` would _also_ be good enough for you. Hell, you might be able to just use `alias rg="rg --ignore-file .hgignore"` and have it work most of the time, depending on your `.hgignore` setup.

Despite the fact that The Silver Searcher advertises support for `.hgignore`, it treats it just like a `.gitignore` file with no specific support for hg's custom format. For example, it doesn't support hg's regex mode at all.

See:

https://github.com/ggreer/the_silver_searcher/blob/a509a8172b4a67803e1b367bf4356450bc97130b/src/ignore.c#L36

And:

https://github.com/ggreer/the_silver_searcher/blob/a509a8172b4a67803e1b367bf4356450bc97130b/src/search.c#L540-L546

I don't think I've ever seen anyone complain about this. Folks also seem to rarely complain about the litany of bugs in The Silver Searcher's gitignore support itself. The difference in the implementation of ripgrep's ignore support and The Silver Searcher is _dramatic_. Just to drive this point home, ripgrep's support for ignore patterns uses _more code than the entirety of The Silver Searcher_:

```
$ tokei -t C
-------------------------------------------------------------------------------
 Language            Files        Lines         Code     Comments       Blanks
-------------------------------------------------------------------------------
 C                      12         4603         3907          211          485
-------------------------------------------------------------------------------
 Total                  12         4603         3907          211          485
-------------------------------------------------------------------------------

$ tokei -t Rust crates/ignore/
-------------------------------------------------------------------------------
 Language            Files        Lines         Code     Comments       Blanks
-------------------------------------------------------------------------------
 Rust                   10         6274         4400         1317          557
-------------------------------------------------------------------------------
 Total                  10         6274         4400         1317          557
-------------------------------------------------------------------------------
```

This is due to substantial differences in both correctness and performance. (And this doesn't even include the custom glob implementation that ripgrep uses.)

---

_Closed by @BurntSushi on 2020-03-19 10:16_

---

_Comment by @naryl on 2020-03-19 12:58_

> If The Silver Searcher's support for hgignore is good enough for you, then incidentally, symlinking .hgignore to .gitignore would also be good enough for you.

Well, that actually solves it... I was under the impression that `ag` does something better but yes, its support is good enough.

---

_Comment by @merijn on 2021-02-26 09:56_

@BurntSushi 

> Hell, you might be able to just use alias rg="rg --ignore-file .hgignore" and have it work most of the time, depending on your .hgignore setup.

Tbh, this would probably solve 99%, if not 100%, of my uses. The main problem with this approach is that it only looks in the current directory and produces an error every single time you run `rg` in a directory without that file. Ideally there would be a way to specify the ignore file relative to the project's root and have a way to silence the error if it's missing, but I'm not sure those two things would warrant a separate issue?

---
