```yaml
number: 18906
title: Add missing rule code comments
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - internal
assignees: []
merged: true
base: main
head: add-rule-code-comments
created_at: 2025-06-24T00:48:05Z
updated_at: 2025-06-25T01:42:36Z
url: https://github.com/astral-sh/ruff/pull/18906
synced_at: 2026-01-12T15:56:27Z
```

# Add missing rule code comments

---

_@MeGaGiGaGon_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

While making some of my other changes, I noticed some of the lints were missing comments with their lint code/had the wrong numbered lint code. These comments are super useful since they allow for very easily and quickly finding the source code of a lint, so I decided to try and normalize them.

Most of them were fairly straightforward, just adding a doc comment/comment in the appropriate place.

I decided to make all of the `Pylint` rules have the `PL` prefix. Previously it was split between no prefix and having prefix, but I decided to normalize to with prefix since that's what's in the docs, and the with prefix will show up on no prefix searches, while the reverse is not true.

I also ran into a lot of rules with implementations in "non-standard" places (where "standard" means inside a file matching the glob `crates/ruff_linter/rules/*/rules/**/*.rs` and/or the same rule file where the rule `struct`/`ViolationMetadata` is defined).

I decided to move all the implementations out of `crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs` and into their own files, since that is what the rest of the rules in `deferred_scopes.rs` did, and those were just the outliers.

There were several rules which I did not end up moving, which you can see as the extra paths I had to add to my python code besides the "standard" glob. These rules are generally the error-type rules that just wrap an error from the parser, and have very small implementations/are very tightly linked to the module they are in, and generally every rule of that type was implemented in module instead of in the "standard" place.

Resolving that requires answering a question I don't think I'm equipped to handle: Is the point of these comments to give quick access to the rule definition/docs, or the rule implementation? For all the rules with implementations in the "standard" location this isn't a problem, as they are the same, but it is an issue for all of these error type rules. In the end I chose to leave the implementations where they were, but I'm not sure if that was the right choice.

<details>
<summary>Python script I wrote to find missing comments</summary>

This script assumes it is placed in the top level `ruff` directory (ie next to `.git`/`crates`/`README.md`)

```py
import re
from copy import copy
from pathlib import Path

linter_to_code_prefix = {
    "Airflow": "AIR",
    "Eradicate": "ERA",
    "FastApi": "FAST",
    "Flake82020": "YTT",
    "Flake8Annotations": "ANN",
    "Flake8Async": "ASYNC",
    "Flake8Bandit": "S",
    "Flake8BlindExcept": "BLE",
    "Flake8BooleanTrap": "FBT",
    "Flake8Bugbear": "B",
    "Flake8Builtins": "A",
    "Flake8Commas": "COM",
    "Flake8Comprehensions": "C4",
    "Flake8Copyright": "CPY",
    "Flake8Datetimez": "DTZ",
    "Flake8Debugger": "T10",
    "Flake8Django": "DJ",
    "Flake8ErrMsg": "EM",
    "Flake8Executable": "EXE",
    "Flake8Fixme": "FIX",
    "Flake8FutureAnnotations": "FA",
    "Flake8GetText": "INT",
    "Flake8ImplicitStrConcat": "ISC",
    "Flake8ImportConventions": "ICN",
    "Flake8Logging": "LOG",
    "Flake8LoggingFormat": "G",
    "Flake8NoPep420": "INP",
    "Flake8Pie": "PIE",
    "Flake8Print": "T20",
    "Flake8Pyi": "PYI",
    "Flake8PytestStyle": "PT",
    "Flake8Quotes": "Q",
    "Flake8Raise": "RSE",
    "Flake8Return": "RET",
    "Flake8Self": "SLF",
    "Flake8Simplify": "SIM",
    "Flake8Slots": "SLOT",
    "Flake8TidyImports": "TID",
    "Flake8Todos": "TD",
    "Flake8TypeChecking": "TC",
    "Flake8UnusedArguments": "ARG",
    "Flake8UsePathlib": "PTH",
    "Flynt": "FLY",
    "Isort": "I",
    "McCabe": "C90",
    "Numpy": "NPY",
    "PandasVet": "PD",
    "PEP8Naming": "N",
    "Perflint": "PERF",
    "Pycodestyle": "",
    "Pydoclint": "DOC",
    "Pydocstyle": "D",
    "Pyflakes": "F",
    "PygrepHooks": "PGH",
    "Pylint": "PL",
    "Pyupgrade": "UP",
    "Refurb": "FURB",
    "Ruff": "RUF",
    "Tryceratops": "TRY",
}

ruff = Path(__file__).parent / "crates"

ruff_linter = ruff / "ruff_linter" / "src"

code_to_rule_name = {}

with open(ruff_linter / "codes.rs") as codes_file:
    for linter, code, rule_name in re.findall(
        # The (?<! skips ruff test rules
        # Only Preview|Stable rules are checked
        r"(?<!#\[cfg\(any\(feature = \"test-rules\", test\)\)\]\n)        \((\w+), \"(\w+)\"\) => \(RuleGroup::(?:Preview|Stable), [\w:]+::(\w+)\)",
        codes_file.read(),
    ):
        code_to_rule_name[linter_to_code_prefix[linter] + code] = (rule_name, [])

ruff_linter_rules = ruff_linter / "rules"
for rule_file_path in [
    *ruff_linter_rules.rglob("*/rules/**/*.rs"),
    ruff / "ruff_python_parser" / "src" / "semantic_errors.rs",
    ruff_linter / "pyproject_toml.rs",
    ruff_linter / "checkers" / "noqa.rs",
    ruff_linter / "checkers" / "ast" / "mod.rs",
    ruff_linter / "checkers" / "ast" / "analyze" / "unresolved_references.rs",
    ruff_linter / "checkers" / "ast" / "analyze" / "expression.rs",
    ruff_linter / "checkers" / "ast" / "analyze" / "statement.rs",
]:
    with open(rule_file_path, encoding="utf-8") as f:
        rule_file_content = f.read()
    for code, (rule, _) in copy(code_to_rule_name).items():
        if rule in rule_file_content:
            if f"// {code}" in rule_file_content or f", {code}" in rule_file_content:
                del code_to_rule_name[code]
            else:
                code_to_rule_name[code][1].append(rule_file_path)

for code, rule in code_to_rule_name.items():
    print(code, rule[0])
    for path in rule[1]:
        print(path)
```

