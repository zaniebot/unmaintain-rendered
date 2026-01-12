```yaml
number: 4266
title: providing settings for rules which are not generated should produce a warning
type: issue
state: closed
author: joaompinto
labels:
  - configuration
assignees: []
created_at: 2023-05-07T08:13:03Z
updated_at: 2024-01-10T21:25:10Z
url: https://github.com/astral-sh/ruff/issues/4266
synced_at: 2026-01-12T15:54:44Z
```

# providing settings for rules which are not generated should produce a warning

---

_@joaompinto_


```toml
# pyproject.toml
[tool.ruff.mccabe]
max-complexity = 1

[tool.flake8]
max-complexity = 1
```` 

```python
# test.py
def some_function():
    if 0 > 1:
        print("1")
    else:
        print("2")
```
```sh
pip install flake8 Flake8-pyproject ruff

$ ruff test.py
`` 
$ flake8 test.py
test.py:5:1: C901 'some_function' is too complex (2)

$ ruff test.py # No output produced

$ ruff --version
ruff 0.0.265


---

_Comment by @charliermarsh on 2023-05-07 13:29_

Did you enable the C rule set, or C901 specifically? Unlike Flake8 we don’t turn that rule on by default.

---

_Comment by @joaompinto on 2023-05-07 13:49_

I do not have any experience with rule sets.
How do I "enable the C rule set" ?
In any case if the option itself does not have any effect, should the user not get a warning ? linting the config :) ?


---

_Comment by @charliermarsh on 2023-05-07 13:53_

There are a couple ways to do it, but they all require specifying some configuration for Ruff. You could run `ruff --select C901 test.py`, or add a `ruff.toml` file to your directory with:

```toml
select = ["E", "F", "C901"]
```


---

_Renamed from "max-complexity not being checked" to "providing settings for rules which are not generated should produce a warning" by @joaompinto on 2023-05-07 14:03_

---

_Comment by @joaompinto on 2023-05-07 14:04_

Thanks, adding the select got me the expected behaivor.
Renamed the bug for covering the case of inneffective settings not being reported.

---

_Label `configuration` added by @charliermarsh on 2023-05-08 01:28_

---

_Comment by @charliermarsh on 2023-05-08 01:29_

I'm uncertain on this, since it's reasonable to configure these settings, but only turn on rules conditionally. E.g., it'd be _reasonable_ to set a `max-complexity` in your configuration file, but not include `C901` in that file, and only run that check periodically from the CLI via `--select`. In which case, the warning would be a false positive.

---

_Comment by @Spectre5 on 2023-05-08 14:55_

Agreed - if this is added it should be under an optional setting or CLI flag and certainly not the default behavior.

---

_Comment by @joaompinto on 2023-05-08 17:19_

I guess the divergence here is what is the _reasonable_ use case that you want to prioritize.

I see two different reasonable cases:
1. A user like me, that from reading https://beta.ruff.rs/docs/settings/#max-complexity, and not being aware of the "select" filtering expects the corresponding rule to be effectively used - maybe it is just a matter of documentation, and making it clear that that such rule is not set by default ?
2. A user which is fully aware of selection filters and want's to provide some settings to be activated only under certin selections

Anyway, my problem is solved, was just trying to improve for other users that might follow my analisys patttern, feel free to close the issue if found to be unreasonable.

Thanks for developing Ruff and the quick support.


---

_Comment by @MichaReiser on 2023-05-09 06:39_

I think, in the long-term, it would help to move rule-specific settings next to the rule configuration

Instead of having a `select` configuration and the `max-complexity` configuration in two entirely different places, move the configuration of the rule next to where we enable the rule:

```
[tools.ruff.lints]

max-complexity = { level = "error", limit = 1 }
```

I believe that this should help because reading ` max-complexity = { level = "allow", limit = 1 }` would make it more apparent that you need to enable the rule.


> I'm uncertain on this, since it's reasonable to configure these settings, but only turn on rules conditionally. E.g., it'd be reasonable to set a max-complexity in your configuration file, but not include C901 in that file, and only run that check periodically from the CLI via --select. In which case, the warning would be a false positive.

Ideally, we should warn about unused configuration (or even disallow it by the schema). That's why I wonder if we could support those use cases by either:

* Adding a `--config` CLI option allowing to set any configuration option. This has the downside that the `complexity` limit is not checked in but it would enable the described workflow
* Ask users to commit two configurations, one where the complexity rule is enabled and one where it is not. Then allow them to specify the configuration with `--config-path`. This may not work well with extended configurations. 

---

_Comment by @charliermarsh on 2024-01-10 21:25_

I'm going to close this out for now since it's hard to act on given our current model for rules and settings. It may feed into changes we choose to make to our settings representation in the future though, as Micha mentioned above. Thank you :)

---

_Closed by @charliermarsh on 2024-01-10 21:25_

---
