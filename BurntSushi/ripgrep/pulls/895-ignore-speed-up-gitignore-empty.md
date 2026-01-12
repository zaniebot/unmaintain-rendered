```yaml
number: 895
title: "ignore: speed up Gitignore::empty"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/faster-gitignore-empty
created_at: 2018-04-24T14:42:54Z
updated_at: 2018-04-24T17:52:16Z
url: https://github.com/BurntSushi/ripgrep/pull/895
synced_at: 2026-01-12T18:23:13Z
```

# ignore: speed up Gitignore::empty

---

_@BurntSushi_

This commit makes Gitignore::empty a bit faster by avoiding allocation
and manually specializing the implementation instead of routing it through
the GitignoreBuilder.

This helps improve uses of ripgrep that traverse *many* directories, and
in particular, when the use of ignores is disabled via command line
switches.

Fixes #835, Closes #836

---

_Comment by @BurntSushi on 2018-04-24 14:50_

@sharkdp I think this should resolve the performance regression. I think my [initial analysis here](https://github.com/BurntSushi/ripgrep/pull/836#issuecomment-372030260) wasn't quite the whole story. In addition to the overhead of `create_gitignore`, it turns out that `Gitignore::empty()` had a fair bit of cost associated with it as well.

Here are the benchmarks (first is current master, second is your PR on top of current master and third is this PR):

```
$ hyperfine -s basic --warmup 10 'rg-baseline -uu --files -g "README.rst"'
Benchmark #1: rg-baseline -uu --files -g "README.rst"
  Time (mean ± σ):     596.9 ms ±   4.7 ms    [User: 3.815 s, System: 3.200 s]
  Range (min … max):   589.8 ms … 604.1 ms

$ hyperfine -s basic --warmup 10 'rg-sharkdp -uu --files -g "README.rst"'
Benchmark #1: rg-sharkdp -uu --files -g "README.rst"
  Time (mean ± σ):     544.0 ms ±   2.8 ms    [User: 3.287 s, System: 3.112 s]
  Range (min … max):   540.3 ms … 548.5 ms

$ hyperfine -s basic --warmup 10 'rg -uu --files -g "README.rst"'
Benchmark #1: rg -uu --files -g "README.rst"
  Time (mean ± σ):     554.9 ms ±   6.8 ms    [User: 3.365 s, System: 3.160 s]
  Range (min … max):   547.7 ms … 566.3 ms
```

One important observation here is that because `-u` is used, the calls to `matched` on the `Gitignore` matchers never actually take place since `has_any_ignore_rules` returns `false`.

Your PR is still a hair faster, but I'm not inclined to it since it spreads case analysis out too much. The likely cause of the performance difference is the additional function calls/copying when calling `Gitignore::empty` as opposed to just using `None`. One possible middle ground is to change `Gitignore` to be `pub struct Gitignore(Option<GitignoreInner>)` which would make the implementation of `Gitignore::empty()` equivalent to your PR without spreading out the case analysis. I'm not inclined to think it is worth it though.

---

_Merged by @BurntSushi on 2018-04-24 15:19_

---

_Closed by @BurntSushi on 2018-04-24 15:19_

---

_Branch deleted on 2018-04-24 15:23_

---

_Comment by @sharkdp on 2018-04-24 17:52_

Very cool - thank you very much for looking into this!

I've run the benchmark again and see no significant difference between my PR and yours.

---
