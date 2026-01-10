```yaml
number: 2135
title: Resolve versions appearing in multiple extra indexes
type: pull_request
state: closed
author: sapir
labels: []
assignees: []
base: main
head: main
created_at: 2024-03-02T23:22:08Z
updated_at: 2024-03-03T22:44:57Z
url: https://github.com/astral-sh/uv/pull/2135
synced_at: 2026-01-10T14:54:43Z
```

# Resolve versions appearing in multiple extra indexes

---

_Pull request opened by @sapir on 2024-03-02 23:22_

## Summary

This is intended to fix an issue similar to https://github.com/astral-sh/uv/issues/1451 and https://github.com/astral-sh/uv/pull/2083 for a case where there are multiple private extra-index-urls. afaik they're all equally trusted, so there shouldn't be any security issue.

## Test Plan

I really don't know how to test this properly.


---

_Comment by @notatallshaw on 2024-03-03 19:05_

> afaik they're all equally trusted, so there shouldn't be any security issue.

Why would you make that assumption? It's very possible the user does not know the trust level of an index, that they could be using multiple public indexes. This assumption is the problem that pip is now stuck with, and leads to dependency confusion attacks.

I appreciate that isn't a review of this PR but the approach, but I don't see an open issue linked that this is supposed to solve.

---

_Comment by @charliermarsh on 2024-03-03 19:25_

I appreciate the PR (nice job figuring this out) but I think we're unlikely to merge this, especially without some kind of user opt-in, since it does create additional safety issues that don't exist today.

---

_Comment by @sapir on 2024-03-03 22:44_

> > afaik they're all equally trusted, so there shouldn't be any security issue.
> 
> Why would you make that assumption? It's very possible the user does not know the trust level of an index, that they could be using multiple public indexes. This assumption is the problem that pip is now stuck with, and leads to dependency confusion attacks.
> 
> I appreciate that isn't a review of this PR but the approach, but I don't see an open issue linked that this is supposed to solve.

Apologies, I meant that this is my actual use case, where I trust the extra indexes equally. For now I've solved it by instead setting up a kind of reverse proxy that combines the various indexes.

---

_Closed by @sapir on 2024-03-03 22:44_

---
