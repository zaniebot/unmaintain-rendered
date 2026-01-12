```yaml
number: 808
title: Potentially Upcoming Minor Breaking Change
type: issue
state: closed
author: kbknapp
labels:
  - M-breaking-change
assignees: []
created_at: 2017-01-05T16:26:35Z
updated_at: 2017-10-04T02:41:36Z
url: https://github.com/clap-rs/clap/issues/808
synced_at: 2026-01-12T16:14:09Z
```

# Potentially Upcoming Minor Breaking Change

---

_@kbknapp_

(*Note:* Those who already responded in the PR, need not repeat there answer. I'm simply moving the discussion to allow progress without breaking code :wink:)

## Proposed Minor Breaking Change in an upcoming release

There is a very minor breaking change that I'd like to make in an upcoming release without bumping the major version. Why is the major version not bumped? The following items:

 * it's to fix a bug in the original code
   * the fix is very beneficial to users of said feature
 * the fix for users is trivial and not always required
 * it's a niche feature

**Details:** `App::write_help` would require `&mut self` instead of `&self`. This allows the `App::write_help` to actually print the true and full help message, which isn't happening currently. That's it.

This means if you use

```rust
let app = /* .. */;
app.write_help(/* .. */);
```
Change your code to:

```rust
let mut app = // all else remains the same
```

Note `mut` app.

However, if you used

```rust
let app = // no semi-colon
    .write_help(/* .. */);
```

No change is required

---- 

A search on github found the following users of said feature. Please respond if you have objections to this release (specifically the change to your code when using `App::write_help`). 

- [ ] @quininer
- [ ] @landesfeind
- [x] @eagletmt
- [ ] @brayniac
- [x] @lise-henry
- [x] @faern
- [x] @Xion
- [ ] @pcwalton (I'm a big fan, thanks for your work on Rust! 😄)
- [x] @Phrohdoh
----

Relates to #801 

---

_Label `E: breaking change` added by @kbknapp on 2017-01-05 16:26_

---

_Label `T: RFC / question` added by @kbknapp on 2017-01-05 16:26_

---

_Assigned to @kbknapp by @kbknapp on 2017-01-05 16:26_

---

_Referenced in [clap-rs/clap#801](../../clap-rs/clap/issues/801.md) on 2017-01-05 16:27_

---

_Referenced in [clap-rs/clap#806](../../clap-rs/clap/pulls/806.md) on 2017-01-05 16:28_

---

_Renamed from "Upcoming Minor Breaking Change" to "Potentially Upcoming Minor Breaking Change" by @kbknapp on 2017-01-05 16:51_

---

_Closed by @kbknapp on 2017-10-04 02:41_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2018-01-31 01:45_

---

_Referenced in [clap-rs/clap#1924](../../clap-rs/clap/issues/1924.md) on 2020-05-13 11:55_

---

_Referenced in [ur0/lolcat#12](../../ur0/lolcat/pulls/12.md) on 2020-05-14 08:41_

---

_Referenced in [clap-rs/clap#1927](../../clap-rs/clap/pulls/1927.md) on 2020-05-14 12:25_

---
