```yaml
number: 2471
title: "Don't walk past project root when figuring out exclusion"
type: pull_request
state: merged
author: akx
labels: []
assignees: []
merged: true
base: main
head: project-root
created_at: 2023-02-02T12:25:22Z
updated_at: 2023-02-03T13:23:51Z
url: https://github.com/astral-sh/ruff/pull/2471
synced_at: 2026-01-12T15:55:08Z
```

# Don't walk past project root when figuring out exclusion

---

_@akx_

Refs https://github.com/charliermarsh/ruff/issues/2034

---

_Review comment by @charliermarsh on `src/resolver.rs`:399 on 2023-02-02 13:36_

I think something like this could work, but isn't this condition always true?

---

_@charliermarsh reviewed on 2023-02-02 13:36_

---

_Review comment by @charliermarsh on `src/resolver.rs`:399 on 2023-02-02 13:38_

I think we may want this, after the `match`:

```rust
        if path == settings.project_root {
            // Bail out if we've walked past the project root
            // (so excludes etc. are virtually "rooted" to the project).
            break;
        }
```

---

_@charliermarsh reviewed on 2023-02-02 13:38_

---

_@akx reviewed on 2023-02-02 14:36_

---

_Review comment by @akx on `src/resolver.rs`:399 on 2023-02-02 14:36_

Nope, it's not always true – `for path in path.ancestors() {` above :)



---

_@charliermarsh reviewed on 2023-02-02 16:13_

---

_Review comment by @charliermarsh on `src/resolver.rs`:399 on 2023-02-02 16:13_

Sorry, I wasn't being clear!

I created a directory structure like this:

```console
build/
  foo/
    ruff.toml/
    bop.py
```

Where `bop.py` contained one unused import.

In this case, if I run `cargo run build/foo/bop.py --force-exclude` on this branch, `bop.py` is still excluded:

```console
❯ cargo run build/foo/bop.py --force-exclude
Checking "/Users/crmarsh/workspace/ruff/build/foo/bop.py" against "/Users/crmarsh/workspace/ruff/build/foo"
Checking "/Users/crmarsh/workspace/ruff/build/foo" against "/Users/crmarsh/workspace/ruff/build/foo"
Checking "/Users/crmarsh/workspace/ruff/build" against "/Users/crmarsh/workspace/ruff"
warning: No Python files found under the given path(s)
```

However, if I change this check to `path == &settings.project_root`:

```console
❯ cargo run build/foo/bop.py --force-exclude -n
Checking "/Users/crmarsh/workspace/ruff/build/foo/bop.py" against "/Users/crmarsh/workspace/ruff/build/foo"
Checking "/Users/crmarsh/workspace/ruff/build/foo" against "/Users/crmarsh/workspace/ruff/build/foo"
build/foo/bop.py:1:8: F401 `sys` imported but unused
Found 1 error.
1 potentially fixable with the --fix option.
```

Perhaps I'm misunderstanding what we're trying to prevent though!


---

_Converted to draft by @akx on 2023-02-02 17:40_

---

_@akx reviewed on 2023-02-03 13:22_

---

_Review comment by @akx on `src/resolver.rs`:399 on 2023-02-03 13:22_

Yep, you're right :)

Switched to this implementation + added a test that piggybacks on the existing directory structure...

---

_Marked ready for review by @akx on 2023-02-03 13:22_

---

_Review requested from @charliermarsh by @akx on 2023-02-03 13:22_

---

_Merged by @charliermarsh on 2023-02-03 13:23_

---

_Closed by @charliermarsh on 2023-02-03 13:23_

---
