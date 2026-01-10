```yaml
number: 16154
title: fix compile error on missing uv-python metadata
type: pull_request
state: merged
author: dead10ck
labels: []
assignees: []
merged: true
base: main
head: fix/uv-python-build
created_at: 2025-10-07T15:57:29Z
updated_at: 2025-10-09T19:02:44Z
url: https://github.com/astral-sh/uv/pull/16154
synced_at: 2026-01-10T06:36:15Z
```

# fix compile error on missing uv-python metadata

---

_Pull request opened by @dead10ck on 2025-10-07 15:57_

The `uv-python` crate reads the checked-in download metadata to write out a minified version to be statically included into the binary.

Sometimes, the resulting `download-metadata-minified.json` can go missing, such as when `uv` is installed via `cargo` as a git source, and there is an update; `cargo` will do a hard reset on the git repo it stores in the cache, causing it to be deleted. In these instances, the `uv-python` crate does not currently re-run its build script, because as it happens, there was no change to the `download-metadata.json` since it was last compiled. This causes a build error like:

```
            Updating git repository `https://github.com/astral-sh/uv`
          Installing uv v0.8.24 (https://github.com/astral-sh/uv?tag=0.8.24#252f8873)
            Updating crates.io index
           Compiling uv-version v0.8.24 (/home/skhawtho/.cargo/git/checkouts/uv-c9e40703e19509a8/252f887/crates/uv-version)
           Compiling uv-git v0.0.1 (/home/skhawtho/.cargo/git/checkouts/uv-c9e40703e19509a8/252f887/crates/uv-git)
           Compiling uv-build-backend v0.1.0 (/home/skhawtho/.cargo/git/checkouts/uv-c9e40703e19509a8/252f887/crates/uv-build-backend)
           Compiling uv-configuration v0.0.1 (/home/skhawtho/.cargo/git/checkouts/uv-c9e40703e19509a8/252f887/crates/uv-configuration)
           Compiling uv-client v0.0.1 (/home/skhawtho/.cargo/git/checkouts/uv-c9e40703e19509a8/252f887/crates/uv-client)
           Compiling uv-extract v0.0.1 (/home/skhawtho/.cargo/git/checkouts/uv-c9e40703e19509a8/252f887/crates/uv-extract)
           Compiling uv-workspace v0.0.1 (/home/skhawtho/.cargo/git/checkouts/uv-c9e40703e19509a8/252f887/crates/uv-workspace)
           Compiling uv-python v0.0.1 (/home/skhawtho/.cargo/git/checkouts/uv-c9e40703e19509a8/252f887/crates/uv-python)
           Compiling uv-requirements-txt v0.0.1 (/home/skhawtho/.cargo/git/checkouts/uv-c9e40703e19509a8/252f887/crates/uv-requirements-txt)
           Compiling uv-bin-install v0.0.1 (/home/skhawtho/.cargo/git/checkouts/uv-c9e40703e19509a8/252f887/crates/uv-bin-install)
           Compiling uv-publish v0.1.0 (/home/skhawtho/.cargo/git/checkouts/uv-c9e40703e19509a8/252f887/crates/uv-publish)
        error: couldn't read `crates/uv-python/src/download-metadata-minified.json`: No such file or directory (os error 2)
           --> crates/uv-python/src/downloads.rs:795:45
            |
        795 | const BUILTIN_PYTHON_DOWNLOADS_JSON: &str = include_str!("download-metadata-minified.json");
            |                                             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

        error: could not compile `uv-python` (lib) due to 1 previous error
        error: failed to compile `uv v0.8.24 (https://github.com/astral-sh/uv?tag=0.8.24#252f8873)`, intermediate artifacts can be found at `/home/skhawtho/.cache/cargo`.
        To reuse those artifacts with a future compilation, set the environment variable `CARGO_TARGET_DIR` to that path.
```

This fixes the issue by also adding a `rerun-if-changed` on the resulting minified JSON file.

Separately, this revision also changes the output directory of the minified file to the build script's output directory via the `OUT_DIR` environment variable, so as not to pollute the source code.

---

_@zanieb approved on 2025-10-07 16:04_

Thanks!

---

_Comment by @zanieb on 2025-10-07 16:04_

Looks like

```
error: couldn't read `crates/uv-python/src/download-metadata-minified.json`: No such file or directory (os error 2)
   --> crates/uv-python/src/downloads.rs:795:45
    |
795 | const BUILTIN_PYTHON_DOWNLOADS_JSON: &str = include_str!("download-metadata-minified.json");
```

needs update

---

_Comment by @dead10ck on 2025-10-07 16:30_

Whoops, go figure, I forgot to update that path too, but it still compiled for me locally because it was still cached haha

---

_@zanieb approved on 2025-10-07 17:32_

---

_Merged by @zanieb on 2025-10-07 17:33_

---

_Closed by @zanieb on 2025-10-07 17:33_

---

_Comment by @zanieb on 2025-10-09 16:20_

@dead10ck I think this might be causing `uv-python` to re-compile on every `cargo` invocation?

---

_Comment by @dead10ck on 2025-10-09 18:29_

Shoot, you're right; it looks like `rerun-if-changed` is only based on mod times, so it's always different every run. I'll see if I can fix it

---

_Comment by @dead10ck on 2025-10-09 19:02_

I submitted another PR: https://github.com/astral-sh/uv/pull/16214

---
