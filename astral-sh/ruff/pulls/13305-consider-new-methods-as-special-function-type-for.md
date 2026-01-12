```yaml
number: 13305
title: "Consider `__new__` methods as special function type for enforcing class method or static method rules"
type: pull_request
state: merged
author: cake-monotone
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: dunder-new-as-staticmethod
created_at: 2024-09-10T11:08:48Z
updated_at: 2025-02-16T20:28:53Z
url: https://github.com/astral-sh/ruff/pull/13305
synced_at: 2026-01-12T15:55:43Z
```

# Consider `__new__` methods as special function type for enforcing class method or static method rules

---

_@cake-monotone_

## Summary

`__new__` methods are technically static methods, with `cls` as their first argument. However, Ruff currently classifies them as classmethod, which causes two issues:

- It conveys incorrect information, leading to confusion. For example, in cases like ARG003, `__new__` is explicitly treated as a classmethod.
- Future rules that should apply to staticmethod may not be applied correctly due to this misclassification.

Motivated by this, the current PR makes the following adjustments:

1. Introduces `FunctionType::NewMethod` as an enum variant, since, for the purposes of lint rules, `__new__` sometimes behaves like a static method and other times like a class method. This is an internal change.

2. The following rule behaviors and messages are totally unchanged:
   - [too-many-arguments (PLR0913)](https://docs.astral.sh/ruff/rules/too-many-arguments/#too-many-arguments-plr0913)
   - [too-many-positional-arguments (PLR0917)](https://docs.astral.sh/ruff/rules/too-many-positional-arguments/#too-many-positional-arguments-plr0917)
3. The following rule behaviors are unchanged, but the messages have been changed for correctness to use "`__new__` method" instead of "class method":
   - [self-or-cls-assignment (PLW0642)](https://docs.astral.sh/ruff/rules/self-or-cls-assignment/#self-or-cls-assignment-plw0642)
4. The following rules are changed _unconditionally_ (not gated behind preview) because their current behavior is an honest bug: it just isn't true that `__new__` is a class method, and it _is_ true that `__new__` is a static method:
   - [unused-class-method-argument (ARG003)](https://docs.astral.sh/ruff/rules/unused-class-method-argument/#unused-class-method-argument-arg003) no longer applies to `__new__`
   - [unused-static-method-argument (ARG004)](https://docs.astral.sh/ruff/rules/unused-static-method-argument/#unused-static-method-argument-arg004) now applies to `__new__`
5. The only changes which differ based on `preview` are the following:
   - [invalid-first-argument-name-for-class-method (N804)](https://docs.astral.sh/ruff/rules/invalid-first-argument-name-for-class-method/#invalid-first-argument-name-for-class-method-n804): This is _skipped_ when `preview` is _enabled_. When `preview` is _disabled_, the rule is the same but the _message_ has been modified to say "`__new__` method" instead of "class method".
   - [bad-staticmethod-argument (PLW0211)](https://docs.astral.sh/ruff/rules/bad-staticmethod-argument/#bad-staticmethod-argument-plw0211): When `preview` is enabled, this now applies to `__new__`.

Closes #13154

---

_Review requested from @AlexWaygood by @cake-monotone on 2024-09-10 11:08_

---

_Comment by @github-actions[bot] on 2024-09-10 11:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -2 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/5e0367a180d1d3b321492f1c3939aee4c1665278/src/latch_cli/centromere/utils.py#L67'>src/latch_cli/centromere/utils.py:67:34:</a> ARG003 Unused class method argument: `args`
+ <a href='https://github.com/latchbio/latch/blob/5e0367a180d1d3b321492f1c3939aee4c1665278/src/latch_cli/centromere/utils.py#L67'>src/latch_cli/centromere/utils.py:67:34:</a> ARG004 Unused static method argument: `args`
- <a href='https://github.com/latchbio/latch/blob/5e0367a180d1d3b321492f1c3939aee4c1665278/src/latch_cli/centromere/utils.py#L67'>src/latch_cli/centromere/utils.py:67:42:</a> ARG003 Unused class method argument: `kwargs`
+ <a href='https://github.com/latchbio/latch/blob/5e0367a180d1d3b321492f1c3939aee4c1665278/src/latch_cli/centromere/utils.py#L67'>src/latch_cli/centromere/utils.py:67:42:</a> ARG004 Unused static method argument: `kwargs`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ARG004 | 2 | 2 | 0 | 0 | 0 |
| ARG003 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -2 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/5e0367a180d1d3b321492f1c3939aee4c1665278/src/latch_cli/centromere/utils.py#L67'>src/latch_cli/centromere/utils.py:67:34:</a> ARG003 Unused class method argument: `args`
+ <a href='https://github.com/latchbio/latch/blob/5e0367a180d1d3b321492f1c3939aee4c1665278/src/latch_cli/centromere/utils.py#L67'>src/latch_cli/centromere/utils.py:67:34:</a> ARG004 Unused static method argument: `args`
- <a href='https://github.com/latchbio/latch/blob/5e0367a180d1d3b321492f1c3939aee4c1665278/src/latch_cli/centromere/utils.py#L67'>src/latch_cli/centromere/utils.py:67:42:</a> ARG003 Unused class method argument: `kwargs`
+ <a href='https://github.com/latchbio/latch/blob/5e0367a180d1d3b321492f1c3939aee4c1665278/src/latch_cli/centromere/utils.py#L67'>src/latch_cli/centromere/utils.py:67:42:</a> ARG004 Unused static method argument: `kwargs`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ARG004 | 2 | 2 | 0 | 0 | 0 |
| ARG003 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Comment by @cake-monotone on 2024-09-10 11:52_

I'm sure there are other breaking changes besides the ruff-ecosystem results
I'll try to summarize them.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-09-10 13:21_

---

_Comment by @cake-monotone on 2024-09-11 09:42_

## Affected Rules
- **ARG003**: unused-class-method-argument
- **ARG004**: unused-static-method-argument
- **PLW0642**: self-or-cls-assignment
- **PLW0211**: bad-staticmethod-argument
- **N804**: invalid-first-argument-name-for-class-method
- **PLR0913**: too-many-arguments
- **PLR0917**: too-many-positional-arguments

### ARG003, ARG004
**ARG003** (`unused-class-method-argument`) -> **ARG004** (`unused-static-method-argument`)

### PLW0642
`__new__` methods will no longer be checked by **PLW0642**. This rule only applies to methods and class methods, not static methods.

### N804, PLW0211
**N804** (`invalid-first-argument-name-for-class-method`) -> **PLW0211** (`bad-staticmethod-argument`)

### PLR0913, PLR0917
**PLR0913** (`too-many-arguments`) and **PLR0917** (`too-many-positional-arguments`) behave differently for classmethod and staticmethod. In classmethod, the first argument cls is excluded from the argument count. In staticmethod, cls is not excluded, so the argument limit applies more strictly.

## Example

You can view an example of how these rules are applied here:
https://play.ruff.rs/2df5f2b6-093f-4988-a562-c6101a1c71cc

```py
class ClassDunderNew:
    @classmethod
    def __new__(
        cls,
        x,  # ARG003: Unused class method argument: `x`
    ):
        cls = 1  # PLW0642: Reassigned `cls` variable in class method

    @classmethod
    def __new__(  # PLR0913: Too many arguments in function definition (6 > 5)  # PLR0917: Too many positional arguments (6/5)
        self,  # N804: First argument of a class method should be named `cls`
        a1, a2, a3, a4, a5,
        a6,
    ):
        ...


class StaticDunderNew:
    @staticmethod
    def __new__(
        cls,
        x,  # ARG004: Unused static method argument: `x`
    ):
        cls = 1

    @staticmethod
    def __new__(  # PLR0913: Too many arguments in function definition (6 > 5)  # PLR0917: Too many positional arguments (6/5)
        self,  # PLW0211: First argument of a static method should not be named `self`
        a1, a2, a3, a4, a5,
    ):
        ...
```

---

_Comment by @cake-monotone on 2024-09-11 09:51_

I reviewed all the code affected by `function_type::classify`. Let me know if I missed any affected rules.

Please check if these affected rules are acceptable. :)

---

_Unassigned @AlexWaygood by @AlexWaygood on 2024-09-19 16:47_

---

_Label `rule` added by @MichaReiser on 2024-12-26 09:37_

---

_Label `needs-decision` added by @MichaReiser on 2024-12-26 09:37_

---

_Label `needs-decision` removed by @dylwil3 on 2025-02-15 18:49_

---

_@dylwil3 reviewed on 2025-02-15 18:53_

Thanks so much!

I think this is pretty much good to go - had to tweak some things to rebase, and I decided we should special case the counting of arguments for the "too-many-(positional-)arguments" rules to treat `__new__` like a classmethod, since it seems more intuitive there. Also added more test fixtures to track changes if we decide to further modify the treatment of `__new__` later.

My only hesitation on merging is whether this should be considered breaking and wait until the next minor release - thoughts @AlexWaygood / @MichaReiser ?

Also to Alex: It'd be great if you could spot check the conflict resolution for PYI019 to make sure it's in line with what you had in mind for the recent changes to that rule. https://github.com/astral-sh/ruff/pull/13305/files#diff-bfce927fd2c71368a7fc9c045c1f51f03f4ac25e398ed8b3a571caf01f4efe06

---

_Comment by @MichaReiser on 2025-02-15 21:35_

I'd prefer if this is a preview only change just because of the scope of it. It's definitely a breaking change otherwise 

---

_Comment by @dylwil3 on 2025-02-16 19:25_

I don't think making the change preview only makes sense in many cases, since I consider the old behavior a bug/incorrect. Here's what I ended up doing. Even though this is a long list, there are actually not so many behavior changes, so I wouldn't consider this breaking, personally. What do you think?

1. As suggested by Alex, I introduced `FunctionType::NewMethod` as an enum variant, since, for the purposes of lint rules, `__new__` sometimes behaves like a static method and other times like a class method. This is an internal change.

2. The following rule behaviors and messages are totally unchanged:
   - [too-many-arguments (PLR0913)](https://docs.astral.sh/ruff/rules/too-many-arguments/#too-many-arguments-plr0913)
   - [too-many-positional-arguments (PLR0917)](https://docs.astral.sh/ruff/rules/too-many-positional-arguments/#too-many-positional-arguments-plr0917)
3. The following rule behaviors are unchanged, but the messages have been changed for correctness to use "`__new__` method" instead of "class method":
   - [self-or-cls-assignment (PLW0642)](https://docs.astral.sh/ruff/rules/self-or-cls-assignment/#self-or-cls-assignment-plw0642)
4. The following rules are changed _unconditionally_ (not gated behind preview) because I think their current behavior is an honest bug: it just isn't true that `__new__` is a class method, and it _is_ true that `__new__` is a static method:
   - [unused-class-method-argument (ARG003)](https://docs.astral.sh/ruff/rules/unused-class-method-argument/#unused-class-method-argument-arg003) no longer applies to `__new__`
   - [unused-static-method-argument (ARG004)](https://docs.astral.sh/ruff/rules/unused-static-method-argument/#unused-static-method-argument-arg004) now applies to `__new__`
5. The only changes which differ based on `preview` are the following:
   - [invalid-first-argument-name-for-class-method (N804)](https://docs.astral.sh/ruff/rules/invalid-first-argument-name-for-class-method/#invalid-first-argument-name-for-class-method-n804): This is _skipped_ when `preview` is _enabled_. When `preview` is _disabled_, the rule is the same but the _message_ has been modified to say "`__new__` method" instead of "class method".
   - [bad-staticmethod-argument (PLW0211)](https://docs.astral.sh/ruff/rules/bad-staticmethod-argument/#bad-staticmethod-argument-plw0211): When `preview` is enabled, this now applies to `__new__`.


---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/function_type.rs`:44 on 2025-02-16 19:44_

```suggestion
            "__init_subclass__" | "__class_getitem__" => FunctionType::ClassMethod, // Implicit class methods.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:471 on 2025-02-16 19:59_

```suggestion
                        // we use `method()` here rather than `function()`, as although `__new__` is
                        // an implicit staticmethod, `__new__` methods must always have >= parameter
                        method(Argumentable::StaticMethod, parameters, scope, checker);
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_first_argument_name.rs`:141 on 2025-02-16 20:00_

Lol, yet another reason for us to introduce a `summary_sentence()` method to the Violation trait instead of deriving the summary sentence from the `message()` method 🙃

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/function_type.rs`:15 on 2025-02-16 20:01_

Nit: you could use doc-comments here so that we get nice tooltips when hovering over this method in VSCode 

```suggestion
    /// `__new__` is an implicit static method but
    /// is treated similarly to class methods for several lint rules
```

---

_@AlexWaygood approved on 2025-02-16 20:02_

This looks excellent! Thanks @cake-monotone for the PR, and thanks @dylwil3 for driving it over the line!

---

_Label `preview` added by @dylwil3 on 2025-02-16 20:05_

---

_Renamed from "Refactor method classification logic: `__new__` methods are now classified as `staticmethod`" to "Consider `__new__` methods as special function type for enforcing class method or static method rules" by @dylwil3 on 2025-02-16 20:09_

---

_Merged by @dylwil3 on 2025-02-16 20:12_

---

_Closed by @dylwil3 on 2025-02-16 20:12_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_first_argument_name.rs`:242 on 2025-02-16 20:26_

Oh, I guess it might be good to add a note about this to the rule's docs?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_unused_arguments/rules/unused_arguments.rs`:470 on 2025-02-16 20:27_

Ah shoot, my previous suggestion had a typo -- it should have been this :-(

```suggestion
                        // an implicit staticmethod, `__new__` methods must always have >=1 parameters
```

---

_@AlexWaygood reviewed on 2025-02-16 20:28_

argh sorry, a couple of tiny nits I missed pre-merge that might be good to address as a followup

---
