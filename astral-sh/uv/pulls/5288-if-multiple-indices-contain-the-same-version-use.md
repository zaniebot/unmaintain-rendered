```yaml
number: 5288
title: If multiple indices contain the same version, use the first index
type: pull_request
state: merged
author: yorickvP
labels:
  - bug
assignees: []
merged: true
base: main
head: yorickvp/fix-multi-index
created_at: 2024-07-22T14:08:43Z
updated_at: 2024-07-22T18:03:36Z
url: https://github.com/astral-sh/uv/pull/5288
synced_at: 2026-01-12T16:06:44Z
```

# If multiple indices contain the same version, use the first index

---

_@yorickvP_

This fixes resolving packages that publish an invalid stub to pypi, such as tensorrt-llm.

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

In https://github.com/astral-sh/uv/pull/3138 , we implemented `unsafe-best-match`. However, it seems to not quite work as expected. When multiple indices contain the same version, it's not clear which index the current code uses. This PR fixes that to use the first index the package is in.

## Test Plan

```console
$ echo 'tensorrt-llm==0.11.0' | ./target/debug/uv pip compile - --extra-index-url https://pypi.nvidia.com --python-version=3.10 --index-strategy=unsafe-best-match --annotation-style=line
```


cc: @charliermarsh 


---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-22 14:14_

---

_Label `bug` added by @charliermarsh on 2024-07-22 14:14_

---

_Comment by @charliermarsh on 2024-07-22 16:04_

Yeah interesting, contrary to my expectation `kmerge` isn't "stable" by iterator:

```rust
#[cfg(test)]
mod tests {
    use itertools::Itertools;

    #[test]
    fn kmerge_reverse() {
        let a = vec![('a', 1), ('b', 2), ('c', 3)];
        let b = vec![('d', 1), ('e', 2), ('f', 3)];

        let its = [a, b];
        let its = its.iter().kmerge_by(|a, b| a.1 < b.1);

        let actual = its.collect::<Vec<_>>();
        let expected = vec![&('a', 1), &('d', 1), &('b', 2), &('e', 2), &('c', 3), &('f', 3)];

        // This fails; left is: `[('a', 1), ('d', 1), ('e', 2), ('b', 2), ('c', 3), ('f', 3)]`
        assert_eq!(actual, expected);
    }
}
```

I didn't have any basis for expecting that so I don't think it's a bug, I just assumed it was true.

---

_Comment by @charliermarsh on 2024-07-22 16:16_

@yorickvP - I tweaked to avoid re-comparing. Can you test that it's still passing on your test case?

---

_Review requested from @charliermarsh by @charliermarsh on 2024-07-22 16:31_

---

_Comment by @yorickvP on 2024-07-22 17:52_

Still works!

---

_Merged by @charliermarsh on 2024-07-22 18:03_

---

_Closed by @charliermarsh on 2024-07-22 18:03_

---

_Comment by @charliermarsh on 2024-07-22 18:03_

Thanks!

---
