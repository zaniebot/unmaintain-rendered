```yaml
number: 4172
title: Add support for listing system toolchains
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/find-toolchains
created_at: 2024-06-09T16:22:09Z
updated_at: 2024-06-13T15:25:31Z
url: https://github.com/astral-sh/uv/pull/4172
synced_at: 2026-01-12T16:06:04Z
```

# Add support for listing system toolchains

---

_@zanieb_

Includes system interpreters in `uv toolchain list`.

This includes a refactor of `find_toolchain` to support iterating over all toolchains
that match a request rather than ending earlier.


---

_Renamed from "Add support for listing all available toolchains" to "Add support for listing system toolchains" by @zanieb on 2024-06-10 14:24_

---

_Comment by @zanieb on 2024-06-11 19:10_

Example

```
❯ uv toolchain list
cpython-3.12.3-macos-aarch64-none	/opt/homebrew/opt/python@3.12/bin/python3.12
cpython-3.12.3-macos-aarch64-none	/Users/zb/Library/Application Support/uv/toolchains/cpython-3.12.3-macos-aarch64-none/install/bin/python3
cpython-3.11.9-macos-aarch64-none
cpython-3.10.14-macos-aarch64-none
cpython-3.9.19-macos-aarch64-none	/Users/zb/Library/Application Support/uv/toolchains/cpython-3.9.19-macos-aarch64-none/install/bin/python3
cpython-3.9.6-macos-aarch64-none	/Library/Developer/CommandLineTools/usr/bin/python3
cpython-3.8.19-macos-aarch64-none	/Users/zb/Library/Application Support/uv/toolchains/cpython-3.8.19-macos-aarch64-none/install/bin/python3
```

```
❯ uv toolchain list --all-platforms
cpython-3.12.3-windows-x86_64-none
cpython-3.12.3-macos-x86_64-none
cpython-3.12.3-macos-aarch64-none	/opt/homebrew/opt/python@3.12/bin/python3.12
cpython-3.12.3-macos-aarch64-none	/Users/zb/Library/Application Support/uv/toolchains/cpython-3.12.3-macos-aarch64-none/install/bin/python3
cpython-3.12.3-linux-x86_64-musl
cpython-3.11.9-windows-x86_64-none
cpython-3.11.9-macos-x86_64-none
cpython-3.11.9-linux-x86_64-musl
cpython-3.10.14-windows-x86_64-none
cpython-3.10.14-macos-x86_64-none
cpython-3.10.14-linux-x86_64-musl
cpython-3.9.19-windows-x86_64-none
cpython-3.9.19-macos-x86_64-none
cpython-3.9.19-macos-aarch64-none	/Users/zb/Library/Application Support/uv/toolchains/cpython-3.9.19-macos-aarch64-none/install/bin/python3
cpython-3.9.19-linux-x86_64-musl
cpython-3.9.6-macos-aarch64-none	/Library/Developer/CommandLineTools/usr/bin/python3
cpython-3.8.19-windows-x86_64-none
cpython-3.8.19-macos-x86_64-none
cpython-3.8.19-macos-aarch64-none	/Users/zb/Library/Application Support/uv/toolchains/cpython-3.8.19-macos-aarch64-none/install/bin/python3
cpython-3.8.19-linux-x86_64-musl
cpython-3.7.9-windows-x86_64-none
cpython-3.7.9-macos-x86_64-none
```

