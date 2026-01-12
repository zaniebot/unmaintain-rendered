```yaml
number: 404
title: Add an option to use Resolvo in lieu of PubGrub
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/resolvo
created_at: 2023-11-13T01:55:23Z
updated_at: 2024-01-05T01:22:13Z
url: https://github.com/astral-sh/uv/pull/404
synced_at: 2026-01-12T16:03:55Z
```

# Add an option to use Resolvo in lieu of PubGrub

---

_@charliermarsh_

## Summary

We're interested in exploring [resolvo](https://github.com/mamba-org/resolvo), to give us a point of comparison for [pubgrub-rs](https://github.com/pubgrub-rs/pubgrub). This PR implements a resolver on top of our existing infrastructure, but using resolvo instead of pubgrub-rs.

Specifically, you can now swap out the resolver via the CLI, like so:

```shell
cargo run -p puffin-cli -- pip-compile ./scripts/benchmarks/requirements.in --resolver pubgrub
cargo run -p puffin-cli -- pip-compile ./scripts/benchmarks/requirements.in --resolver resolvo
```

On `./scripts/benchmarks/requirements.in`, they produce identical resolutions.

The resolvo-based version is far less fully-featured, since I omitted everything that seemed unnecessary for comparing it to pubgrub-rs. Specifically, the resolvo-based resolver does _not_ support:

- Pre-fetching. In the PubGrub-based resolver, we aggressively pre-fetch metadata (e.g., when we select a package version, we go ahead and fetch the package-level metadata for all of its dependencies). That's harder to do in Resolvo-based resolver, so I just ignored it for now. **As such, we should really only compare cached resolutions for now.**
- URL dependencies.
- Git dependencies.
- Pre-release handling (instead, it filters out all pre-releases).
- Resolution modes (it always uses the highest-matching version).

It does, however, support:

- Source distributions (yay).
- Extras.

I don't plan to merge this (and am not even looking for a review -- this is more intended to fuel experimentation, so I'm opening it up here for visibility), though I think it did lead to some good generalizations that I'll likely pull-in via separate PRs.

---

_Comment by @charliermarsh on 2023-11-13 14:34_

(Test failures are expected. I simplified the output format to make it compatible between the two tools.)

---

_Comment by @zanieb on 2024-01-04 22:50_

Let's close for now? Relatively stale at this point.

---

_Closed by @charliermarsh on 2024-01-05 01:22_

---
