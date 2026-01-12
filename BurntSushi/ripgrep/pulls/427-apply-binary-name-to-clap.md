```yaml
number: 427
title: Apply binary name to clap
type: pull_request
state: closed
author: kpp
labels: []
assignees: []
base: master
head: feature_426_usage_bin_name
created_at: 2017-03-30T16:26:55Z
updated_at: 2017-03-30T19:11:09Z
url: https://github.com/BurntSushi/ripgrep/pull/427
synced_at: 2026-01-12T18:23:13Z
```

# Apply binary name to clap

---

_@kpp_

Fixes #426 

Example:
```
$ cargo run -- -h | head -n14 | tail -n 1
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/rg -h`
USAGE: rg [OPTIONS] <pattern> [--] [path]...
```

---

_Comment by @BurntSushi on 2017-03-30 16:36_

I don't think this is correct. The docs of `bin_name` say:

> Overrides the system-determined binary name. This should only be used when absolutely necessary, such as when the binary name for your application is misleading, or perhaps not how the user should invoke your program.

clap should be able to pick up the binary name automatically. So I think this is a bug in clap.

---

_Closed by @kpp on 2017-03-30 19:11_

---