```
❯ uv toolchain list --all-versions
cpython-3.12.3-macos-aarch64-none	/opt/homebrew/opt/python@3.12/bin/python3.12
cpython-3.12.3-macos-aarch64-none	/Users/zb/Library/Application Support/uv/toolchains/cpython-3.12.3-macos-aarch64-none/install/bin/python3
cpython-3.12.3-macos-aarch64-none
cpython-3.12.2-macos-aarch64-none
cpython-3.12.1-macos-aarch64-none
cpython-3.12.0-macos-aarch64-none
cpython-3.11.9-macos-aarch64-none
cpython-3.11.8-macos-aarch64-none
cpython-3.11.7-macos-aarch64-none
cpython-3.11.6-macos-aarch64-none
cpython-3.11.5-macos-aarch64-none
cpython-3.11.4-macos-aarch64-none
cpython-3.11.3-macos-aarch64-none
cpython-3.11.1-macos-aarch64-none
cpython-3.10.14-macos-aarch64-none
cpython-3.10.13-macos-aarch64-none
cpython-3.10.12-macos-aarch64-none
cpython-3.10.11-macos-aarch64-none
cpython-3.10.9-macos-aarch64-none
cpython-3.10.8-macos-aarch64-none
cpython-3.10.7-macos-aarch64-none
cpython-3.10.6-macos-aarch64-none
cpython-3.10.5-macos-aarch64-none
cpython-3.10.4-macos-aarch64-none
cpython-3.10.3-macos-aarch64-none
cpython-3.10.2-macos-aarch64-none
cpython-3.10.0-macos-aarch64-none
cpython-3.9.19-macos-aarch64-none	/Users/zb/Library/Application Support/uv/toolchains/cpython-3.9.19-macos-aarch64-none/install/bin/python3
cpython-3.9.19-macos-aarch64-none
cpython-3.9.18-macos-aarch64-none
cpython-3.9.17-macos-aarch64-none
cpython-3.9.16-macos-aarch64-none
cpython-3.9.15-macos-aarch64-none
cpython-3.9.14-macos-aarch64-none
cpython-3.9.13-macos-aarch64-none
cpython-3.9.12-macos-aarch64-none
cpython-3.9.11-macos-aarch64-none
cpython-3.9.10-macos-aarch64-none
cpython-3.9.7-macos-aarch64-none
cpython-3.9.6-macos-aarch64-none	/Library/Developer/CommandLineTools/usr/bin/python3
cpython-3.9.6-macos-aarch64-none
cpython-3.9.5-macos-aarch64-none
cpython-3.9.4-macos-aarch64-none
cpython-3.9.3-macos-aarch64-none
cpython-3.9.2-macos-aarch64-none
cpython-3.8.19-macos-aarch64-none	/Users/zb/Library/Application Support/uv/toolchains/cpython-3.8.19-macos-aarch64-none/install/bin/python3
cpython-3.8.19-macos-aarch64-none
cpython-3.8.18-macos-aarch64-none
cpython-3.8.17-macos-aarch64-none
cpython-3.8.16-macos-aarch64-none
cpython-3.8.15-macos-aarch64-none
cpython-3.8.14-macos-aarch64-none
cpython-3.8.13-macos-aarch64-none
cpython-3.8.12-macos-aarch64-none
```


---

_Comment by @zanieb on 2024-06-11 19:12_

Will probably add `--all-architectures` and `--all-libc` flags eventually because they seems more useful than `--all-platforms` but I'm going to wait. What I did here felt like the minimum necessary for decent output.


---

_Marked ready for review by @zanieb on 2024-06-11 19:12_

---

_Review requested from @konstin by @zanieb on 2024-06-12 14:11_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-12 14:11_

---

_Label `preview` added by @zanieb on 2024-06-12 18:30_

---

_Review requested from @BurntSushi by @zanieb on 2024-06-13 13:19_

---

_Comment by @BurntSushi on 2024-06-13 13:41_

Still reviewing, but the first question in my head when I see the output is: why do some installations have a path and some don't?

---

_Comment by @zanieb on 2024-06-13 13:44_

The ones that don't have a path aren't downloaded yet. Couldn't find a nice way to represent that yet.

---

_Review comment by @BurntSushi on `crates/uv-toolchain/src/discovery.rs`:601 on 2024-06-13 13:47_

Maybe... `result.map_err(|e| should_stop_discovery(&e)).err().unwrap_or(true)`? I think that works, but it's a little subtle because `err()` is turning the `Result<Foo, bool>` into `Option<bool>`. Could also be written `result.err().map_or(true, |e| should_stop_discovery(&e))`.

---

_Review comment by @BurntSushi on `crates/uv/src/commands/toolchain/list.rs`:69 on 2024-06-13 13:49_

