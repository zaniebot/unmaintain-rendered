```yaml
number: 13831
title: "[red-knot] Cleanup generated names of mdtest tests"
type: pull_request
state: merged
author: Aditya-PS-05
labels:
  - ty
assignees: []
merged: true
base: main
head: 13768/cleanup-markdown-test
created_at: 2024-10-20T10:27:39Z
updated_at: 2024-10-21T15:56:16Z
url: https://github.com/astral-sh/ruff/pull/13831
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Cleanup generated names of mdtest tests

---

_Pull request opened by @Aditya-PS-05 on 2024-10-20 10:27_

Part of #13768 

## Summary
Added `dir-test` to `red_knot_python_semantic` and modify `mdtest` definition.

---

_Review requested from @carljm by @Aditya-PS-05 on 2024-10-20 10:27_

---

_Review requested from @MichaReiser by @Aditya-PS-05 on 2024-10-20 10:27_

---

_Review requested from @AlexWaygood by @Aditya-PS-05 on 2024-10-20 10:27_

---

_Comment by @github-actions[bot] on 2024-10-20 10:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `red-knot` added by @MichaReiser on 2024-10-20 10:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/Cargo.toml`:38 on 2024-10-20 13:07_

Our preferred style for pinning dependencies is to put the pins in the `Cargo.toml` file in the repository root. So you'd put `dir-test = "0.3.0"` in the `Cargo.toml` file in the repository root, and in this `Cargo.toml` file you'd just put `dir-test = { workspace = true }`

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/tests/mdtest.rs`:4 on 2024-10-20 13:07_

Please add this doc-comment back -- it's very useful!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/tests/mdtest.rs`:22 on 2024-10-20 13:11_

This certainly makes the tests run a lot faster! Unfortunately, that's because you're not actually testing anything anymore 😄 You need to make sure you're actually still calling `red_knot_test::run()` on the path after you've "cleaned the path up" :-)

---

_@AlexWaygood requested changes on 2024-10-20 13:11_

Thanks!

---

_Review comment by @Aditya-PS-05 on `crates/red_knot_python_semantic/tests/mdtest.rs`:22 on 2024-10-20 13:18_

Sorry, I didn't understand. Are you trying to point out it's wrong and so I need to add `red_knot_test::run()`?

---

_@Aditya-PS-05 reviewed on 2024-10-20 13:18_

---

_@AlexWaygood reviewed on 2024-10-20 13:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/tests/mdtest.rs`:22 on 2024-10-20 13:24_

> Sorry, I didn't understand. Are you trying to point out it's wrong and so I need to add `red_knot_test::run()`?

Yes.

The way these tests work is a macro generates (at build time) a Rust test for each file in a specific directory. Each generated test runs `red_knot_test::run()` on a specific Markdown file here. The `red_knot_test::run()` call is the part of the test that actually makes assertions that red-knot is running correctly:

https://github.com/astral-sh/ruff/blob/7ca357119496ecae3bcc72549e0133e8b69c3ec6/crates/red_knot_python_semantic/tests/mdtest.rs#L13 

Your PR currently removes this call, meaning that the macro is generating a lot of tests, but those tests aren't really testing anything :-)

---

_Renamed from "modifying markdown test names" to "[red-knot] Cleanup generated names of mdtest tests" by @AlexWaygood on 2024-10-20 13:31_

---

_@Aditya-PS-05 reviewed on 2024-10-20 14:00_

---

_Review comment by @Aditya-PS-05 on `crates/red_knot_python_semantic/tests/mdtest.rs`:22 on 2024-10-20 14:00_

Required changes done.
Thanks for review and sorry for blunders.

---

_@AlexWaygood reviewed on 2024-10-20 14:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/tests/mdtest.rs`:22 on 2024-10-20 14:14_

No worries!

---

_@AlexWaygood approved on 2024-10-20 14:16_

Thanks! This LGTM. I confirmed it implements the feature @MichaReiser asked for, and that the tests still fail correctly if there's an incorrect assertion in a `.md` file. The tests also _appear_ to be running slightly faster for me locally as well, which is nice!

I'll wait for another approval before merging, though, as I'm pretty new to `dir-test`.

---

_@MichaReiser reviewed on 2024-10-20 14:29_

---

_Review comment by @MichaReiser on `Cargo.lock`:410 on 2024-10-20 14:29_

