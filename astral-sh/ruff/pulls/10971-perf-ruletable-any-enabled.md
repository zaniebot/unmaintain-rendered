```yaml
number: 10971
title: "perf: `RuleTable::any_enabled`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
assignees: []
merged: true
base: main
head: perf-rules-any-enabled
created_at: 2024-04-16T08:40:52Z
updated_at: 2024-04-16T13:18:32Z
url: https://github.com/astral-sh/ruff/pull/10971
synced_at: 2026-01-12T15:55:34Z
```

# perf: `RuleTable::any_enabled`

---

_@MichaReiser_

## Summary

This PR improves the performance of `RuleTable::any_enabled` which is called frequently in expression checking to determine if a specific set of rules is enabled. 

The old implementation used `enabled.intersects(RuleSet::from_iter(rules))` to test if the enabled set and the tested rules overlap. 
This worked fine when we had few rules but is now becoming a performance bottleneck when bumping `RuleSet` from 13 to 14 usizes because each call zero initializes a 14 * 8=112 bytes large array on the stack, sets the rule indexes and then computes if the sets overlap. 

The new implementation avoids constructing a `RuleSet` using `from_iter` based on the assumption that we mainly query `any_enabled` with a few rules. This avoids writing 112 bytes on each call.

This should make the `any_enabled` check independent of the size of the `RuleSet`. 

## Test Plan

`cargo test`


---

_Label `performance` added by @MichaReiser on 2024-04-16 08:44_

---

_Comment by @codspeed-hq[bot] on 2024-04-16 08:46_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/perf-rules-any-enabled)

### Merging #10971 will **improve performances by 20.14%**

<sub>Comparing <code>perf-rules-any-enabled</code> (bac6d21) with <code>main</code> (f779bab)</sub>



### Summary

`⚡ 6` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `perf-rules-any-enabled` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[large/dataset.py]` | 82.4 ms | 79 ms | +4.32% |
| ⚡ | `linter/default-rules[large/dataset.py]` | 19.3 ms | 16.1 ms | +20.14% |
| ⚡ | `linter/default-rules[numpy/ctypeslib.py]` | 4.1 ms | 3.7 ms | +13.11% |
| ⚡ | `linter/default-rules[numpy/globals.py]` | 636.8 µs | 595.2 µs | +6.98% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 8.6 ms | 7.7 ms | +11.52% |
| ⚡ | `linter/default-rules[unicode/pypinyin.py]` | 1.5 ms | 1.3 ms | +16.67% |


---

_Assigned to @BurntSushi by @MichaReiser on 2024-04-16 08:46_

---

_Comment by @github-actions[bot] on 2024-04-16 08:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-04-16 09:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/registry/rule_set.rs`:254 on 2024-04-16 09:02_

I intentionally use an bitwise OR here to avoid any branching. 

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/registry/rule_set.rs`:254 on 2024-04-16 11:45_

I'd bet it would compile down to the same code if you used an `if`. :-) But I actually find this pretty readable as it is personally.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/registry/rule_set.rs`:253 on 2024-04-16 11:46_

It looks like this entire function could just be written as `rules.iter().any(|r| self.contains(r))`? Did you try that and it was slower? (I believe it also has the benefit of short circuiting, which may or may not help depending on the typical length of `rules`.)

---

_@BurntSushi reviewed on 2024-04-16 11:47_

---

_@MichaReiser reviewed on 2024-04-16 11:55_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/registry/rule_set.rs`:253 on 2024-04-16 11:55_

The typical length is like 1-8 rules (where 8 is rare). The downside is that the function can't be const anymore ;). 

But the performance is about the same. So lets use `any` as it is easier to understand.

Edit: Codspeed disagrees. The shift version is slightly faster (23% speedup instead of 20%)

---

_@MichaReiser reviewed on 2024-04-16 12:06_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/registry/rule_set.rs`:254 on 2024-04-16 12:06_

Acutally, it does not

The iter version has a jump

```
mov     rax, qword ptr [rsp - 112]
movzx   edx, byte ptr [rsp - 116]
mov     cl, 1
bt      rax, rdx
jb      .LBB0_2
movzx   ecx, byte ptr [rsp - 114]
bt      rax, rcx
setb    cl
```

The loop version does not (but it requires more instructions)

```
mov     byte ptr [rsp - 117], cl
lea     rax, [rsp - 117]
movzx   ecx, byte ptr [rsp - 114]
mov     edx, 1
shl     edx, cl
movzx   ecx, byte ptr [rsp - 116]
bts     edx, ecx
test    dword ptr [rsp - 112], edx
setne   byte ptr [rsp - 117]
```
https://godbolt.org/z/oWWzqc6s1


---

_Merged by @MichaReiser on 2024-04-16 12:20_

---

_Closed by @MichaReiser on 2024-04-16 12:20_

---

_Branch deleted on 2024-04-16 12:20_

---

_@BurntSushi reviewed on 2024-04-16 12:24_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/registry/rule_set.rs`:254 on 2024-04-16 12:24_

Yeah I just meant, `if self.contains(rules[i]) { any = true }`.

It makes sense that the `iter` version has an extra jump because of the short circuit.

---

_@BurntSushi reviewed on 2024-04-16 12:24_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/registry/rule_set.rs`:253 on 2024-04-16 12:24_

Ah I missed the `const` requirement.

---

_@MichaReiser reviewed on 2024-04-16 12:27_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/registry/rule_set.rs`:254 on 2024-04-16 12:27_

ah yeah, that probably compiles down to the same

---

_Comment by @zanieb on 2024-04-16 13:18_

Woo!

---
