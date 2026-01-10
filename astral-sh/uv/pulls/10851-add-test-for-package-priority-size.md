```yaml
number: 10851
title: Add test for package priority size
type: pull_request
state: closed
author: konstin
labels:
  - internal
assignees: []
draft: true
base: main
head: konsti/assert-package-priority-size
created_at: 2025-01-22T11:13:03Z
updated_at: 2025-02-19T10:33:43Z
url: https://github.com/astral-sh/uv/pull/10851
synced_at: 2026-01-10T11:10:34Z
```

# Add test for package priority size

---

_Pull request opened by @konstin on 2025-01-22 11:13_

See also #10833

---

_Label `internal` added by @konstin on 2025-01-22 11:13_

---

_Comment by @konstin on 2025-01-22 11:46_

This is an optimization that rustc misses, the type below stores the same data and is 16 bytes:

```rust
    #[derive(Debug, Clone, Copy, PartialEq, Eq, PartialOrd, Ord)]
    pub(crate) enum PubGrubPriority {
        /// The package has no specific priority.
        ///
        /// As such, its priority is based on the order in which the packages were added (FIFO), such
        /// that the first package we visit is prioritized over subsequent packages.
        ///
        /// TODO(charlie): Prefer constrained over unconstrained packages, if they're at the same depth
        /// in the dependency graph.
        Unspecified(Reverse<usize>, u32),

        /// Selected version of this package were often the culprit of rejecting another package, so
        /// it's deprioritized behind `ConflictEarly`. It's still the higher than `Unspecified` to
        /// conflict before selecting unrelated packages.
        ConflictLate(Reverse<usize>, u32),

        /// Selected version of this package were often rejected, so it's prioritized over
        /// `ConflictLate`.
        ConflictEarly(Reverse<usize>, u32),

        /// The version range is constrained to a single version (e.g., with the `==` operator).
        Singleton(Reverse<usize>, u32),

        /// The package was specified via a direct URL.
        ///
        /// N.B.: URLs need to have priority over registry distributions for correctly matching registry
        /// distributions to URLs, see [`PubGrubPackage::from_package`] an
        /// [`ForkUrls`].
        DirectUrl(Reverse<usize>, u32),

        /// The package is the root package.
        Root,
    }
```

---

_Closed by @konstin on 2025-02-19 10:33_

---
