```yaml
number: 2140
title: "Use space as delimiter for `UV_EXTRA_INDEX_URL`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/delimit
created_at: 2024-03-03T18:54:54Z
updated_at: 2024-03-03T19:01:08Z
url: https://github.com/astral-sh/uv/pull/2140
synced_at: 2026-01-12T16:04:53Z
```

# Use space as delimiter for `UV_EXTRA_INDEX_URL`

---

_@charliermarsh_

## Summary

I was looking at something unrelated and saw this in the Clap docs.

Closes https://github.com/astral-sh/uv/issues/1702.

## Test Plan

```shell
❯ UV_EXTRA_INDEX_URL="https://google.com https://foo.com" cargo run pip compile -
    Finished dev [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/uv pip compile -`
index: None
extra_index_url: [Url(VerbatimUrl { url: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("google.com")), port: None, path: "/", query: None, fragment: None }, given: Some("https://google.com") }), Url(VerbatimUrl { url: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("foo.com")), port: None, path: "/", query: None, fragment: None }, given: Some("https://foo.com") })]
```


---

_Label `bug` added by @charliermarsh on 2024-03-03 18:54_

---

_Label `compatibility` added by @charliermarsh on 2024-03-03 18:54_

---

_Merged by @charliermarsh on 2024-03-03 19:01_

---

_Closed by @charliermarsh on 2024-03-03 19:01_

---

_Branch deleted on 2024-03-03 19:01_

---