</details>

## Test Plan

<!-- How was it tested? -->

N/A, no tests/functionality affected.

---

_Comment by @github-actions[bot] on 2025-06-24 00:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:842 on 2025-06-24 02:32_

This could be a good target for a follow-up PR, but we could fold these `if checker.is_rule_enabled { checker.report_diagnostic` checks into calls to `checker.report_diagnostic_if_enabled`. I didn't notice these when adding that method.

Also, the docs on `Checker::report_diagnostic_if_enabled` and `LintContext::report_diagnostic_if_enabled` are outdated now that the `Rule` conversion is basically free :sweat_smile: 

No pressure to take on this refactor, just an idea if you're interested!

---

_@ntBre reviewed on 2025-06-24 02:47_

Nice! I always grep for the rule codes first, so this will definitely help me. I'm curious if @MichaReiser has any strong feelings about moving the rule implementations, but I think that makes sense to me too.

> Resolving that requires answering a question I don't think I'm equipped to handle: Is the point of these comments to give quick access to the rule definition/docs, or the rule implementation?

If I had to pick one, I'd probably say implementation. The implementation is virtually guaranteed to include a reference to the `Violation` struct, and it's a bit easier to jump to definition on that struct than find all references from the struct definition, in my opinion. So I think you put them in the right place, for my tastes at least.

I also think it made sense to leave those very short rules in place. They're basically one-liners, especially if we switch to `report_diagnostic_if_enabled`.

---

_Label `internal` added by @ntBre on 2025-06-24 02:48_

---

_@MeGaGiGaGon reviewed on 2025-06-24 05:49_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:842 on 2025-06-24 05:49_

Sure, I could see if I can make something up for that after this is merged (~~because otherwise there'd probably be a billion merge conflicts~~ Looking at it more, probably not actually?). Side note, I find it really funny how I'm basically writing lints in python for the rust code of a python linter.

---

_Comment by @MichaReiser on 2025-06-24 06:04_

I'd prefer if we can split the PR into

1. Add the missing rule comments
2. Anything changing the structure

That makes the discussion easier. I personally don't find "standard locations" to be that important to find an implementation because I use go to definition or find all references to find the implementation. It's also often easier to understand the implementation if everything is in one place.

---

_Comment by @MeGaGiGaGon on 2025-06-24 17:48_

> I'd prefer if we can split the PR into
> 
> 1. Add the missing rule comments
> 2. Anything changing the structure
> 
> That makes the discussion easier. I personally don't find "standard locations" to be that important to find an implementation because I use go to definition or find all references to find the implementation. It's also often easier to understand the implementation if everything is in one place.

Split out, I think I did all the git stuff correct. I'll wait to make a PR on the movement parts since the comments for the ones that I'm considering moving would not be moved in that branch, making a mess. Those changes are now [in this branch](https://github.com/MeGaGiGaGon/ruff/tree/move-big-rule-imeplementations)

I guess in the end I prefer the comments on the implementation, since that's what I usually want to see. Having them gets rid of a jump or two, since while it's not too hard it does take a while to navigate through the links and find what you're looking for. The unfortunate part of having the comments on the implementations is that it's harder to get to the docs if that's your goal, but oh well.

---

_Comment by @MichaReiser on 2025-06-24 19:13_

> Split out, I think I did all the git stuff correct. 

Thank you! And sorry for the extra work.

> The unfortunate part of having the comments on the implementations is that it's harder to get to the docs if that's your goal, but oh well.

Yeah, there's no perfect solution. What I do is that I search where we emit the diagnostic and then use Go to definition to jump to the violation definition. I find that easier than going from the violation definition to the implementation (IDEs are slower at finding all references)

---

_@ntBre approved on 2025-06-25 01:17_

Awesome, thank you!

---

_Merged by @ntBre on 2025-06-25 01:18_

---

_Closed by @ntBre on 2025-06-25 01:18_

---

_Branch deleted on 2025-06-25 01:42_

---
