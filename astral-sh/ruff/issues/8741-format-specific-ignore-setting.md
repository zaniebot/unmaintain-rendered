```yaml
number: 8741
title: "format-specific `ignore` setting"
type: issue
state: closed
author: dga-nagra
labels:
  - question
assignees: []
created_at: 2023-11-17T15:32:30Z
updated_at: 2024-07-16T13:32:25Z
url: https://github.com/astral-sh/ruff/issues/8741
synced_at: 2026-01-12T15:54:48Z
```

# format-specific `ignore` setting

---

_@dga-nagra_

Hi,

First, thank you for this wonderful tool.

I am facing this warning:
```bash
ruff format ingest_cosmos_fw.py --preview
```
> warning: The following rules may cause conflicts when used with the formatter: `COM812`, `ISC001`. To avoid unexpected behavior, we recommend disabling these rules, either by removing them from the `select` or `extend-select` configuration, or adding them to the `ignore` configuration.

The rules work on `check`, but not on `format`.

Would it be possible to ignore them on `format` command only. Could we have an `extend-ignore` in the `[format]` ?



Also, I used the `flake8-bandit` tool, but it is not reporting the same way as the bandit tool itself and it is also not providing any fix recommendation (e.g. "Use defusedxml instead of lxml"). Is there a way to tweak that ?


---

_Comment by @zanieb on 2023-11-17 15:58_

The `format` command does not use any rule codes. Here, you are being warned that when you use `check`, it may make changes that conflict with the formatter since you are using format-related rules. See https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules for details.

> Also, I used the flake8-bandit tool, but it is not reporting the same way as the bandit tool itself and it is also not providing any fix recommendation (e.g. "Use defusedxml instead of lxml"). Is there a way to tweak that ?

Perhaps you are looking for the ` --show-source` flag?

---

_Label `question` added by @zanieb on 2023-11-17 15:59_

---

_Comment by @dga-nagra on 2023-11-20 12:23_

@zanieb  Thank you for the quick answer, I will first check the link and flag and come back to you right after.

---

_Comment by @dga-nagra on 2023-11-24 09:08_

Hi again @zanieb,

I was able to test/understand your answers.

For the `format` question: if I understand correctly, the formatter is doing changes by itself without considering the rules in the linter. The way the formatting is done may not correspond to the linter rules (e.g. suppose we have a linter ensuring that we never have a trailing comma and the formatter always add the trailing comma).
It is true that I initially though that the formatting was also following the linting rules, but I know wonder why isn't the formatter able to consider the linting rules ? It can detect the concern rules which are not that complicate, I assumed it would be able to adapt itself.


The `--show-source` flag is not providing the same feedbacks as the actual tools (this might be related to using `flake8-bandit` and not actually `bandit` directly)
![image](https://github.com/astral-sh/ruff/assets/147379886/1da775c0-96b3-45e6-86e9-6aeef4591c1f)

What I am missing is the details about the security issue and recommended fix. Is there a way for it?

Thank you again for your time.

---

_Comment by @MichaReiser on 2023-11-27 08:03_

> For the format question: if I understand correctly, the formatter is doing changes by itself without considering the rules in the linter. The way the formatting is done may not correspond to the linter rules (e.g. suppose we have a linter ensuring that we never have a trailing comma and the formatter always add the trailing comma).
It is true that I initially though that the formatting was also following the linting rules, but I know wonder why isn't the formatter able to consider the linting rules ? It can detect the concern rules which are not that complicate, I assumed it would be able to adapt itself.

Your understanding is correct. Rules are considered incompatible if the formatter may introduce new lint violations. 
The formatter isn't always compatible with the linter because it supports fewer options and is intentional opinionated about how code gets formatted. Supporting the exact formatting expected by the lint rules would require making the formatter more configurable, which isn't aligned with its philosophy and, in some cases, implementing the expected output in the formatter may not even be possible (E501 in cases where the line cannot be split without e.g. introducing a new temporary variable)

> The --show-source flag is not providing the same feedbacks as the actual tools (this might be related to using flake8-bandit and not actually bandit directly)
image

`--show-source` only shows the source that triggers the lint rule. We have plans to suggested fix (if available) by default and there should be an issue somewhere but haven't gotten around implementing it yet.

---

_Comment by @zanieb on 2023-11-27 15:23_

Glad the documentation was helpful!

See #7352 for showing fixes by default.

---

_Closed by @zanieb on 2023-11-27 15:23_

---

_Comment by @dga-nagra on 2023-11-29 11:14_

> `--show-source` only shows the source that triggers the lint rule. We have plans to suggested fix (if available) by default and there should be an issue somewhere but haven't gotten around implementing it yet.

Just to avoid any misunderstanding: I am not expecting ruff to do anything that is not done by the original tool. The fixes recommendations mentioned here only concerns bandit's output: the original tool is doing the recommendations.
This is not critical since I can just use bandit directly instead


Regarding the formatter, I didn't found much configuration options, but I wrongly assumed it was relying on the linting option and missed that it was meant to be **strongly** opinionated. I don't know if this information is easy to find.

Also, even if we can partially use ruff with another linter/formatter, I feel like ruff's format _should_ be able to match all the checks that it can define.

Thank you all for the responses.
Have a nice day,

---

_Comment by @kdebrab on 2024-07-16 13:32_

FWIW, below is the list of rules (I think) that can be ignored when using the formatter as they are fixed by (and in some cases, depending on the configuration, contradict with) the formatter:
"COM", "D201", "D203", "D204", "D206", "D300", "E1", "E2", "E3", "E502", "Q", "W1", "W2", "W3",

In other words, these rules fully overlap with the formatter.

"E501" is not contained in this list as it only partly overlaps with the formatter as explained in https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules.

Remark that "ISC001" and "ISC002" are listed in https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules because running the formatter may sometimes introduce these lint errors (in particular, when lines are combined into a single line by the formatter). However, they are not contained in above list as the formatter doesn't fix or contradict these lint errors and thus they are not overlapping.


---
