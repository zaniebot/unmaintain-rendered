```yaml
number: 598
title: plan roadmap to 1.0
type: issue
state: closed
author: dfabulich
labels: []
assignees: []
created_at: 2017-09-06T09:57:28Z
updated_at: 2017-10-22T01:01:27Z
url: https://github.com/BurntSushi/ripgrep/issues/598
synced_at: 2026-01-12T16:13:22Z
```

# plan roadmap to 1.0

---

_@dfabulich_

The latest ripgrep release is v0.6. Are there desired criteria for a 1.0 release? Are there missing features that are a "must have" for v1? Backwards compatible changes still planned?

(Or is it time to just bite the bullet and call it 1.0?)

---

_Comment by @BurntSushi on 2017-09-06 10:41_

The next "big" thing for ripgrep is mostly about restructuring its internal organization, which is tracked via the libripgrep milestone: https://github.com/BurntSushi/ripgrep/milestone/1 Some of those tickets have end user facing ramifications, such as tweaking the behavior of `-w` and adding multiline search.

Otherwise, I don't really have an answer for you. Biting the bullet and cutting 1.0 is premature without going through the issue tracker and thinking carefully about any other breaking changes that I might like to make. Obviously, releasing 1.0 doesn't mean there won't be a 2.0, but I'd like to be at least somewhat judicious about it.

The other thing that needs to get ironed out is how to deal with bumps in the minimum Rust version used. It can be difficult to resist the temptation to use new Rust features/library items, and in particular, the wider ecosystem is often eager to use them as well. Thus far, I've only increased the minimum Rust version on 0.x releases. Should I continue that with 1.x releases? Or should I only bump the minimum Rust version on major releases? My inclination is to continue bumping it on 1.x.

> Backwards compatible changes still planned?

Concretely, what I'd like to do is triage the issue tracker, get libripgrep finished and then we can *start* planning 1.0. I'd like to note that I'm in no real rush, and I wouldn't be surprised if 1.0 wasn't actually released until a year from now.

---

_Renamed from "Roadmap to 1.0?" to "plan roadmap to 1.0" by @BurntSushi on 2017-09-06 10:41_

---

_Comment by @BurntSushi on 2017-10-22 01:01_

I'm going to close this because it's somewhat vague and not particularly actionable. 1.0 will happen when it happens.

---

_Closed by @BurntSushi on 2017-10-22 01:01_

---
