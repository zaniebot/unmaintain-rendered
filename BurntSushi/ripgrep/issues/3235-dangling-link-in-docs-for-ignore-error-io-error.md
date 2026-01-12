```yaml
number: 3235
title: "Dangling link in docs for `ignore::Error::io_error`"
type: issue
state: open
author: Cerber-Ursi
labels: []
assignees: []
created_at: 2025-12-04T13:16:54Z
updated_at: 2025-12-04T14:19:26Z
url: https://github.com/BurntSushi/ripgrep/issues/3235
synced_at: 2026-01-12T16:13:25Z
```

# Dangling link in docs for `ignore::Error::io_error`

---

_@Cerber-Ursi_

In [docs](https://docs.rs/ignore/0.4.25/ignore/enum.Error.html#method.io_error) for `ignore::Error::io_error`, there's the following paragraph:
> This is the original [std::io::Error](https://doc.rust-lang.org/stable/std/io/struct.Error.html) and is not the same as [impl From<Error> for std::io::Error](https://docs.rs/ignore/0.4.25/ignore/struct.Error.html#impl-From%3CError%3E) which contains additional context about the error.

The second link however leads to "This resource does not exist" page (which is understandable, since there's no `struct Error` in `ignore`), and, furthermore, I can't see the implementation it could refer to, either - this link seems to be dangling from [the very beginning](https://github.com/BurntSushi/ripgrep/commit/873abecbf1c4fbac2e070ac33a25e7a23fea6700). Is this some artifact of the first unmerged implementation, or something that is actually expected to exist?

---

_Comment by @BurntSushi on 2025-12-04 13:19_

I believe it's referring to this impl: https://docs.rs/ignore/0.4.25/src/ignore/lib.rs.html#361-365

---

_Comment by @Cerber-Ursi on 2025-12-04 13:49_

It would be strange though, sine the impl you're linking to seems to be in the reverse direction - it's `impl From<io::Error> for ignore::Error`, not `impl From<ignore::Error> for io::Error`.

---

_Comment by @BurntSushi on 2025-12-04 13:57_

Hmmm yeah. I'm honestly not sure. Probably that paragraph should just be removed?

---

_Comment by @Cerber-Ursi on 2025-12-04 14:03_

Pinging original implementor @epage, just in case.

---

_Comment by @epage on 2025-12-04 14:08_

https://github.com/BurntSushi/ripgrep/pull/1740#discussion_r528348871

It was copied over from `WalkDir`, see https://docs.rs/walkdir/latest/walkdir/struct.Error.html#method.io_error.

The comment was introduced in BurntSushi/walkdir#92

---

_Comment by @Cerber-Ursi on 2025-12-04 14:19_

I guess the question is then, do we want to drop the comment or to add the `impl` (and fix the link)?

---
