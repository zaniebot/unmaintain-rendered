```yaml
number: 2488
title: "args: use non-capturing groups when joining multiple patterns"
type: pull_request
state: closed
author: gofri
labels:
  - rollup
assignees: []
base: master
head: master
created_at: 2023-04-13T06:42:06Z
updated_at: 2023-07-08T22:52:49Z
url: https://github.com/BurntSushi/ripgrep/pull/2488
synced_at: 2026-01-12T18:23:14Z
```

# args: use non-capturing groups when joining multiple patterns

---

_@gofri_

Fixes #2480

Fix the joined pattern so that multiple VALID patterns cannot affect each other.
e.g. worked before but fixed now:
`echo FooBar | rg -e '(?i)notfoo' -e bar # no result` 

Wontfix the joined pattern to handle some cases of multiple INVALID patterns that are "coordinated" to fix each other.
e.g. still works but is unexpected:
`echo 'FooBar' | rg -e '(' -e ')' # matches with an empty string`

also, introduces a new class of invalid patterns that work now but wouldn't work before:
e.g. got parse error before but not now:
`echo 'FooBar' | rg ')(' # matches with an empty string`

edit: pushed another commit to avoid the regerssion for a single pattern.
the new class still applies for multiple patterns:
e.g. got parse error before but not now:
`echo 'FooBar' | rg -e ')(' -e 'x' # matches with an empty string`

---

_Comment by @gofri on 2023-04-13 06:56_

@BurntSushi I figured the new case just after creating the PR:

> also, introduces a new class of invalid patterns that work now but wouldn't work before:
e.g. got parse error before but not now:
echo 'FooBar' | rg ')(' # matches with an empty string

IMHO:
It's not too bad to have this new class for multiple patterns, but I really don't like the fact that it also affects the detection for a single pattern (namely, `)(`).
let me know if you think I should handle `len(patterns) == 1` differently 

edit: I pushed the commit to fix it, so let me know if you prefer that I revert it.

---

_Label `rollup` added by @BurntSushi on 2023-07-07 13:39_

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---
