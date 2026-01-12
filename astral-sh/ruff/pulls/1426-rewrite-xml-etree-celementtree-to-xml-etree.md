```yaml
number: 1426
title: Rewrite xml.etree.cElementTree to xml.etree.ElementTree
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: cElementTree
created_at: 2022-12-28T15:58:29Z
updated_at: 2022-12-28T21:30:36Z
url: https://github.com/astral-sh/ruff/pull/1426
synced_at: 2026-01-12T15:55:06Z
```

# Rewrite xml.etree.cElementTree to xml.etree.ElementTree

---

_@colin99d_

Another check for #827 

---

_Marked ready for review by @colin99d on 2022-12-28 15:58_

---

_Review comment by @squiddy on `src/pyupgrade/plugins/rewrite_c_element_tree.rs`:25 on 2022-12-28 16:12_

I suppose it's a matter of preference, but both match arms, under certain conditions, assign exactly the same values to `range` and `selected_contents`.

What do you think of this (didn't test it)?

```rust
    if match &stmt.node {
        StmtKind::Import { names } => {
            let item = names.get(0).unwrap();
            item.node.name == "xml.etree.cElementTree" && item.node.asname.is_some()
        }
        StmtKind::ImportFrom { module, .. } => {
            module == &Some("xml.etree.cElementTree".to_string())
        }
        _ => false,
    } {
        let range = Range::from_located(stmt);
        let mut check = Check::new(CheckKind::RewriteCElementTree, &range);
        if checker.patch(check.kind.code()) {
            let content = checker.locator.slice_source_code_range(&range).to_string();
            check.amend(Fix::replacement(
                content.replace("cElementTree", "ElementTree"),
                stmt.location,
                stmt.end_location.unwrap(),
            ));
        }
        checker.add_check(check);
    }
```

---

_@squiddy reviewed on 2022-12-28 16:12_

---

_Review comment by @colin99d on `src/pyupgrade/plugins/rewrite_c_element_tree.rs`:25 on 2022-12-28 19:13_

I agree, this is much better.

---

_@colin99d reviewed on 2022-12-28 19:13_

---

_Comment by @colin99d on 2022-12-28 19:24_

@squiddy good catch with adding in all tests, I found a couple issues in this PR. Everything is fixed now.

---

_Merged by @charliermarsh on 2022-12-28 21:30_

---

_Closed by @charliermarsh on 2022-12-28 21:30_

---
