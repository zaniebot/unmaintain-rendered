```yaml
number: 2247
title: use aho-corasick when -F and -x are set
type: issue
state: open
author: BurntSushi
labels: []
assignees: []
created_at: 2022-06-24T16:55:36Z
updated_at: 2023-11-24T19:34:04Z
url: https://github.com/BurntSushi/ripgrep/issues/2247
synced_at: 2026-01-12T16:13:24Z
```

# use aho-corasick when -F and -x are set

---

_@BurntSushi_

Currently, when `-x/--line-regexp` and `-F` are given to ripgrep, Aho-Corasick won't be used because the `-x` flag turns each pattern into a regex via `(?m)^(?:pattern)$`. This in turn causes the Aho-Corasick optimization to get defeated. In cases where the number of patterns is very large, using the regex engine for this much much slower than Aho-Corasick.

See a discussion on this that motivated this ticket:

### Discussed in https://github.com/BurntSushi/ripgrep/discussions/2244

<div type='discussions-op-text'>

<sup>Originally posted by **T145** June 24, 2022</sup>
I'm aware that ripgrep has set memory limits that [can cause problems](https://github.com/BurntSushi/ripgrep/issues/362#issuecomment-355848324), and that has been encountered while trying to perform the following operation:
```
rg -NFxvf source.txt target.txt
```
Where the source is `~8MB` and the target is `~168MB`. What should be the memory allocations I use in order to handle this workload?</div>

---

_Comment by @BurntSushi on 2022-06-24 17:05_

I looked into this briefly, but the existing Aho-Corasick optimization is a giant hack. Ideally this would be automatically handled by the regex engine. But if I can't make that work, then we should try to make the optimization in ripgrep a bit more robust.

---

_Comment by @BurntSushi on 2023-11-24 19:34_

I took another quick peek at this and I think the right way to go about this would be to add an optimization in the meta regex engine in `regex-automata` that specifically looks for patterns like `<look-around-assertion>(alternation|of|literals)<look-around-assertion>`. And then create a prefilter for the alternation of literals. This has the benefit of working for both the `-x` and `-w` flags.

This wasn't possible before because the old regex engine didn't know how to resolve look-around assertions after a prefilter match.

One alternative that would be worth exploring is to see whether the existing literal extraction can be augmented for this case. It somewhat intentionally does not handle this case because it tries to keep its literal sets small, since large literal sets tend to be counter-productive.

---
