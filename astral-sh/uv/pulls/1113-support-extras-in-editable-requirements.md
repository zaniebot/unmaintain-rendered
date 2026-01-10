```yaml
number: 1113
title: Support extras in editable requirements
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/editable-extra
created_at: 2024-01-26T00:17:42Z
updated_at: 2024-01-26T12:34:50Z
url: https://github.com/astral-sh/uv/pull/1113
synced_at: 2026-01-10T15:39:03Z
```

# Support extras in editable requirements

---

_Pull request opened by @charliermarsh on 2024-01-26 00:17_

## Summary

This PR adds support for requirements like `-e .[d]`.

Closes #1091.


---

_Review requested from @zanieb by @charliermarsh on 2024-01-26 00:19_

---

_Review requested from @konstin by @charliermarsh on 2024-01-26 00:19_

---

_Converted to draft by @charliermarsh on 2024-01-26 00:22_

---

_Comment by @charliermarsh on 2024-01-26 00:22_

Draft while I fix tests.

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolution.rs`:168 on 2024-01-26 01:07_

This was an existing bug, we were dropping dependencies here in the annotation graph.

---

_@charliermarsh reviewed on 2024-01-26 01:07_

---

_Marked ready for review by @charliermarsh on 2024-01-26 01:10_

---

_@charliermarsh reviewed on 2024-01-26 01:11_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/tests/resolver.rs`:188 on 2024-01-26 01:11_

(An example of a dependency we lost, now fixed.)

---

_Comment by @charliermarsh on 2024-01-26 01:11_

Ok, tests passing.

---

_Review comment by @konstin on `crates/requirements-txt/src/lib.rs`:122 on 2024-01-26 08:38_

```suggestion
                    Pep508ErrorSource::String(_) | Pep508ErrorSource::UrlError(_) => RequirementsTxtParserError::Pep508 {
                        start: err.start,
                        end: err.start + err.len,
                        source: err,
                    },
```

---

_Review comment by @konstin on `crates/requirements-txt/src/lib.rs`:173 on 2024-01-26 08:48_

I find it too easy to mess up manual chars iteration (and harder too read) and the perf doesn't matter here.

```rust
    let Some((path, extras)) = given
        .strip_suffix("]")
        .and_then(|given| given.rsplit_once("["))
    else {
        return None;
    };
    if extras.contains("]") {
        return None;
    }
    Some((paths, extras))
```

---

_@konstin approved on 2024-01-26 08:51_

---

_@charliermarsh reviewed on 2024-01-26 11:55_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:173 on 2024-01-26 11:55_

Ironically that's actually not quite correct because it omits the braces on `extras` which need to be retained :joy:

---

_@charliermarsh reviewed on 2024-01-26 11:58_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:173 on 2024-01-26 11:58_

I could do it with `rfind` but I don't know that it's that much simpler.

---

_@charliermarsh reviewed on 2024-01-26 12:00_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:173 on 2024-01-26 12:00_

I think it would be this:
```rust
if !given.ends_with(']') {
    return None;
}
let index = given.rfind('[')?;
let (requirement, extras) = given.split_at(index);
if extras[..extras.len() - 1].contains(']') {
    return None;
}
Some((requirement, extras))
```

---

_Merged by @charliermarsh on 2024-01-26 12:07_

---

_Closed by @charliermarsh on 2024-01-26 12:07_

---

_Branch deleted on 2024-01-26 12:07_

---

_@konstin reviewed on 2024-01-26 12:34_

---

_Review comment by @konstin on `crates/requirements-txt/src/lib.rs`:173 on 2024-01-26 12:34_

uh yeah i see why you went with char iterator

---
