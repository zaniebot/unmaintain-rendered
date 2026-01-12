```yaml
number: 13898
title: "Correct sync documentation to correct a misleading comment on `uv lock --check`"
type: pull_request
state: closed
author: avallbona
labels: []
assignees: []
base: main
head: avallbona-patch-1
created_at: 2025-06-08T07:53:20Z
updated_at: 2025-06-09T08:00:34Z
url: https://github.com/astral-sh/uv/pull/13898
synced_at: 2026-01-12T16:10:55Z
```

# Correct sync documentation to correct a misleading comment on `uv lock --check`

---

_@avallbona_

## Summary

Updated the [sync documentation](https://docs.astral.sh/uv/concepts/projects/sync/#checking-if-the-lockfile-is-up-to-date) because it was refering to an option `--check` that, it seems, it does not exist anymore.

Not sure if it's related to this [`UV_FROZEN=0 uv lock --check fails #`](https://github.com/astral-sh/uv/issues/13385)

## Test Plan

Execute the command:

```bash
uv lock --check
```
and got the output:

```bash
error: unexpected argument '--check' found
```



---

_Comment by @charliermarsh on 2025-06-09 00:21_

I think the comment is correct. What's your uv version?

```
❯ uv lock --check
Resolved 120 packages in 2ms
```

---

_Comment by @avallbona on 2025-06-09 07:58_

> I think the comment is correct. What's your uv version?
> 
> ```
> ❯ uv lock --check
> Resolved 120 packages in 2ms
> ```

My version is: `uv 0.5.5`
I tried with `uv 0.6.2` and it worked. 👍🏿 

Thanks and sorry for the noise.




---

_Closed by @avallbona on 2025-06-09 08:00_

---
