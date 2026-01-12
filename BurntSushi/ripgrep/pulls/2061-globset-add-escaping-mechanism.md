```yaml
number: 2061
title: "globset: add escaping mechanism"
type: pull_request
state: closed
author: piegamesde
labels:
  - rollup
assignees: []
base: master
head: master
created_at: 2021-11-12T17:43:42Z
updated_at: 2023-07-08T22:53:12Z
url: https://github.com/BurntSushi/ripgrep/pull/2061
synced_at: 2026-01-12T18:23:14Z
```

# globset: add escaping mechanism

---

_@piegamesde_

Closes #2060. The implementation is taken 1:1 from the glob crate. Function signatures may be subject to bike shedding.

I feel that there may be some optimization potential in the `escape` method that doesn't allocate if there are no special characters to be escaped (probably by returning a `Cow`), but I would have to benchmark it and *creating* globs is usually not on the critical path.

I've seen some `Strategy` foo while skimming over the code. Will the crate automatically pick a `Literal` strategy or do I need to do something to activate it?

---

_Comment by @piegamesde on 2021-12-01 12:49_

@BurntSushi friendly ping

---

_Label `rollup` added by @BurntSushi on 2023-07-08 21:25_

---

_Comment by @BurntSushi on 2023-07-08 21:27_

Thanks! I had to trim this PR down quite a bit as you added a bunch of stuff beyond `pub fn escape(..) -> String` that I do not want.

And yes, `Literal` should automatically be selected. And if it isn't, we should fix that. But nothing else should care about escaping.

---

_Comment by @BurntSushi on 2023-07-08 21:27_

And yeah, definitely no need to optimize a routine like this. Your signature and implementation are more than sufficient.

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---
