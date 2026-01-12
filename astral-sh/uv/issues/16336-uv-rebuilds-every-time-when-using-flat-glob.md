```yaml
number: 16336
title: "uv rebuilds every time when using flat glob pattern `*.{h,c,hpp,cpp}` in `cache-keys`"
type: issue
state: open
author: jgauth
labels:
  - needs-decision
assignees: []
created_at: 2025-10-17T05:37:23Z
updated_at: 2025-10-30T18:09:48Z
url: https://github.com/astral-sh/uv/issues/16336
synced_at: 2026-01-12T16:02:29Z
```

# uv rebuilds every time when using flat glob pattern `*.{h,c,hpp,cpp}` in `cache-keys`

---

_@jgauth_

### Summary

When using the `cache-keys` field with a flat source layout, uv always triggers a rebuild, even when none of the relevant files have changed.

By default, (e.g. with `uv init --lib --build-backend=scikit .`), uv creates a `src/` dir, and includes in the `cache-keys`  `{ file = "src/**/*.{h,c,hpp,cpp}" }`. This works as you would expect.

If instead you have no nesting - source files flat in the top-level directory, and set `cache-keys` to `{ file = "*.{h,c,hpp,cpp}" }`, uv will trigger a rebuild every time, even if no files have changed.

### Minimal Reproducible Example
See https://github.com/jgauth/uv-scikit-flat

Clone, run `uv run python` repeatedly. Observe that `uv` rebuilds the packages every time.

### Platform

Ubuntu 22.04.5 LTS Linux 5.15.0-156-generic x86_64 GNU/Linux

### Version

uv 0.9.3

### Python version

Python 3.11.8

---

_Label `bug` added by @jgauth on 2025-10-17 05:37_

---

_Comment by @konstin on 2025-10-17 09:02_

The bug is that we traverse into subdirectories with `*` (without `**`) and ignore `./`, so we a build file as modified: `build/cp311-cp311-linux_x86_64/CMakeFiles/3.28.3/CompilerIdCXX/CMakeCXXCompilerId.cpp`.

CC @BurntSushi for globs.

---

_Comment by @jgauth on 2025-10-17 22:24_

Thanks. In the meantime, I can work around this by using more specific patterns:
`file = "myprefix_*.{h,cpp}"`

---

_Comment by @BurntSushi on 2025-10-28 13:40_

@konstin Can you say more? Is there a specific bug with glob matching that you're referring to? (I'm not sure what the `+` is referring to.)

---

_Comment by @konstin on 2025-10-28 13:41_

Wrong sigil, sorry, I meant with `+`.

I'm curious about your thoughts on our current behavior that `"*.{h,c,hpp,cpp}"` includes all files and what we might want to change about this.

---

_Comment by @BurntSushi on 2025-10-28 13:53_

Oh I see. Yeah, that is consistent with gitignore semantics. And I kind of think it makes intuitive sense. Users should be able to do `/*.{h,c,hpp,cpp}` to anchor it to the current working directory. @jgauth Does that work for you?

---

_Label `bug` removed by @charliermarsh on 2025-10-28 23:54_

---

_Label `question` added by @charliermarsh on 2025-10-28 23:54_

---

_Comment by @charliermarsh on 2025-10-28 23:54_

(Marking as a question.)

---

_Comment by @konstin on 2025-10-29 11:44_

`/*.{h,c,hpp,cpp}` doesn't seem to work, we start recursing through the filesystem root:

```
WARN Failed to read glob entry: IO error for operation on /proc/259871/fd: Permission denied (os error 13)
WARN Failed to read glob entry: IO error for operation on /proc/259871/map_files: Permission denied (os error 13)
WARN Failed to read glob entry: IO error for operation on /proc/259871/fdinfo: Permission denied (os error 13)
WARN Failed to read glob entry: IO error for operation on /proc/259871/ns: Permission denied (os error 13)
WARN Failed to read glob entry: IO error for operation on /root: Permission denied (os error 13)
```

---

_Comment by @BurntSushi on 2025-10-29 12:30_

Oh interesting, I guess we are traversing based on the path in the glob itself. Then yeah, I think the way we currently have things setup, users can't write globs that only match the top level directory.

> I'm curious about your thoughts on our current behavior that `"*.{h,c,hpp,cpp}"` includes all files and what we might want to change about this.

We could look at [enabling `literal_separator`](https://docs.rs/globset/latest/globset/struct.GlobBuilder.html#method.literal_separator), so that the only thing that can match a `/` is `/` or `**`. I just don't know how much people are relying on, e.g., `*.c`, to match _anywhere_ in the directory tree.

---

_Comment by @zanieb on 2025-10-29 14:12_

I think if someone provides a path starting with `/` we should root that in the CWD instead of going to actual `/`?

---

_Comment by @BurntSushi on 2025-10-29 14:25_

I think that makes sense yeah.

---

_Label `question` removed by @konstin on 2025-10-29 14:26_

---

_Label `needs-design` added by @konstin on 2025-10-29 14:26_

---

_Label `needs-design` removed by @konstin on 2025-10-29 14:26_

---

_Label `needs-decision` added by @konstin on 2025-10-29 14:26_

---

_Comment by @konstin on 2025-10-29 14:31_

We have a `*`/`**` distinction in other globs, and also support absolute paths in most fields (or parse a leading `/` and say absolute paths are forbidden), so I'd prefer the `literal_separator` behavior. I'd be surprised if a lot of people depend on it, we always use `**/*.{ext}` in our example, at least I was unaware of our current behavior. Still, I think it's a breaking change with either change?

---
