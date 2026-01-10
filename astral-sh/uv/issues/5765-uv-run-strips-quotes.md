---
number: 5765
title: uv run strips quotes
type: issue
state: closed
author: palfrey
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-04T19:37:36Z
updated_at: 2024-08-04T20:55:25Z
url: https://github.com/astral-sh/uv/issues/5765
synced_at: 2026-01-10T01:23:52Z
---

# uv run strips quotes

---

_Issue opened by @palfrey on 2024-08-04 19:37_

`uv run` seems to strip quotes from the command, and I'm yet to figure out a way around it.

Demo of the problem:
1. Dump the following into `demo.sh`
```bash
#!/bin/bash 
for i; do 
   echo $i
done
```
2. Run `uv run bash demo.sh foo bar` and `uv run bash demo.sh "foo bar"` and see how both treat foo and bar as two args and not one in the second case. `uv run bash demo.sh ""foo bar""` also behaves exactly the same...

Version: 0.2.33

---

_Comment by @charliermarsh on 2024-08-04 19:40_

Thanks, that seems off.

---

_Label `bug` added by @charliermarsh on 2024-08-04 19:40_

---

_Label `preview` added by @charliermarsh on 2024-08-04 19:40_

---

_Comment by @charliermarsh on 2024-08-04 20:35_

Wait sorry, what's the expected behavior here?

```
❯ ./demo.sh "foo bar"
foo bar

❯ cargo run -- run bash demo.sh "foo bar"
foo bar
```

---

_Comment by @palfrey on 2024-08-04 20:55_

Argh. Whoops, my fault. I've got a script I was running uv via (like https://github.com/palfrey/fenestra/blob/main/uv but in something currently unreleased) which I've just realised ends with the line `$UV $*` when it should have been `$UV "$@"` and fixing that removes the bug.

Apologies, thanks for checking things.

---

_Closed by @palfrey on 2024-08-04 20:55_

---
