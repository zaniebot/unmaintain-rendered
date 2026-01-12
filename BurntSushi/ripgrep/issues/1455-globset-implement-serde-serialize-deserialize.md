```yaml
number: 1455
title: "globset: Implement serde Serialize/Deserialize"
type: issue
state: closed
author: LPGhatguy
labels: []
assignees: []
created_at: 2020-01-03T23:45:28Z
updated_at: 2020-02-21T12:40:48Z
url: https://github.com/BurntSushi/ripgrep/issues/1455
synced_at: 2026-01-12T16:13:23Z
```

# globset: Implement serde Serialize/Deserialize

---

_@LPGhatguy_

My project currently uses a wrapper type around globset's `GlobMatcher` in order to implement `Serialize` and `Deserialize`. The implementation [is pretty trivial](https://github.com/rojo-rbx/rojo/blob/c82bc8a3583a65279d7aa28cde4b53977cd78fc4/src/glob.rs#L38-L50)!

Does it make sense for the globset crate to implement `Serialize`/`Deserialize`? I'd like to have them on both the `Glob` and `GlobMatcher` types for myself, but it's possible other types make sense. This would probably want to be behind a feature flag.

If this sounds okay, I'll file a PR! I think this change is similar to the spirit of #1447, but that PR hasn't been reviewed yet as of time of this writing.

---

_Comment by @LPGhatguy on 2020-02-19 01:40_

Looks like #1447 was just rolled up and merged! Since it's low effort, I'll get a PR up.

---

_Closed by @BurntSushi on 2020-02-21 12:40_

---
