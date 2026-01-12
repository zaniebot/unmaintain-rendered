```yaml
number: 11606
title: "Allow dependency groups to define a separate `requires-python`"
type: issue
state: closed
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-02-18T22:40:16Z
updated_at: 2025-06-13T22:04:14Z
url: https://github.com/astral-sh/uv/issues/11606
synced_at: 2026-01-12T16:00:41Z
```

# Allow dependency groups to define a separate `requires-python`

---

_@zanieb_

### Summary

Right now, the `[dependency-groups]` have the same `requires-python` as the `[project]` defines, but this causes problems when a group is only intended to be used with a different Python version range, e.g., you could only care about 3.12+ for building your documentation but 3.9+ matters for publishing your library.

See

- #11605

As a meta-note, I know this was discussed elsewhere but cannot find it.

### Example

```toml
[project]
name = "example"
version = "0.0.0"
requires-python = ">=3.10"

[tool.uv.dependency-groups.foo]
requires-python = ">=3.12"
```

---

_Label `enhancement` added by @zanieb on 2025-02-18 22:40_

---

_Comment by @konstin on 2025-02-19 10:11_

To add a motivating example, users may have tooling such as building documentation, code generation or importing external data that requires a more recent Python version than the project itself. A common example is sphinx, which (as of master) requires Python 3.11, while libraries supporting all non-EOL Python versions support `>=3.9`. The current workaround is `sphinx; python_version >= "3.11"` in the dependency group, which allows selecting the group on an older Python version without actually installing sphinx.

---

_Comment by @charliermarsh on 2025-03-19 01:55_

I think this is fairly straightforward to implement by adding it to `lowering.rs` (just put `python_version >= "3.12"` on each dependency; the resolver will do the rest). However, we probably also want to error (?) if users try to install these groups with a lower Python version? So we'll need to track it elsewhere too, and add error handling around it.


---

_Comment by @zanieb on 2025-03-19 02:02_

I see this as fairly high priority given the workaround is to add a marker to every dependency in the group.

@Gankra perhaps you'd be interested in picking it up? (I know you _love_ dependency groups)

---

_Comment by @charliermarsh on 2025-03-19 02:05_

(The only weirdness with the approach I described above is we'll then add `python_version >= "3.12"` to each of the dependency entries in the lockfile _and_ we'd have some annotation on the group itself, which is kind of ugly.)

---

_Assigned to @Gankra by @Gankra on 2025-03-19 12:46_

---

_Comment by @Gankra on 2025-03-19 12:46_

Sure I can give this a shot

---

_Comment by @a-reich on 2025-06-09 00:00_

I’d love this feature too, it’s [analogous](https://discuss.python.org/t/dependency-groups-extras-and-how-to-manage-environments-python-versions/69297) to one hatch has (for what it terms environments).

---

_Comment by @RobertCraigie on 2025-06-10 11:25_

We're running into this as well, as all of our libraries still support Python 3.8, we can't add an optional dependency that requires 3.9. From our perspective it's totally fine if users can only use some optional dependencies if they're on later Python versions.

Is there any workaround possible until this is fully supported?

---

_Comment by @charliermarsh on 2025-06-10 11:48_

Yeah the existing workaround is to add `; python_version >= "3.9"` to each such dependency.

---

_Comment by @RobertCraigie on 2025-06-10 12:11_

ah thank you! sorry I missed that earlier in this thread.

---

_Closed by @Gankra on 2025-06-13 22:04_

---
