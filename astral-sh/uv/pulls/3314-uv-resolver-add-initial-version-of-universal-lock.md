```yaml
number: 3314
title: "uv-resolver: add initial version of universal lock file format"
type: pull_request
state: merged
author: BurntSushi
labels:
  - preview
assignees: []
merged: true
base: main
head: ag/lock-file-format
created_at: 2024-04-29T16:07:22Z
updated_at: 2024-04-29T18:03:18Z
url: https://github.com/astral-sh/uv/pull/3314
synced_at: 2026-01-12T16:05:34Z
```

# uv-resolver: add initial version of universal lock file format

---

_@BurntSushi_

This is meant to be a base on which to build. There are some parts
which are implicitly incomplete and others which are explicitly
incomplete. The latter are indicated by TODO comments.

Here is a non-exhaustive list of incomplete things. In many cases, these
are incomplete simply because the data isn't present in a
`ResolutionGraph`. Future work will need to refactor our resolver so
that this data is correctly passed down.

* Not all wheels are included. Only the "selected" wheel for the current
  distribution is included.
* Marker expressions are always absent.
* We don't emit hashes for certainly kinds of distributions (direct
  URLs, git, and path).
* We don't capture git information from a dependency specification.
  Right now, we just always emit "default branch."

There are perhaps also other changes we might want to make to the format
of a more cosmetic nature. Right now, all arrays are encoded using
whatever the `toml` crate decides to do. But we might want to exert more
control over this. For example, by using inline tables or squashing more
things into strings (like I did for `Source` and `Hash`). I think the
main trade-off here is that table arrays are somewhat difficult to read
(especially without indentation), where as squashing things down into a
more condensed format potentially makes future compatible additions
harder.

I also went pretty light on the documentation here than what I would
normally do. That's primarily because I think this code is going to
go through some evolution and I didn't want to spend too much time
documenting something that is likely to change.

Finally, here's an example of the lock file format in TOML for the
`anyio` dependency. I generated it with the following command:

```
cargo run -p uv -- pip compile -p3.10 ~/astral/tmp/reqs/anyio.in --unstable-uv-lock-file
```

And that writes out a `uv.lock` file:

```toml
version = 1

[[distribution]]
name = "anyio"
version = "4.3.0"
source = "registry+https://pypi.org/simple"

[[distribution.wheel]]
url = "https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl"
hash = "sha256:048e05d0f6caeed70d731f3db756d35dcc1f35747c8c403364a8332c630441b8"

[[distribution.dependencies]]
name = "exceptiongroup"
version = "1.2.1"
source = "registry+https://pypi.org/simple"

[[distribution.dependencies]]
name = "idna"
version = "3.7"
source = "registry+https://pypi.org/simple"

[[distribution.dependencies]]
name = "sniffio"
version = "1.3.1"
source = "registry+https://pypi.org/simple"

[[distribution.dependencies]]
name = "typing-extensions"
version = "4.11.0"
source = "registry+https://pypi.org/simple"

[[distribution]]
name = "exceptiongroup"
version = "1.2.1"
source = "registry+https://pypi.org/simple"

[[distribution.wheel]]
url = "https://files.pythonhosted.org/packages/01/90/79fe92dd413a9cab314ef5c591b5aa9b9ba787ae4cadab75055b0ae00b33/exceptiongroup-1.2.1-py3-none-any.whl"
hash = "sha256:5258b9ed329c5bbdd31a309f53cbfb0b155341807f6ff7606a1e801a891b29ad"

[[distribution]]
name = "idna"
version = "3.7"
source = "registry+https://pypi.org/simple"

[[distribution.wheel]]
url = "https://files.pythonhosted.org/packages/e5/3e/741d8c82801c347547f8a2a06aa57dbb1992be9e948df2ea0eda2c8b79e8/idna-3.7-py3-none-any.whl"
hash = "sha256:82fee1fc78add43492d3a1898bfa6d8a904cc97d8427f683ed8e798d07761aa0"

[[distribution]]
name = "sniffio"
version = "1.3.1"
source = "registry+https://pypi.org/simple"

[[distribution.wheel]]
url = "https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl"
hash = "sha256:2f6da418d1f1e0fddd844478f41680e794e6051915791a034ff65e5f100525a2"

[[distribution]]
name = "typing-extensions"
version = "4.11.0"
source = "registry+https://pypi.org/simple"

[[distribution.wheel]]
url = "https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl"
hash = "sha256:c1f94d72897edaf4ce775bb7558d5b79d8126906a14ea5ed1635921406c0387a"
```


---

_Review requested from @konstin by @BurntSushi on 2024-04-29 16:07_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-04-29 16:07_

---

_Label `preview` added by @zanieb on 2024-04-29 16:09_

---

_Review comment by @konstin on `crates/distribution-types/src/file.rs`:197 on 2024-04-29 16:40_

Can we derive those with `thiserror::Error`?

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:223 on 2024-04-29 16:46_

I also ran into this problem a bunch; we're tracking some information on the urls of the dist types but it's not readily accessible

---

_@konstin approved on 2024-04-29 16:59_

---

_@BurntSushi reviewed on 2024-04-29 17:11_

---

_Review comment by @BurntSushi on `crates/distribution-types/src/file.rs`:197 on 2024-04-29 17:11_

My hands are very good at writing things out without macros because `thiserror` is IMO not usually worth the dependency. But I suppose that's irrelevant here.

---

_Merged by @BurntSushi on 2024-04-29 18:03_

---

_Closed by @BurntSushi on 2024-04-29 18:03_

---

_Branch deleted on 2024-04-29 18:03_

---
