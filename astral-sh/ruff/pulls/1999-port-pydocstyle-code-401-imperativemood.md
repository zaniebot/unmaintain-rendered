```yaml
number: 1999
title: Port pydocstyle code 401 (ImperativeMood)
type: pull_request
state: merged
author: akx
labels: []
assignees: []
merged: true
base: main
head: imperative-mood
created_at: 2023-01-19T15:35:24Z
updated_at: 2023-01-20T12:18:33Z
url: https://github.com/astral-sh/ruff/pull/1999
synced_at: 2026-01-12T15:55:07Z
```

# Port pydocstyle code 401 (ImperativeMood)

---

_@akx_

This adds support for pydocstyle code D401 using the `imperative` crate.

---

_Marked ready for review by @akx on 2023-01-19 15:47_

---

_Comment by @not-my-profile on 2023-01-19 17:14_

Looks neat! Some thoughts:

* I assume that this only supports English? I think we're quite likely to end up with multiple language-specific lints (e.g. spell checking has already been requested #1628), so at some point we should probably introduce some `language` setting to better support non-English code bases (akin to our current `convention` setting, setting e.g. `language = "es"` would disable all language-specific lints that don't support Spanish).
* This introduces a new dependency for a single rule ... I think this would be a good candidate for a [feature gate](https://doc.rust-lang.org/cargo/reference/features.html).

---

_@not-my-profile reviewed on 2023-01-19 17:16_

---

_Review comment by @not-my-profile on `src/violations.rs`:3526 on 2023-01-19 17:16_

The violations are only all defined in `violations.rs` for historical reasons. We actually want to move these next to the rule implementations in `rules::`. Also you'll need to remove the placeholder and add `#[derive_message_formats]` to the `message` function.

---

_@not-my-profile reviewed on 2023-01-19 17:17_

---

_Review comment by @not-my-profile on `src/rules/pydocstyle/rules.rs`:816 on 2023-01-19 17:17_

I think it would make sense to move this new code (along with the violation defintion) to a new file `pydocstyle/non_imperative_mood.rs` since we actually want to break up such `rules.rs` files into smaller files.

---

_Review comment by @not-my-profile on `src/violations.rs`:4257 on 2023-01-19 17:19_

I think this violation should rather be named NonImperativeMood, see 5bf6da0db7ef76fd81f2932eac4a18d99e380789 for the reasoning.

---

_@not-my-profile reviewed on 2023-01-19 17:19_

---

_@not-my-profile reviewed on 2023-01-19 17:20_

---

_Review comment by @not-my-profile on `src/violations.rs`:3524 on 2023-01-19 17:20_

I think using a raw string literal `r#" ... "#` is nicer when the string contains double quotes.

---

_Review comment by @not-my-profile on `resources/test/fixtures/pydocstyle/D401.py`:11 on 2023-01-19 17:22_

nit: but maybe choose some less distracting method names

---

_@not-my-profile reviewed on 2023-01-19 17:22_

---

_@akx reviewed on 2023-01-19 17:36_

---

_Review comment by @akx on `src/violations.rs`:4257 on 2023-01-19 17:36_

I would kind of love that the name of the violation is also in imperative mood 😁 

---

_Review comment by @akx on `src/rules/pydocstyle/rules.rs`:816 on 2023-01-19 17:36_

Should I maybe make a separate PR that breaks up `pydocstyle/rules.rs` to begin with?

---

_@akx reviewed on 2023-01-19 17:36_

---

_@akx reviewed on 2023-01-19 17:38_

---

_Review comment by @akx on `src/violations.rs`:3526 on 2023-01-19 17:38_

Okay, sure – maybe `CONTRIBUTING.md` should be updated to instruct a non-historic method? 😅 

---

_Review comment by @not-my-profile on `src/rules/pydocstyle/rules.rs`:816 on 2023-01-19 17:44_

You don't need to break up all of `pydocstyle/rules.rs` to introduce a new pydocstyle rule but yes if you want to break that file up you could do so in a separate PR.

---

_@not-my-profile reviewed on 2023-01-19 17:44_

---

_@not-my-profile reviewed on 2023-01-19 17:45_

---

_Review comment by @not-my-profile on `src/violations.rs`:4257 on 2023-01-19 17:45_

Well the idea is that you can have `Allow <violation-name>` which is imperative mood :)

---

_Comment by @akx on 2023-01-19 17:49_

> * This introduces a new dependency for a single rule ... I think this would be a good candidate for a [feature gate](https://doc.rust-lang.org/cargo/reference/features.html).

I'm not sure how we'd ship `ruff` to PyPI with optional features since you can't just `pip install ruff[language]` them in... The dependency is pretty small, though; a release build grows 0,575 % with this feature:
```
$ gdu -b ruff-*
11223080	ruff-imp
11168864	ruff-main
```


---

_@not-my-profile reviewed on 2023-01-19 17:49_

---

_Review comment by @not-my-profile on `src/violations.rs`:3526 on 2023-01-19 17:49_

Yeah I agree that updating `CONTRIBUTING.md` would make sense ... however I am currently actively figuring out a better structure for ruff rules (moving away from the upstream-centric architecture since many rules are implemented by several linters), so the recommended structure will probably change again in the near future ... at which point I'll certainly make an effort to properly document it :)

---

_Review comment by @akx on `resources/test/fixtures/pydocstyle/D401.py`:11 on 2023-01-19 17:50_

Ported from https://github.com/PyCQA/pydocstyle/blob/master/src/tests/test_cases/test.py#L353-L368

---

_@akx reviewed on 2023-01-19 17:50_

---

_@akx reviewed on 2023-01-19 17:52_

---

_Review comment by @akx on `src/violations.rs`:3526 on 2023-01-19 17:52_

Sure, I get that ruff is a very fast-moving target :)

