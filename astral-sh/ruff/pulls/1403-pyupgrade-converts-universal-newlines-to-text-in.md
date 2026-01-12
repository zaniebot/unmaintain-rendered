```yaml
number: 1403
title: "Pyupgrade: converts `universal_newlines` to `text` in `subprocess.run`"
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: un_nl_to_txt
created_at: 2022-12-27T13:43:34Z
updated_at: 2022-12-27T23:30:38Z
url: https://github.com/astral-sh/ruff/pull/1403
synced_at: 2026-01-12T05:36:31Z
```

# Pyupgrade: converts `universal_newlines` to `text` in `subprocess.run`

---

_Pull request opened by @colin99d on 2022-12-27 13:43_

A part of #827.

---

_Review comment by @squiddy on `src/pyupgrade/plugins/replace_universal_newlines.rs`:24 on 2022-12-27 13:49_

Check out `ast::helpers::find_keyword`. I think you can rewrite these as

```rust
let Some(the_kwarg) = find_keyword(kwargs, "universal_newlines") else { return; };
```

---

_@squiddy reviewed on 2022-12-27 13:49_

---

_@colin99d reviewed on 2022-12-27 13:52_

---

_Review comment by @colin99d on `src/pyupgrade/plugins/replace_universal_newlines.rs`:24 on 2022-12-27 13:52_

Awesome, thank you for this.

---

_Comment by @colin99d on 2022-12-27 14:04_

`This file is outdated. You may have to rerun 'cargo dev generate-options' and/or 'cargo dev generate-rules-table'.`
@charliermarsh if we have all these commands that just need to be run every time, why not add it do the CI so that contributors don't need to worry about it?

---

_Comment by @charliermarsh on 2022-12-27 14:06_

@colin99d - I'm hoping to make it a build step so that it all gets built automatically.

---

_Comment by @charliermarsh on 2022-12-27 14:07_

Eh, maybe some of these are inappropriate for a `cargo build` script.

---

_Merged by @charliermarsh on 2022-12-27 17:01_

---

_Closed by @charliermarsh on 2022-12-27 17:01_

---

_@charliermarsh reviewed on 2022-12-27 17:02_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/replace_universal_newlines.rs`:23 on 2022-12-27 17:02_

@colin99d - I think I was able to simplify this a little bit.

---

_Branch deleted on 2022-12-27 23:30_

---