Hmm, that's unfortunate that dir-test still uses `syn` one but so be it. We can always make a PR upstream

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/Cargo.toml`:38 on 2024-10-20 14:29_

This should be a dev dependency

---

_@MichaReiser approved on 2024-10-20 14:30_

This looks good. Thanks. Let's move the `dir-test` dependency to the dev dependency section and we can land this

---

_Review comment by @MichaReiser on `Cargo.toml`:68 on 2024-10-20 15:01_

```suggestion
dir-test = { version = "0.3.0" }
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/Cargo.toml`:46 on 2024-10-20 15:01_

```suggestion
ruff_db = { workspace = true, features = ["os", "testing"] }
ruff_python_parser = { workspace = true }
red_knot_test = { workspace = true }
red_knot_vendored = { workspace = true }

anyhow = { workspace = true }
dir-test = {workspace = true}
```

---

_@MichaReiser approved on 2024-10-20 15:01_

Thanks

---

_Merged by @MichaReiser on 2024-10-20 15:11_

---

_Closed by @MichaReiser on 2024-10-20 15:11_

---

_Branch deleted on 2024-10-20 15:15_

---

_Comment by @Aditya-PS-05 on 2024-10-20 15:19_

Can I ask a question? 
I am a student and I want to get an internship as in my college internship is necessary. Can you guys please suggest me advice or give me tips to get an work through open-source and how to improve my open-source work? 

Thanks 

---

_Comment by @AlexWaygood on 2024-10-21 10:47_

Hey @Aditya-PS-05. Thanks again for the PR! It was really useful, and an important improvement.

GitHub issue trackers and PR threads aren't really the best place to ask for this kind of advice. There are forums such as Reddit and various programming Discord servers where volunteers are often very happy to give this kind of advice, but on GitHub we prefer to be more focussed on specific coding projects and problems :-)

That said, my advice would be:
- Continue submitting open-source PRs! I managed to get hired on the back of my open-source experience, so it's definitely possible.
- Focus on PRs where you're confident you understand how to fix the problem. A PR that doesn't really address an issue or that approaches an issue in completely the wrong way can end up meaning that a maintainer has to spend more time reviewing and fixing the PR than they would have spent if they just did it themselves.

  This PR wasn't that -- I think it did save us some time! It did also have some important mistakes in it at the start, though, which I think could possibly have been avoided if you'd taken some more time to think about the problem before submitting the PR.
- Your ability to make excellent PRs will improve if you continue to improve your programming ability in other ways as well. Read lots of code, watch YouTube programming tutorials, read tutorials/howtos/StackOverflow questions, continue to work on your own hobby projects. You don't have to do all of these, but they're all great ways to continue learning and developing your skills, and they'll all make you a better open-source contributor in the long run.

Hope that helps. Good luck with your coding journey! :-)

---

_Comment by @Aditya-PS-05 on 2024-10-21 15:56_




> Hey @Aditya-PS-05. Thanks again for the PR! It was really useful, and an important improvement.
> 
> GitHub issue trackers and PR threads aren't really the best place to ask for this kind of advice. There are forums such as Reddit and various programming Discord servers where volunteers are often very happy to give this kind of advice, but on GitHub we prefer to be more focussed on specific coding projects and problems :-)
> 
> That said, my advice would be:
> 
>     * Continue submitting open-source PRs! I managed to get hired on the back of my open-source experience, so it's definitely possible.
> 
>     * Focus on PRs where you're confident you understand how to fix the problem. A PR that doesn't really address an issue or that approaches an issue in completely the wrong way can end up meaning that a maintainer has to spend more time reviewing and fixing the PR than they would have spent if they just did it themselves.
>       This PR wasn't that -- I think it did save us some time! It did also have some important mistakes in it at the start, though, which I think could possibly have been avoided if you'd taken some more time to think about the problem before submitting the PR.
> 
>     * Your ability to make excellent PRs will improve if you continue to improve your programming ability in other ways as well. Read lots of code, watch YouTube programming tutorials, read tutorials/howtos/StackOverflow questions, continue to work on your own hobby projects. You don't have to do all of these, but they're all great ways to continue learning and developing your skills, and they'll all make you a better open-source contributor in the long run.
> 
> 
> Hope that helps. Good luck with your coding journey! :-)

Thank you @AlexWaygood for such a great advice.

---
