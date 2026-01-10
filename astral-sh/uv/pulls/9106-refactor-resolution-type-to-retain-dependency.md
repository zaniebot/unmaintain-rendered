```yaml
number: 9106
title: "Refactor `Resolution` type to retain dependency graph"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/graph
created_at: 2024-11-14T00:16:01Z
updated_at: 2024-11-14T20:25:36Z
url: https://github.com/astral-sh/uv/pull/9106
synced_at: 2026-01-10T12:00:00Z
```

# Refactor `Resolution` type to retain dependency graph

---

_Pull request opened by @charliermarsh on 2024-11-14 00:16_

## Summary

This PR should not contain any user-visible changes, but the goal is to refactor the `Resolution` type to retain a dependency graph. We want to be able to explain _why_ a given package was excluded on error (see: https://github.com/astral-sh/uv/issues/8962), which in turn requires that at install time, we can go back and figure out the dependency chain. At present, `Resolution` is just a map from package name to distribution; this PR remodels it as a graph in which each node is a package, and the edges contain markers plus extras or dependency groups.

---

_Label `internal` added by @charliermarsh on 2024-11-14 00:16_

---

_Marked ready for review by @charliermarsh on 2024-11-14 00:16_

---

_@charliermarsh reviewed on 2024-11-14 05:19_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/target.rs`:160 on 2024-11-14 05:19_

I'm pretty unhappy with basically this whole method. It's just a bit tortured to achieve the representations we want (e.g., we have to retain nodes even though we won't install them).

---

_Review requested from @BurntSushi by @charliermarsh on 2024-11-14 05:19_

---

_Review requested from @konstin by @charliermarsh on 2024-11-14 05:19_

---

_Review comment by @BurntSushi on `crates/uv-distribution-types/src/resolution.rs`:177 on 2024-11-14 13:26_

Is "single platform" correct here? I _think_ it might be, "subset of all marker environments" instead.

Although, this is also for a `Node`... Should this be moved to the docs for `Resolution`?

---

_Review comment by @BurntSushi on `crates/uv-distribution-types/src/resolution.rs`:197 on 2024-11-14 13:26_

Nice

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock/target.rs`:197 on 2024-11-14 13:30_

What was hard about turning this into a helper method instead of a macro? It's not immediately obvious to me.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolution/output.rs`:817 on 2024-11-14 13:32_

I think `s/ResolutionGraph/ResolverOutput`?

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolution/output.rs`:823 on 2024-11-14 13:32_

Very helpful! Thank you!

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolution/output.rs`:826 on 2024-11-14 13:34_

Same "single platform" phrasing here... should it also be "subset of all marker environments"? But the last "shouldn't be called" part suggests maybe not...

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolution/output.rs`:826 on 2024-11-14 13:36_

And if "single platform" is right, should we have an assertion like `assert!(output.fork_markers.is_empty())` to enforce this?

---

_Review comment by @konstin on `crates/uv-distribution-types/src/resolution.rs`:13 on 2024-11-14 13:36_

Nit: `Node, Edge, Directed` sounds like they are generic types from petgraph itself.

---

_@BurntSushi approved on 2024-11-14 13:37_

This overall LGTM.

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/target.rs`:160 on 2024-11-14 13:48_

What about replacing the macro with a method? https://gist.github.com/konstin/da92fa38a8840050b8b2dedfefaef559

```rust
    /// Convert a lockfile entry to an installable distribution.
    fn package_to_node(
        &self,
        package: &Package,
        tags: &Tags,
        build_options: &BuildOptions,
        install_options: &InstallOptions,
    ) -> Result<Node, LockError> {
        if install_options.include_package(
            package.name(),
            self.project_name(),
            self.lock().members(),
        ) {
            let dist = package.to_dist(
                self.workspace().install_path(),
                TagPolicy::Required(tags),
                build_options,
            )?;
            let version = package.version().clone();
            let dist = ResolvedDist::Installable { dist, version };
            let hashes = package.hashes();
            Ok(Node::Dist {
                dist,
                hashes,
                install: true,
            })
        } else {
            let dist = package.to_dist(
                self.workspace().install_path(),
                TagPolicy::Preferred(tags),
                &BuildOptions::default(),
            )?;
            let version = package.version().clone();
            let dist = ResolvedDist::Installable { dist, version };
            let hashes = package.hashes();
            Ok(Node::Dist {
                dist,
                hashes,
                install: false,
            })
        }
    }
```



---

_Review comment by @konstin on `crates/uv-resolver/src/lock/target.rs`:160 on 2024-11-14 13:51_

tbh i kinda like having the full graph, it makes reasoning easier.

Another option here would be to have the `install` outline as a mask, e.g. a hashset of node indexes that represent the installed subset; haven't checked if that actually helps though.

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/target.rs`:228 on 2024-11-14 13:54_

I really that we don't have orphan nodes anymore, but aren't those a bit different in that they aren't mandatory, but depend on the user selection? (This may just be a documentation issue)

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/target.rs`:250 on 2024-11-14 13:55_

nit: `if !... { continue; }` and dedent the rest

---

_@konstin approved on 2024-11-14 13:57_

---

_@charliermarsh reviewed on 2024-11-14 19:57_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/target.rs`:160 on 2024-11-14 19:57_

Good call.

---

_Comment by @charliermarsh on 2024-11-14 20:07_

Thank you both!

---

_Merged by @charliermarsh on 2024-11-14 20:25_

---

_Closed by @charliermarsh on 2024-11-14 20:25_

---

_Branch deleted on 2024-11-14 20:25_

---
