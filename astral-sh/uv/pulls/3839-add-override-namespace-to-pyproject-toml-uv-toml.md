```yaml
number: 3839
title: Add override namespace to pyproject.toml/uv.toml
type: pull_request
state: merged
author: Di-Is
labels: []
assignees: []
merged: true
base: main
head: add-override-namaspace-to-toml
created_at: 2024-05-26T13:42:03Z
updated_at: 2024-07-19T06:29:51Z
url: https://github.com/astral-sh/uv/pull/3839
synced_at: 2026-01-12T16:05:53Z
```

# Add override namespace to pyproject.toml/uv.toml

---

_@Di-Is_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

See #3834 .

This PR adds a new namespace, `override-dependencies`, to pyproject.toml/uv.toml.
This namespace assumes that the dependencies you want to override are written in the form of `requirements.txt`.


a example of pyproject.toml
```toml
[project]
name = "example"
version = "0.0.0"
dependencies = [
  "flask==3.0.0"
]

[tool.uv]
override-dependencies = [
  "werkzeug==2.3.0"
]
```

This will improve usability by allowing you to override dependencies without having to specify the --override option when running `uv pip compile/install`.

## Test Plan

added test to `crates/uv/tests/pip_compile.rs`.


---

_Converted to draft by @Di-Is on 2024-05-26 13:50_

---

_Comment by @codspeed-hq[bot] on 2024-05-26 13:50_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/Di-Is:add-override-namaspace-to-toml)

### Merging #3839 will **not alter performance**

<sub>Comparing <code>Di-Is:add-override-namaspace-to-toml</code> (a25f901) with <code>main</code> (1b769b0)</sub>



### Summary

`✅ 13` untouched benchmarks






---

_Comment by @konstin on 2024-05-27 09:58_

I skimmed through the diff and it looks good. To get the tests passing you need to run `cargo dev generate-json-schema` and commit the json change. Mark your PR as ready for review and/or comment when you want a proper review.

---

_Comment by @Di-Is on 2024-05-28 12:54_

@konstin 
Thanks for the advice!
I was able to update the `uv.schema.json` successfully!

Over the next few days I added a feature to the output of the `uv pip compile` to show that it was overridden from the workspace configuration.

Here is an excerpt of the output from `uv pip compile`.
```txt
    werkzeug==2.3.0
        # via.
        # --override (from workspace)
        # flask.
```

However, the implementation has become more complicated with the addition of this feature.
Please review when you have time.

---

_Marked ready for review by @Di-Is on 2024-05-28 12:54_

---

_Renamed from "Add override namaspace to pyproject.toml/uv.toml" to "Add override namespace to pyproject.toml/uv.toml" by @Di-Is on 2024-05-28 14:01_

---

_Review comment by @konstin on `crates/pep508-rs/src/origin.rs`:13 on 2024-05-29 14:57_

Can we call this `WorkspaceOverride`?

---

_Review comment by @konstin on `crates/pep508-rs/src/origin.rs`:22 on 2024-05-29 14:58_

Can we instead attach the file containing the override?

---

_Review comment by @konstin on `crates/distribution-types/src/annotation.rs`:28 on 2024-05-29 14:59_

Could you write a value here instead of panicking?

---

_Review comment by @konstin on `crates/uv-workspace/src/settings.rs`:40 on 2024-05-29 15:03_

Can you make this `Option<Vec<Requirement>>`? This should simplify the `specification.rs` code too.

---

_@konstin reviewed on 2024-05-29 15:04_

---

_@Di-Is reviewed on 2024-05-30 11:46_

---

_Review comment by @Di-Is on `crates/pep508-rs/src/origin.rs`:22 on 2024-05-30 11:46_

I understand that multiple toml files(pyproject.toml, uv.toml) are combined to obtain an override dependency.

It seems difficult to keep track of which files have which overriding dependencies.

I'll give it a thought.

---

_@Di-Is reviewed on 2024-05-30 15:04_

---

_Review comment by @Di-Is on `crates/pep508-rs/src/origin.rs`:13 on 2024-05-30 15:04_

I chose a more general name considering that we might also add a similar namespace to toml for constraints in the future.

