```yaml
number: 3493
title: "Allow `# ruff:` prefix for isort action comments"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ruff-isort
created_at: 2023-03-13T21:38:03Z
updated_at: 2023-03-14T18:34:31Z
url: https://github.com/astral-sh/ruff/pull/3493
synced_at: 2026-01-12T04:39:45Z
```

# Allow `# ruff:` prefix for isort action comments

---

_Pull request opened by @charliermarsh on 2023-03-13 21:38_

This PR lets users write `# ruff: isort: skip_file` in lieu of `# isort: skip_file` -- similar to how we support `# ruff: noqa: F401` in lieu of `# flake8: noqa: F401`.

This option is admittedly more verbose, but it's clearer that they're intended for Ruff, that the project is using Ruff, etc.

Closes #3450.


---

_Review requested from @konstin by @charliermarsh on 2023-03-13 21:38_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-13 21:38_

---

_Comment by @github-actions[bot] on 2023-03-13 21:49_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/directives.rs`:110 on 2023-03-14 08:35_

Nit

```suggestion
        if matches!(comment_text, "# isort: split" | "# ruff: isort: split") {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/directives.rs`:116 on 2023-03-14 08:38_

Nit

```suggestion
        } else if matches!(comment_text, "# isort: skip_file" | "# isort:skip_file" | "# ruff: isort: skip_file" | "# ruff: isort:skip_file")
        {
```

or use a `match` statement 

```rust
match comment_text {
	"# isort: split" | "# ruff: isort: split" => { ... },
	"# isort: skip_file" | "# isort:skip_file" | " #ruff: isort: skip_file" | ... => {},
	_ if off.is_some() => {...
}
```

---

_Review comment by @MichaReiser on `crates/ruff/src/directives.rs`:109 on 2023-03-14 08:39_

It could help performance if you first test if the comment starts with either `# isort` or `ruff: isort` and, if not, bail out early. That ensures that we short-circuit for the majority of comments.

We could even go as far as doing, to reduce the casing necessary

```rust
    if let Some(isort_comment) = comment_text.split_once(["# isort:", "# ruff: isort:"]) {
        match isort_comment.trim_start() {
            "split" => {},
            "skip_file" => {}
        }
    }
```


---

_Review comment by @MichaReiser on `crates/ruff/resources/test/fixtures/isort/skip.py`:9 on 2023-03-14 08:41_

Can we add a test for `# ruff: isort:skip_file`

---

_@MichaReiser approved on 2023-03-14 08:41_

I find the "double" colons a bit confusing but it makes sense to use the same format since we already have precedence. 

Rome uses the `rome linter:` format to avoid having two colons [docs](https://docs.rome.tools/linter/#ignoring-code) which I find nice (also supports rome linter/rule-name). 

---

_@konstin approved on 2023-03-14 10:20_

---

_Merged by @charliermarsh on 2023-03-14 18:34_

---

_Closed by @charliermarsh on 2023-03-14 18:34_

---

_Branch deleted on 2023-03-14 18:34_

---
