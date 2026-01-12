```yaml
number: 2885
title: "regex: fix inner literal extraction that resulted in false negatives"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/fix-inner-literal-extraction
created_at: 2024-09-09T01:55:16Z
updated_at: 2024-09-09T13:05:12Z
url: https://github.com/BurntSushi/ripgrep/pull/2885
synced_at: 2026-01-12T18:23:14Z
```

# regex: fix inner literal extraction that resulted in false negatives

---

_@BurntSushi_

In some rare cases, it was possible for ripgrep's inner literal detector
to extract a set of literals that could produce a false negative. #2884
gives an example: `(?i:e.x|ex)`. In this case, the set extracted can be
discovered by running `rg '(?i:e.x|ex) --trace`:

    Seq[E("EX"), E("Ex"), E("eX"), E("ex")]

This extraction leads to building a multi-substring matcher for `EX`,
`Ex`, `eX` and `ex`. Searching the haystack `e-x` produces no match,
and thus, ripgrep shows no matches. But the regex `(?i:e.x|ex)` matches
`e-x`.

The issue at play here was that when two extracted literal sequences
were unioned, we were incorrectly unioning their "prefix" attribute.
And this in turn leads to those literal sequences being combined
incorrectly via cross product. This case in particular triggers it
because two different optimizations combine to produce an incorrect
result. Firslty, the regex has a common prefix extracted and is
rewritten as `(?i:e(?:.x|x))`. Secondly, the `x` in the first branch of
the alternation has its `prefix` attribute set to `false` (correctly),
which means it can't be cross producted with another concatenation. But
in this case, it is unioned with the `x` from the second branch, and
this results in the union result having `prefix` set to `true`. This
in turn pops up and lets it get cross producted with the `e` prefix,
producing an incorrect literal sequence.

We fix this by changing the implementation of `union` to return
`prefix` set to `true` only when *both* literal sequences being unioned
have `prefix` set to `true`.

Doing this exposed a second bug that was present, but was purely
cosmetic: the extracted literals in this case, after the fix, are
`X` and `x`. They were considered "exact" (i.e., lead to a match),
but of course they are not. Observing an `X` or an `x` does not mean
there is a match. This was fixed by making `choose` always return
an inexact literal sequence. This is perhaps too conservative in
aggregate in some cases, but always correct. The idea here is that if
one is choosing between two concatenations, then it is likely the case
that the sequence returned should be considered inexact. The issue
is that this can lead to avoiding cross products in some cases that
would otherwise be correct. This is bad because it means extracting
shorter literals in some cases. (In general, the longer the literal the
better.) But we prioritize correctness for now and fix it. You can see
a few tests where this shortens some extracted literals.

Fixes #2884


---

_Merged by @BurntSushi on 2024-09-09 02:00_

---

_Closed by @BurntSushi on 2024-09-09 02:00_

---

_Branch deleted on 2024-09-09 02:00_

---