---

_@konstin reviewed on 2024-05-31 09:09_

---

_Review comment by @konstin on `crates/pep508-rs/src/origin.rs`:22 on 2024-05-31 09:09_

Make sense! What i'd like to do is try to avoid explicit panics where possible. Sometimes they are unavoidable, sometimes so you restructure code so the codepath becomes unreachable, sometimes you add a dummy value, etc.

---

_@Di-Is reviewed on 2024-06-02 13:34_

---

_Review comment by @Di-Is on `crates/pep508-rs/src/origin.rs`:22 on 2024-06-02 13:34_

I changed to return dummy paths instead of panic.

https://github.com/astral-sh/uv/pull/3839/commits/766bf17dad667fe62a0f9cefd10a066183025b79#diff-a8cc5969fdef854f20c6a58c35e8727e9066c25b4625338501beda19b447ac2bR23

---

_@Di-Is reviewed on 2024-06-02 13:36_

---

_Review comment by @Di-Is on `crates/distribution-types/src/annotation.rs`:28 on 2024-06-02 13:36_

I changed so as not to cause panic.

https://github.com/astral-sh/uv/pull/3839/commits/f480a6571c4e4cac948291730acfbf1acb3af673#diff-838c9ead31fb7f2602f55cb685ca61849dffa88781386be29b84027a83b7258eR28-R30

---

_@Di-Is reviewed on 2024-06-02 13:37_

---

_Review comment by @Di-Is on `crates/uv-workspace/src/settings.rs`:40 on 2024-06-02 13:37_

I added the deserialization process and JsonSchema and changed the type to Vec<pypi_types::Reuqirement>.

#### JsonSchema
https://github.com/astral-sh/uv/pull/3839/commits/766bf17dad667fe62a0f9cefd10a066183025b79#diff-ef8d164011970bb55e80ae99198f0698f74e77faf77cfa76ed6ea650194cad3fR34

#### Deserialization

https://github.com/astral-sh/uv/pull/3839/commits/766bf17dad667fe62a0f9cefd10a066183025b79#diff-c615ba811c1d932585ca0c27d737c61469970854c4e527a9e49d73cba5bef35fR46

---

_@Di-Is reviewed on 2024-06-02 13:57_

---

_Review comment by @Di-Is on `crates/uv/src/main.rs`:114 on 2024-06-02 13:57_

If the flag is not set here, warnings of workspace parsing failure will not be output.

---

_@Di-Is reviewed on 2024-06-02 13:59_

---

_Review comment by @Di-Is on `crates/uv/src/main.rs`:160 on 2024-06-02 13:59_

Reflect workspace quiet flag after workspace loading.
Added new uv_warnings::disable() for this process.

---

_@Di-Is reviewed on 2024-06-02 14:02_

---

_Review comment by @Di-Is on `crates/uv/src/main.rs`:114 on 2024-06-02 14:02_

I noticed this bug when I was creating the test.

If it is better to separate them into separate PRs, I will remove the relevant process and some of the tests.

---

_Review requested from @konstin by @Di-Is on 2024-06-02 14:02_

---

_Review comment by @konstin on `crates/uv/src/main.rs`:114 on 2024-06-03 09:35_

Yeah, could you create a different PR for those please?

---

_@konstin reviewed on 2024-06-03 09:35_

---

_@konstin approved on 2024-06-03 09:50_

---

_Comment by @konstin on 2024-06-03 09:52_

I've removed the warnings changes in this PR to keep this to only adding `overrides-dependencies`, if you want you can make a separate PR for the warnings changes.

---

_Merged by @konstin on 2024-06-03 10:15_

---

_Closed by @konstin on 2024-06-03 10:15_

---

_Comment by @charliermarsh on 2024-06-03 13:13_

@konstin - How does this intersect with the existing overrides feature?

---

_Comment by @konstin on 2024-06-03 14:23_

We chain them, so you can use both atm: https://github.com/astral-sh/uv/blob/5c776939d2e1bf388fb0beda1d1ec149ac4ae6e5/crates/uv/src/commands/pip/compile.rs#L406-L415

---

_Branch deleted on 2024-07-19 06:29_

---
