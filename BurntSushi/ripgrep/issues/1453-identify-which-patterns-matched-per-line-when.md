```yaml
number: 1453
title: Identify which patterns matched per line when using multiple patterns
type: issue
state: closed
author: prashanthellina
labels:
  - duplicate
assignees: []
created_at: 2020-01-03T15:57:04Z
updated_at: 2024-05-22T15:12:42Z
url: https://github.com/BurntSushi/ripgrep/issues/1453
synced_at: 2026-01-12T16:13:23Z
```

# Identify which patterns matched per line when using multiple patterns

---

_@prashanthellina_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

If you installed ripgrep with snap and are getting strange file permission or
file not found errors, then please do not file a bug. Instead, use one of the
Github binary releases.

#### What operating system are you using ripgrep on?

```
Distributor ID: Ubuntu
Description:    Ubuntu 19.10
Release:        19.10
Codename:       eoan
```

#### Describe your question, feature request, or bug.

When I do this,

```bash
rg -e "apple" -e "orange" myfile.text
```

The output has lines which match at least one pattern. It will be nice to have a feature where the output identifies which patterns matched.

---

_Label `duplicate` added by @BurntSushi on 2020-01-03 15:59_

---

_Comment by @BurntSushi on 2020-01-03 16:00_

Duplicate of #1286. Sorry.

---

_Closed by @BurntSushi on 2020-01-03 16:00_

---

_Comment by @TheTechromancer on 2024-05-22 15:12_

@prashanthellina did you end up finding a solution to your problem of correlating regex patterns to their matches? I have a similar need. Thanks!

---
