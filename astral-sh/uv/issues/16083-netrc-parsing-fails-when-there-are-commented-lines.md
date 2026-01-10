---
number: 16083
title: .netrc parsing fails when there are commented lines
type: issue
state: open
author: iluetkeb
labels:
  - bug
  - external
assignees: []
created_at: 2025-10-01T10:58:46Z
updated_at: 2025-10-02T08:04:02Z
url: https://github.com/astral-sh/uv/issues/16083
synced_at: 2026-01-10T01:26:03Z
---

# .netrc parsing fails when there are commented lines

---

_Issue opened by @iluetkeb on 2025-10-01 10:58_

### Summary

.netrc parsing fails with `WARN uv_auth::middleware Error reading netrc file: parsing error: bad follower token '<machine name>' (line 14) in the file '/home/<myuser>/.netrc'` on lines commented with '#'. This is with verbose logging enabled, otherwise no error shows.

Strictly speaking, `.netrc` do not have a comment syntaxbut most other tools just silently ignore lines like that, while uv stops parsing the .netrc file after such a line occurs.

It would be helpful if uv either reported such things more prominently, or continued parsing after such errors.

### Platform

Ubuntu 22.04

### Version

0.5.24

### Python version

3.13.1

---

_Label `bug` added by @iluetkeb on 2025-10-01 10:58_

---

_Comment by @charliermarsh on 2025-10-01 15:58_

We actually don't parse this ourselves -- we use the [rust-netrc](https://docs.rs/rust-netrc/latest/netrc/) crate. This may be better reported there?

---

_Label `external` added by @charliermarsh on 2025-10-01 15:58_

---

_Comment by @terror on 2025-10-01 16:55_

Would you be able to provide a sample `netrc` file that fails to parse? This might help in investigating the issue upstream.

---

_Comment by @iluetkeb on 2025-10-01 17:58_

Sure, here is one with a regular entry, a commented entry and then a regular entry again. The actual content doesn't matter, it fails immediately when encountering the comment. Most likely, it sees three elements in a line where only two are expected.
```
machine localhost
login user
password abc12345

# machine 127.0.0.1
# login none
# password PAT

machine remote
login root
password fine
```


---

_Referenced in [gribouille/netrc#12](../../gribouille/netrc/issues/12.md) on 2025-10-01 18:02_

---

_Comment by @terror on 2025-10-01 19:09_

> Sure, here is one with a regular entry, a commented entry and then a regular entry again. The actual content doesn't matter, it fails immediately when encountering the comment. Most likely, it sees three elements in a line where only two are expected.
> 
> ```
> machine localhost
> login user
> password abc12345
> 
> # machine 127.0.0.1
> # login none
> # password PAT
> 
> machine remote
> login root
> password fine
> ```

I've [put up a PR](https://github.com/gribouille/netrc/pull/13) in the upstream crate that should resolve this issue. Hoping for a speedy review/release so this can get fixed for `uv`.

---

_Comment by @iluetkeb on 2025-10-02 08:04_

@terror Awesome, thank you!

---
