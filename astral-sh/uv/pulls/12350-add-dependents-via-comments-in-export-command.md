```yaml
number: 12350
title: "Add dependents (\"via ...\" comments) in export command"
type: pull_request
state: merged
author: zmeir
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zmeir/export_like_pip_compile
created_at: 2025-03-20T22:25:36Z
updated_at: 2025-04-02T15:36:03Z
url: https://github.com/astral-sh/uv/pull/12350
synced_at: 2026-01-10T11:10:39Z
```

# Add dependents ("via ..." comments) in export command

---

_Pull request opened by @zmeir on 2025-03-20 22:25_

Adding dependency trace/parent comments ("via ...") to the export command output.  
This is a similar behavior to the pip compile output.

#### Note to the eager reviewer:
First of all - thanks!  
Secondly, this is still a very rough draft. These are the first lines of code I've ever written in Rust. This is still mostly an educational/fun exercise for myself. If opening a Draft PR is creating too much noise - I apologize and I will close it until it is ready.

## Summary

Resolves #7777

## Test Plan

- [X] manual command execution
- [x] update expected output in tests 


---

_Review requested from @charliermarsh by @zanieb on 2025-03-21 18:15_

---

_Renamed from "Add dependency parents ("via" comments) in export command" to "Add dependents ("via ..." comments) in export command" by @zmeir on 2025-03-23 21:52_

---

_@charliermarsh reviewed on 2025-03-26 20:17_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/requirements_txt.rs`:319 on 2025-03-26 20:17_

What is the purpose of this sort?

---

_@charliermarsh reviewed on 2025-03-26 20:18_

This looks pretty reasonable overall.

---

_@charliermarsh reviewed on 2025-03-27 12:58_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/requirements_txt.rs`:319 on 2025-03-27 12:58_

I think I would've expected us to sort by name rather than degree.

---

_Review comment by @zmeir on `crates/uv-resolver/src/lock/requirements_txt.rs`:319 on 2025-03-27 20:53_

The purpose is to put on top any dependents that have the root as a direct dependent. This being the current project.

This is before sorting:
<img width="385" alt="image" src="https://github.com/user-attachments/assets/61cfa161-bfc8-4f5d-adb4-cccc6f3d7e6d" />

And this is after:
<img width="385" alt="image" src="https://github.com/user-attachments/assets/b4939efd-ee13-4fe0-a815-08f55005f6fa" />

I will add a secondary sort by name

---

_@zmeir reviewed on 2025-03-27 20:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-27 21:14_

---

_Label `enhancement` added by @charliermarsh on 2025-03-27 21:14_

---

_@charliermarsh reviewed on 2025-03-27 21:14_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/requirements_txt.rs`:319 on 2025-03-27 21:14_

Personally I think I'd prefer to sort by name -- it's a little easier to parse visually since it's predictable.

---

_@zmeir reviewed on 2025-03-27 21:54_

---

_Review comment by @zmeir on `crates/uv-resolver/src/lock/requirements_txt.rs`:319 on 2025-03-27 21:54_

Do you mean just by name or first the project and then the rest of the dependents by name?

I tried to be consistent with the output of `uv pip compile`.

---

_Marked ready for review by @zmeir on 2025-03-27 21:56_

---

_Comment by @zmeir on 2025-03-27 21:59_

Thanks for your review @charliermarsh, I think this is just about ready.

I've updated the sorting as discussed [here](https://github.com/astral-sh/uv/pull/12350#discussion_r2017587628). If you would like me to change it please let me know.

I've also added a CLI option: `--no-annotate`, similarly to `uv pip compile`.

---

_@charliermarsh reviewed on 2025-03-28 13:54_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/requirements_txt.rs`:319 on 2025-03-28 13:54_

I was proposing sorting _just_ by the dependent name. Doesn't `uv pip compile` just sort by name?

```rust
let mut dependents = graph
    .edges_directed(index, Direction::Incoming)
    .map(|edge| &graph[edge.source()])
    .map(uv_distribution_types::Name::name)
    .collect::<Vec<_>>();
dependents.sort_unstable();
dependents.dedup();
dependents
```

---

_@charliermarsh approved on 2025-03-28 14:08_

---

_Merged by @charliermarsh on 2025-03-28 14:37_

---

_Closed by @charliermarsh on 2025-03-28 14:37_

---

_Comment by @eliasecchig on 2025-04-02 15:13_

can you consider changing the behavior so to no-annotate by default? 

This is breaking many frameworks depending on uv

---

_Comment by @charliermarsh on 2025-04-02 15:15_

Sorry, can you expand on that comment? What frameworks are depending on uv, and why are they breaking here? Comments are part of the output format.

---

_Comment by @eliasecchig on 2025-04-02 15:24_

The default output of a `uv export` command changed between `uv <= 0.6.10` and `uv >= 0.6.11`, representing a **backwards-incompatible change.**

I am using uv and the `export` command as part of a public project - see[ this line](https://github.com/GoogleCloudPlatform/agent-starter-pack/blob/main/src/base_template/deployment/cd/staging.yaml#L121C1-L122C1)

```bash
 uv export --no-hashes --no-sources --no-header --no-emit-project --frozen > .requirements.txt
```
**In short the same command:**
*   `uv <= 0.6.10`: Produces **Output X**
*   `uv >= 0.6.11`: Produces **Output Y**

Hence why my request is to ensure `--no-annotate` is the default option.

---

_Comment by @charliermarsh on 2025-04-02 15:28_

Right, but the output is still a valid `requirements.txt` file.

---

_Comment by @zanieb on 2025-04-02 15:29_

Sorry, but a change in the output is not considered a breaking change if the _format_ is unchanged. Here, we've just added comments that are compatible with the file format.

---

_Comment by @eliasecchig on 2025-04-02 15:36_

Understood, I will deal with it on my side thanks!

---
