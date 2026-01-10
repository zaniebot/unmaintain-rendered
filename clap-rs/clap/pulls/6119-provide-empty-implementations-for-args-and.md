---
number: 6119
title: Provide empty implementations for Args and Subcommand
type: pull_request
state: merged
author: alexkazik
labels: []
assignees: []
merged: true
base: master
head: empty-impl
created_at: 2025-09-02T16:03:43Z
updated_at: 2025-09-02T17:00:40Z
url: https://github.com/clap-rs/clap/pull/6119
synced_at: 2026-01-10T01:28:27Z
---

# Provide empty implementations for Args and Subcommand

---

_Pull request opened by @alexkazik on 2025-09-02 16:03_

Fixes #6117

---

_@epage reviewed on 2025-09-02 16:17_

---

_Review comment by @epage on `clap_builder/src/derive.rs`:435 on 2025-09-02 16:17_

Should this also error?

---

_@alexkazik reviewed on 2025-09-02 16:20_

---

_Review comment by @alexkazik on `clap_builder/src/derive.rs`:435 on 2025-09-02 16:20_

Since it's impossible to call update_from_arg_matches on Infallable I don't see why.

The other error is copied from the derive, maybe there is better.

(And I don't know what Lint Commits want from me.)

---

_Review comment by @epage on `clap_builder/src/derive.rs`:430 on 2025-09-02 16:21_

Our error messages should be lower case.  I found a couple that aren't and am preparing a separate PR for those.

---

_@epage reviewed on 2025-09-02 16:21_

---

_@epage reviewed on 2025-09-02 16:22_

---

_Review comment by @epage on `clap_builder/src/derive.rs`:435 on 2025-09-02 16:22_

> Since it's impossible to call update_from_arg_matches on Infallable I don't see why.

Can you put that assumption in the parameter to the macro?

> (And I don't know what Lint Commits want from me.)

We prefer but don't require [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/)

---

_@alexkazik reviewed on 2025-09-02 16:29_

---

_Review comment by @alexkazik on `clap_builder/src/derive.rs`:430 on 2025-09-02 16:29_

Since this is directly copied from (the derive output)[https://github.com/clap-rs/clap/blob/d045daa5c0c096280d4ff983915e56105d42cf4f/clap_derive/src/derives/subcommand.rs#L568] should I change it or leave as is for now?

If it's lower case then also without the dot at the end?

---

_@epage reviewed on 2025-09-02 16:52_

---

_Review comment by @epage on `clap_builder/src/derive.rs`:430 on 2025-09-02 16:52_

As mentioned, I'm preparing a PR for the other cases.

Yes, feel free to also drop the period.

---
