```yaml
number: 9186
title: "docstring code formatter: add a `--docstring-code-format` parameter to `ruff format` CLI"
type: issue
state: closed
author: kdeldycke
labels:
  - formatter
  - wish
assignees: []
created_at: 2023-12-18T15:46:02Z
updated_at: 2024-02-09T21:56:38Z
url: https://github.com/astral-sh/ruff/issues/9186
synced_at: 2026-01-12T15:54:49Z
```

# docstring code formatter: add a `--docstring-code-format` parameter to `ruff format` CLI

---

_@kdeldycke_

Following the implementation of experimental docstring format, a `docstring-code-format` configuration "knob" has been added in #8854. That way you can activate this new behavior by adding the following to `ruff.toml` (or any equivalent):

```toml
[format]
docstring-code-format = true
```

What I would like to have is a `--docstring-code-format` CLI option that I can use like so:

```shell-session
$ ruff format --docstring-code-format .
```

Why? Because I would like to test the docstring formatter on some projects, by the way of reuseable GitHub actions. And for these projects, I do not have the authority on the configuration files. I can only alter CLI parameters of ruff in the actions.

Is my proposal reasonable? Or is there a way I can pass arbitrary local configuration parameters via the CLI?

---

_Renamed from "docstring code formatter: add a `--docstring-code-format` parameter to the CLI" to "docstring code formatter: add a `--docstring-code-format` parameter to `ruff format` CLI" by @kdeldycke on 2023-12-18 15:46_

---

_Label `question` added by @MichaReiser on 2023-12-19 08:40_

---

_Label `formatter` added by @MichaReiser on 2023-12-19 08:40_

---

_Comment by @MichaReiser on 2023-12-19 09:56_

Hy @kdeldycke

Thanks for opening this issue and explaining your use case. 

We've been hesitant to expose all formater options because the main benefit of a formatter is consistency and it requires that all users use the same configuration, ideally commited in the repository. We only support `--line-length` today (see https://github.com/astral-sh/ruff/pull/8363). But we understand that there are use cases where providing the option via CLI is useful, your use case is one of them. Our preferred solution is that ruff has a generic way to set an arbitrary configuration value. It encourage using configuration files over using CLI options but enables use cases like yours. See https://github.com/astral-sh/ruff/issues/8368

Unfortunately, we haven't made progress with implementing a generic way to pass option values yet which, understandably, is unsatisfying for users. 

I don't think there's a good workaround for use case at the moment. You could specify another configuration but it would override all other settings too (and disables the automatic configuration discovery).

Related to:

* https://github.com/astral-sh/ruff/issues/8737

---

_Label `question` removed by @MichaReiser on 2023-12-19 09:58_

---

_Label `question` added by @MichaReiser on 2023-12-19 09:58_

---

_Label `question` removed by @MichaReiser on 2023-12-22 06:16_

---

_Label `wish` added by @MichaReiser on 2023-12-22 06:16_

---

_Comment by @kdeldycke on 2024-01-04 12:47_

Thanks @MichaReiser for the detailed explanation of the current state of the CLI. I totally understand the priority and decision of my feature request. There are things way more important to focus on.

In the mean time, I hacked my way into generating a [custom local `ruff.toml` file](https://github.com/kdeldycke/workflows/blob/dcc778a4534e417b23042867d9fb402ad4ed8568/.github/workflows/autofix.yaml#L85-L131) to feed to my CLI.

---

_Comment by @kdeldycke on 2024-01-05 07:40_

Another use case to consider, is using CLI parameters to setting up [Ruff VScode extension](https://marketplace.visualstudio.com/items?itemName=charliermarsh.ruff):

![Screenshot 2024-01-05 at 11 37 46](https://github.com/astral-sh/ruff/assets/159718/1c0a8f6b-18ad-4581-8634-c7b1a3021e08)


---

_Comment by @MichaReiser on 2024-02-06 19:09_

This will be supported once https://github.com/astral-sh/ruff/issues/9186 is merged

---

_Closed by @AlexWaygood on 2024-02-09 21:56_

---