I think you can just do `.filter_map(|result| result.ok())` here.

---

_Review comment by @BurntSushi on `crates/uv/src/commands/toolchain/list.rs`:80 on 2024-06-13 13:52_

Oh okay, I see now. The significance of the path in the output is that it indicates a toolchain that is actually installed? But any entry _without_ a path means it is merely available and not installed?

The other thing that I'm a little confused by in the output:

```
❯ uv toolchain list --all-versions
cpython-3.12.3-macos-aarch64-none	/opt/homebrew/opt/python@3.12/bin/python3.12
cpython-3.12.3-macos-aarch64-none	/Users/zb/Library/Application Support/uv/toolchains/cpython-3.12.3-macos-aarch64-none/install/bin/python3
cpython-3.12.3-macos-aarch64-none
```

Here, `cpython-3.12.3-macos-aarch64-none` is repeated three times. I get the first two: the output makes it clear they are two distinct installations on my system. But what's the third? I guess it means it's available but not installed? But I have two of them installed already?

---

_@BurntSushi approved on 2024-06-13 13:55_

LGTM!

Concerning the output, I think my confusion stems from a very strong a priori bias that the list I'm looking at reflects toolchains that are already installed. Maybe I'm an odd duck and most won't have that bias, and perhaps therefore most won't be confused. Not sure.

One idea is to add something like `<available to install>` or something for each toolchain that is available but not installed. But... I could see that being noisy. And it's just kind of redundant info repeated a lot.

I think I still personally like listing only installed toolchains by default, and having an opt-in flag like `--available` or whatever that shows the bigger output.

---

_@zanieb reviewed on 2024-06-13 14:22_

---

_Review comment by @zanieb on `crates/uv/src/commands/toolchain/list.rs`:80 on 2024-06-13 14:22_

> The significance of the path in the output is that it indicates a toolchain that is actually installed? But any entry without a path means it is merely available and not installed?

Yes

> But what's the third? I guess it means it's available but not installed? But I have two of them installed already?

Ah we shouldn't list the download there, that's a bug.

---

_@zanieb reviewed on 2024-06-13 14:29_

---

_Review comment by @zanieb on `crates/uv/src/commands/toolchain/list.rs`:80 on 2024-06-13 14:29_

(fixed)

---

_@zanieb reviewed on 2024-06-13 14:33_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:601 on 2024-06-13 14:33_

Opting for `result.as_ref().err().map_or(true, should_stop_discovery)` 

---

_Comment by @zanieb on 2024-06-13 14:35_

Thanks for the review.

I think you have some fair concerns about the output. Maybe I'm over-optimizing on the fact that we dynamically fetch requested interpreters so it doesn't really matter if something is installed or not because you can `uv run --python 3.12.3` regardless. I'm trying to do a bit of a paradigm shift here to reflect that so maybe I'll merge as is and we can tweak it based on feedback? I'm biasing towards trying something new during this experimental phase.

I added the `<download available>` output you mentioned, I think it's reasonable. A little annoying if you're trying to parse the output, I guess.

---

_Comment by @zanieb on 2024-06-13 14:37_

Maybe we _shouldn't_ show paths by default and just _exclude_ system interpreters that overlap with managed interpreters by default. Then the output would be nice and simple like I want :) I will explore... probably as a follow-up.

---

_Comment by @BurntSushi on 2024-06-13 14:40_

> I'm trying to do a bit of a paradigm shift here to reflect that so maybe I'll merge as is and we can tweak it based on feedback? I'm biasing towards trying something new during this experimental phase.

Totally in favor of this! If we're going to try something new, now is definitely the time.

> Maybe we shouldn't show paths by default and just exclude system interpreters that overlap with managed interpreters by default. Then the output would be nice and simple like I want :) I will explore... probably as a follow-up.

:+1:

---

_Merged by @zanieb on 2024-06-13 15:25_

---

_Closed by @zanieb on 2024-06-13 15:25_

---

_Branch deleted on 2024-06-13 15:25_

---
