```yaml
number: 17399
title: How to avoid environment creation race conditions?
type: issue
state: open
author: colobas
labels:
  - question
assignees: []
created_at: 2026-01-10T00:30:58Z
updated_at: 2026-01-10T01:37:54Z
url: https://github.com/astral-sh/uv/issues/17399
synced_at: 2026-01-10T03:11:36Z
```

# How to avoid environment creation race conditions?

---

_Issue opened by @colobas on 2026-01-10 00:30_

### Question

I'm using uv with inline scripts in a nextflow pipeline, which involves running certain tasks many times in parallel. When two tasks implemented by the same script land on the same cluster node, I'm seeing an error which I believe is caused by a race condition in the environment creation:

```
  error: The directory `/tmp/gpires-uv-cache/environments-v2/build-parquet-0118330146dbc8a9` exists, but it's not a virtual environment
```

My interpretation of this is that one of the processes sees that the environment doesn't exist yet and starts creating it, the other process(es) arrive(s) a little bit later and see(s) the folder exists but isn't (yet) a valid environment.

This happens because the environment path is just based on a hash of the script path, as far as I can tell from [here](https://github.com/astral-sh/uv/blob/c10c84a588fa91ab52e54ea353f2c928356a9e1b/crates/uv/src/commands/project/mod.rs#L653-L674)

Likely I just have to avoid using inline scripts but I was wondering if you had other recommendations?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @colobas on 2026-01-10 00:30_

---

_Comment by @zanieb on 2026-01-10 01:37_

I'd expect us to handle this race condition.

We should probably lock the directory while creating and mutating it.

---
