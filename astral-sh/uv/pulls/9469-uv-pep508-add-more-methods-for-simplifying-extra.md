```yaml
number: 9469
title: "uv-pep508: add more methods for simplifying `extra`-related expressions"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/pep508-additions
created_at: 2024-11-27T13:35:45Z
updated_at: 2024-12-02T14:09:37Z
url: https://github.com/astral-sh/uv/pull/9469
synced_at: 2026-01-10T12:00:00Z
```

# uv-pep508: add more methods for simplifying `extra`-related expressions

---

_Pull request opened by @BurntSushi on 2024-11-27 13:35_

In the course of working on #9289, I've had to devise some additions to
our markers. While we are still staying strictly compatible with the
PEP 508 format, we will be abusing the `extra` expression to carry a
lot more information.

Specifically, we want the following additional operations:

* Simplify `extra != 'foo'`
* Remove all extra expressions
* Remove everything except extra expressions

My work on #9289 requires all of these (which will be in a future in
PR).

This PR also allows the `if_not_else` Clippy lint. Specifically, the
lint was flagging this code:

```rust
        if !matches!(node.var, Variable::Extra(_)) {
            i = NodeId::FALSE;
            for child in node.children.nodes() {
                i = self.or(i, child.negate(parent));
            }
            if i.is_true() {
                return NodeId::TRUE;
            }
            self.only_extras(i)
        } else {
            // Restrict all nodes recursively.
            let children = node.children.map(i, |node| self.only_extras(node));
            self.create_node(node.var.clone(), children)
        }
```

It wants you to flip the `if` and `else` bodies and un-negate the
condition. But I wanted to keep it as-written because I find it easier
to read. And I think this applies more generally. I think there are
*some* cases where un-negating a condition can be easier to read (in
particular, double negatives), but I don't think it applies universally
to the point where we should lint against it.


---

_Review requested from @ibraheemdev by @BurntSushi on 2024-11-27 13:35_

---

_Review requested from @konstin by @BurntSushi on 2024-11-27 13:35_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/algebra.rs`:447 on 2024-11-27 17:22_

I'm trying to grok what this function does. What would it do with `((os_name == ... and extra == foo) or (sys_platform == ... and extra != foo))`?

---

_@konstin reviewed on 2024-11-27 17:31_

---

_@BurntSushi reviewed on 2024-12-02 13:15_

---

_Review comment by @BurntSushi on `crates/uv-pep508/src/marker/algebra.rs`:447 on 2024-12-02 13:15_

It would give you `os_name == ... or sys_platform == ...` back. Basically, it removes all `extra` nodes by assuming they are `true`.

---

_@BurntSushi reviewed on 2024-12-02 13:15_

---

_Review comment by @BurntSushi on `crates/uv-pep508/src/marker/algebra.rs`:447 on 2024-12-02 13:15_

I added this as an example to the docs of this function.

---

_Merged by @BurntSushi on 2024-12-02 14:09_

---

_Closed by @BurntSushi on 2024-12-02 14:09_

---

_Branch deleted on 2024-12-02 14:09_

---
