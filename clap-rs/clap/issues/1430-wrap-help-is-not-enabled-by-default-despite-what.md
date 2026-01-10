---
number: 1430
title: "`wrap_help` is not enabled by default (despite what the docs say)"
type: issue
state: closed
author: stepchowfun
labels:
  - A-docs
  - E-help-wanted
  - E-easy
assignees: []
created_at: 2019-03-15T11:13:50Z
updated_at: 2020-04-05T11:03:09Z
url: https://github.com/clap-rs/clap/issues/1430
synced_at: 2026-01-10T01:26:53Z
---

# `wrap_help` is not enabled by default (despite what the docs say)

---

_Issue opened by @stepchowfun on 2019-03-15 11:13_

### Rust Version

* rustc 1.33.0 (2aa4c46cf 2019-02-28)

### Affected Version of clap

* 2.32.0

### Bug or Feature Request Summary

The [documentation](https://docs.rs/clap/2.32.0/clap/#features-enabled-by-default) makes it seem like `wrap_help` is enabled by default:

<img width="932" alt="Screenshot 2019-03-15 04 10 09" src="https://user-images.githubusercontent.com/796574/54427618-3eae4780-46d8-11e9-8f54-579d879df8d4.png">

But it seems this feature is not actually enabled by default:

https://github.com/clap-rs/clap/blob/a10d25c8c01fe9f09d106546258c7c15130e444d/Cargo.toml#L47

### Expected Behavior Summary

The feature is enabled by default.

### Actual Behavior Summary

The feature is not enabled by default.

### Steps to Reproduce the issue

I built my test app with the default feature set:

```toml
[dependencies]
clap = "2"
```

Then I invoked it with the `--help` flag. The help text wrapped poorly. Then I rebuilt the application with:

```toml
[dependencies.clap]
version = "2"
default-features = true
features = ["wrap_help"]
```

Then the help text wrapped nicely.

### Sample Code or Link to Sample Code

N/A

### Debug output

N/A

---

_Comment by @CreepySkeleton on 2020-02-01 14:55_

I confirm the issue. Do we add this feature to defaults or adjust the doc?

---

_Label `C: docs` added by @CreepySkeleton on 2020-02-01 14:56_

---

_Label `good first issue` added by @pksunkara on 2020-02-01 19:23_

---

_Label `help wanted` added by @pksunkara on 2020-02-01 19:23_

---

_Added to milestone `3.0` by @pksunkara on 2020-02-01 19:23_

---

_Comment by @Dylan-DPC-zz on 2020-04-02 17:15_

@stepchowfun how about sending a PR that adjusts the docs?

---

_Comment by @stepchowfun on 2020-04-02 17:51_

I'd be happy to submit the PR. I can take a look some time in the next few days.

---

_Referenced in [clap-rs/clap#1785](../../clap-rs/clap/pulls/1785.md) on 2020-04-04 20:23_

---

_Closed by @bors[bot] on 2020-04-05 11:03_

---