It's just that that's where new contributors (yours truly) look for instructions, and getting contradicting information in a PR review (especially with a "you'll need to" (though maybe it wasn't meant that way)) is a bit unfortunate.

---

_Comment by @not-my-profile on 2023-01-19 17:54_

Right the optional feature could be enabled by default. We are planning on publishing the `ruff` library on crates.io and for ruff as a library it probably makes sense to feature gate such optional dependencies ... however I just realized that `#[cfg(feature = "whatever")]` probably doesn't work within the `define_rule_mapping!` macro ... so I guess you can just forget about that for now.^^

---

_@akx reviewed on 2023-01-19 18:13_

---

_Review comment by @akx on `src/rules/pydocstyle/rules.rs`:816 on 2023-01-19 18:13_

@not-my-profile 👍  Done in https://github.com/charliermarsh/ruff/pull/2003.

---

_Comment by @charliermarsh on 2023-01-19 18:16_

(Yeah, conceptually it makes sense for us to use `features` in some places, but you're right that whatever we ship to PyPI probably has to use all default features with no configurability.)

---

_@akx reviewed on 2023-01-19 19:15_

---

_Review comment by @akx on `src/violations.rs`:4257 on 2023-01-19 19:15_

Will change to `ImperativeMood`, c.f. `FirstLineCapitalized`, `EndsInPunctuation`.

---

_@akx reviewed on 2023-01-19 19:27_

---

_Review comment by @akx on `src/violations.rs`:3524 on 2023-01-19 19:27_

I followed the style used elsewhere in the file.

---

_Renamed from "Port pydocstyle code 401 (UseImperativeMood)" to "Port pydocstyle code 401 (ImperativeMood)" by @akx on 2023-01-19 19:27_

---

_Review requested from @not-my-profile by @akx on 2023-01-19 19:39_

---

_Review comment by @not-my-profile on `src/violations.rs`:4257 on 2023-01-19 20:00_

The violation structs currently aren't named consistently so referencing two others doesn't really make sense. I think the `Allow <violation-name>` convention from Rust makes sense which would mean we'd have to always name the negative case ... which actually does makes sense as well for violations since "imperative mood" isn't a violation but what's expected .... "non-imperative mood" would be the violation.

---

_@not-my-profile reviewed on 2023-01-19 20:00_

---

_@not-my-profile reviewed on 2023-01-19 20:02_

---

_Review comment by @not-my-profile on `resources/test/fixtures/pydocstyle/D401.py`:11 on 2023-01-19 20:02_

Yes I don't think somebody smashing their keyboard is something we should port.

---

_@not-my-profile reviewed on 2023-01-19 20:06_

---

_Review comment by @not-my-profile on `src/violations.rs`:3526 on 2023-01-19 20:06_

Right I get what you mean :)

Now that you have an `imperative_mood.rs` file you could also move the violation definition there like I suggested.

---

_Review comment by @charliermarsh on `src/rules/pydocstyle/rules/imperative_mood.rs`:17 on 2023-01-20 00:29_

Unfortunately I think this is the index of the _last_ physical line in the logical line. So, like, if the "first logical line" extends beyond a single physical line, like:

```
"""Here's a logical line that
extends to two physical lines.
"""
```

Then the index would be 1 instead of 0.

We can either modify `logical_line` to return the (start, end) indices. _Or_, you could just do `body.trim().split_whitespace().next()`? You just want the very first word after all.


---

_@charliermarsh reviewed on 2023-01-20 00:29_

---

_Review comment by @charliermarsh on `src/rules/pydocstyle/rules/imperative_mood.rs`:17 on 2023-01-20 00:30_

(Otherwise, LGTM.)

---

_@charliermarsh reviewed on 2023-01-20 00:30_

---

_@not-my-profile requested changes on 2023-01-20 01:44_

As I said in the comments you now marked as resolved, I think it would be better if the violation struct was named NonImperativeMood and if it was defined in `non_imperative_mood.rs` (as opposed to `violations.rs`.).

---

_Comment by @charliermarsh on 2023-01-20 01:49_

Ah, yeah, sorry -- those are two conventions we're trying to adopt: moving the violations into the rule files, and naming rules such that "Allow X" is descriptive.

---

_Comment by @akx on 2023-01-20 09:26_

> if it was defined in `non_imperative_mood.rs` (as opposed to `violations.rs`.).

Oops, my bad – that broke in rebase.

> and naming rules such that "Allow X" is descriptive.

Okiedoke.

---

_@akx reviewed on 2023-01-20 10:52_

---

_Review comment by @akx on `src/rules/pydocstyle/rules/imperative_mood.rs`:17 on 2023-01-20 10:52_

Changed to trim the docstring, then get the first line, and the first word off there – the error message looks better when it has the context of the full first line 👍 

---

_Review requested from @charliermarsh by @akx on 2023-01-20 11:15_

---

_Merged by @charliermarsh on 2023-01-20 12:18_

---

_Closed by @charliermarsh on 2023-01-20 12:18_

---

_Comment by @charliermarsh on 2023-01-20 12:18_

Thank you so much!

---
