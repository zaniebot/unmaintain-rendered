```yaml
number: 967
title: "Discussion: check code representations"
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2022-11-30T03:02:31Z
updated_at: 2022-12-16T05:26:19Z
url: https://github.com/astral-sh/ruff/issues/967
synced_at: 2026-01-12T15:54:40Z
```

# Discussion: check code representations

---

_@charliermarsh_

Ruff was designed with Flake8-compatibility in mind: it uses the same check code representation (e.g., `F841` and friends), it supports many of the same command-line arguments, it supports the same `# noqa` syntax, etc. I consider this a strength, as it makes it much easier for existing projects to migrate to Ruff, and for users to familiarize themselves with Ruff's conventions.

That said, there's one area where I'd like to at least _consider_ diverging: the check code representations.

Flake8 uses a check code system in which checks have a one-to-three character alphabetical prefix, like `F` or `B` or `ANN`, which is then followed by exactly three digits. You can enable and disable individual codes, or classes of codes, with (e.g.) `--select F` or `--select F5`, and so on.

In general, I see the benefits of this system as:

- Hierarchical encoding enables straightforward and compact configuration (e.g., you can enable and disable entire classes of checks based on a prefix).
- Hierarchical encoding conveys some semantic information from the check code (e.g., if a check starts with `ANN`, you know it's annotation-related).
- Syntax is very compact (e.g., `# noqa: F841`).
- (For _Ruff_ specifically: makes it much easier for users to migrate from Flake8.)

Meanwhile, the weaknesses:

- Lack of human readability: `F841` on its own doesn't tell you anything about the check itself, only its (broad) categorization. This is in contrast to Clippy (and ESLint), which uses names like `approx_constant`. Pylint uses a hybrid scheme, with names and codes like [`bad-builtin` / `W0141`](https://pylint.pycqa.org/en/latest/user_guide/messages/warning/bad-builtin.html).
- Collisions across plugins: prefixes aren't guaranteed to be globally unique. The McCabe complexity check uses C901, while `flake8-comprehensions` uses the `C` prefix too. So `--select C` enables checks from multiple plugins.

I'd like to get some feedback from the community on how they feel about the current representation, and what they'd like to see if the representation were to change. Note that, alongside any _breaking_ changes, I'd like to ship tooling to automatically upgrade across versions of Ruff (e.g., rewrite check codes, or whatever else is needed).

To give us a jumping-off point, here's one rough proposal:

1. Introduce a short-name representation for every check (like `bad-builtin`), and allow users to provide short-names and codes interchangeably.
2. Require that all plugins use a unique prefix code, even if that requires deviating from the prefix code used in Flake8 (e.g., change the comprehensions prefix to `COM`). Err on the side of three-letter codes, with a few exceptions.

It'd also be entirely reasonable for us to say that the current system is fine, and we don't really care about changing it.


---

_Comment by @ljodal on 2022-12-03 19:20_

I think short codes would be useful, even though that'll quickly lead to very long lines. I think I'd prefer that though. Eg with mypy I tend to write `# type:ignore[error-code]`

---

_Comment by @smackesey on 2022-12-05 02:34_

Pylint/ESlint's error codes are vastly more user-friendly than the arbitrary numerical codes of flake8/ruff. IMO this is a no-brainer, I don't understand how flake8 has gone on so long without it:

>Introduce a short-name representation for every check (like bad-builtin), and allow users to provide short-names and codes interchangeably.

---

_Comment by @charliermarsh on 2022-12-05 02:36_

We actually already have a short-name internally for every check (`UnusedLoopControlVariable`, `StripWithMultiCharacters`, etc.) so it's something we're already "maintaining".

---

_Closed by @charliermarsh on 2022-12-16 05:26_

---
