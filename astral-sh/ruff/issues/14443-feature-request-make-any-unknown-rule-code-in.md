```yaml
number: 14443
title: "Feature request: Make *any* unknown rule code in \"ignore\" setting a warning, not an error"
type: issue
state: open
author: Avasam
labels:
  - configuration
assignees: []
created_at: 2024-11-18T20:10:32Z
updated_at: 2026-01-12T12:36:46Z
url: https://github.com/astral-sh/ruff/issues/14443
synced_at: 2026-01-12T12:59:48Z
```

# Feature request: Make *any* unknown rule code in "ignore" setting a warning, not an error

---

_Issue opened by @Avasam on 2024-11-18 20:10_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## What happens currently

https://github.com/astral-sh/ruff/pull/14435 closes #13505 but restricts itself to previously-known error codes.

For the sake of tracking, I'm letting the previous issue be closed, but opening this one as a follow-up to allow *any* error unknown error code in `ignore` setting to be a warning.

Take for example `DOC402` added in Ruff `0.5.7`. Ignoring `DOC402` in my shared configs means that my config it is no longer valid for previous versions of Ruff.

(everything below this line is identical to the previous issue description)

At the moment, ignoring a rule code that Ruff doesn't know about in the `ignore` setting will cause an error:
For example using `ruff==0.5.6`:
```toml
[lint]
select = ["ALL"]
ignore = [
  # ...
  "DOC201", # docstring-missing-returns
  "DOC402", # docstring-missing-yields
  "DOC501", # docstring-missing-exception
]
```
Results in:
```
ruff failed
  Cause: Failed to parse [...]]\ruff.toml
  Cause: TOML parse error at line 17, column 1
   |
17 | [lint]
   | ^^^^^^
Unknown rule selector: `DOC402`
```

## What I'd like to see
I'd like such errors, for `ignore` specifically, to be warnings instead

## Why ?
For shared configurations. Not completely failing on unknown ignored error codes allows the same configuration to be valid for a wider range of Ruff versions, reducing maintenance and increasing compatibility on the configuration side. Whether it's because the consumer has updated to a new Ruff version that removed that rule, or because they're still behind but the config disabled a rule they don't have access to yet.

Whilst rn shared configs are mostly done through copy-and-paste (https://github.com/BesLogic/shared-configs/blob/main/ruff.toml) or some sort of git management (https://github.com/jaraco/skeleton/blob/main/ruff.toml). This will become especially relevant with https://github.com/astral-sh/ruff/issues/12352

## Why only `ignore` ?
This request opens the question: why not do the same for rule selection or settings? Because those convey an explicit desire for the code to conform to those rules and configurations. Ignoring a rule, whether it exists or not, results in the same lint validation.

## Prior art
ESLint works very similarly to what I'm asking, with the only difference that it completely ignores the disabled rule, instead of warning.
The "enable all rules + disable what we don't want or need" pattern has allowed me with https://www.npmjs.com/package/eslint-config-beslogic to support a wide range of peer dependencies whilst staying mostly up to date with new rules (until I need to reconfigure a non-disabled newly added rule, but with ESLint's JS-based configs I have ways to work around that too with version-detection, although that's not relevant here)

Flake8 only errors-out on rule codes that are completely impossible. For example ignoring `AAAA` results in `ValueError: Error code 'AAAA' supplied to 'extend-ignore' option does not match '^[A-Z]{1,3}[0-9]{0,3}$'`

Ruff is already able to output warnings, for example when linting non-UTF-8 files:
```
warning: Failed to lint Pythonwin\pywin\test\_dbgscript.py: stream did not contain valid UTF-8
```



---

_Label `configuration` added by @dhruvmanila on 2024-11-19 03:15_

---

_Label `needs-decision` added by @MichaReiser on 2024-11-19 07:10_

---

_Comment by @MichaReiser on 2024-11-19 07:14_

Whether we want this behavior is less clear to me than #13505. I can see use cases where it is useful, but it requires that the configuration maintainer keeps their configuration in sync with new ruff releases and having users to disregard warnings seems non-ideal to me. 

A somewhat more involved alternative is that the preset explicitly lists the desired rules one by one. This way, there are no warnings whatsoever and upgrading ruff to a newer version doesn't require manual reviewing if new rules "sneaked in"

---

_Comment by @Avasam on 2024-11-19 15:57_

(btw feel free to close either version of this issue, so they don't stay duplicated)

---

_Comment by @MichaReiser on 2024-11-19 16:09_

I'd like to keep it open because it demonstrates a useful use case. I'm just not sure if the proposed solution is the right one or if there's another way of solving the problem of using the same configuration across multiple ruff versions.

---

_Comment by @tuttle on 2026-01-12 12:32_

We also use 

```
select = ["ALL"]
ignore = [
   ... list of not (yet) resolved rules ...
]
```


One of our developers has **Auto update** set on **the Ruff extension to VSCode**, he added the new rule from ruff 0.14.11 to the ignore list, pushed and CI failed with **Unknown rule selector: RUF067** on our CI.

It is convenient to have VSCode extensions up to date so the new ruff rules are used automatically, and the code base could become a bit better. But it collides with CI that uses locked package versions.

We would for example welcome a ruff cmd line option `--warn-about-unknown-ignored-rules` or something like that which we would add to the CI.

---

_Comment by @MichaReiser on 2026-01-12 12:36_

The way we solved this in ty is by making unknown rules a normal diagnostic with a warning severity. 

---

_Label `needs-decision` removed by @MichaReiser on 2026-01-12 12:36_

---
