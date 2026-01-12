```yaml
number: 16245
title: "ci(github-actions): as ubuntu 20.04 is deprecated and will not be supported, this PR update ubuntu from 20.04 to 24.04"
type: pull_request
state: closed
author: Lee-W
labels: []
assignees: []
draft: true
base: main
head: upgrade-ubuntue-20-04-to-latest
created_at: 2025-02-19T06:19:43Z
updated_at: 2025-02-19T07:59:45Z
url: https://github.com/astral-sh/ruff/pull/16245
synced_at: 2026-01-12T15:55:54Z
```

# ci(github-actions): as ubuntu 20.04 is deprecated and will not be supported, this PR update ubuntu from 20.04 to 24.04

---

_@Lee-W_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Replace ubuntu 20.04 with 24.04 as 20.04 has been deprecated and will soon be unsupported

https://github.com/actions/runner-images/issues/11101

## Test Plan

<!-- How was it tested? -->

This PR does not change any rules


---

_Comment by @dhruvmanila on 2025-02-19 06:40_

Sorry if I wasn't clear in https://github.com/astral-sh/ruff/issues/16244, but `release.yml` is auto-generated and it will require `cargo-dist` to upgrade the Ubuntu version on their end (https://github.com/axodotdev/cargo-dist/issues/1760) to update the `release.yml` file on our end.

---

_Converted to draft by @Lee-W on 2025-02-19 06:42_

---

_Comment by @Lee-W on 2025-02-19 06:44_

> Sorry if I wasn't clear in #16244, but `release.yml` is auto-generated and it will require `cargo-dist` to upgrade the Ubuntu version on their end ([axodotdev/cargo-dist#1760](https://github.com/axodotdev/cargo-dist/issues/1760)) to update the `release.yml` file on our end.

ah, sorry, I did not notice this one... I'll make it draft and see what I can do on cargo-dist side

---

_Comment by @Lee-W on 2025-02-19 07:09_

I took a look at the discussion. I guess it means we want to wait for their patch instead of using `[github-custom-runners](https://opensource.axo.dev/cargo-dist/book/reference/config.html#github-custom-runners)`?

---

_Closed by @Lee-W on 2025-02-19 07:21_

---

_Comment by @dhruvmanila on 2025-02-19 07:57_

> I took a look at the discussion. I guess it means we want to wait for their patch instead of using `[github-custom-runners](https://opensource.axo.dev/cargo-dist/book/reference/config.html#github-custom-runners)`?

Yes. I don't think using custom runners is the solution here as `cargo-dist` have acknowledged this and are going to switch, we just have to wait until then.

---

_Comment by @Lee-W on 2025-02-19 07:59_

> > I took a look at the discussion. I guess it means we want to wait for their patch instead of using `[github-custom-runners](https://opensource.axo.dev/cargo-dist/book/reference/config.html#github-custom-runners)`?
> 
> Yes. I don't think using custom runners is the solution here as `cargo-dist` have acknowledged this and are going to switch, we just have to wait until then.

Agree. Thanks!

---
