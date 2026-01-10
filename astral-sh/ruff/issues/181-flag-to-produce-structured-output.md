```yaml
number: 181
title: Flag to produce structured output
type: issue
state: closed
author: HallerPatrick
labels: []
assignees: []
created_at: 2022-09-13T19:51:43Z
updated_at: 2022-09-14T14:43:43Z
url: https://github.com/astral-sh/ruff/issues/181
synced_at: 2026-01-10T15:56:05Z
```

# Flag to produce structured output

---

_Issue opened by @HallerPatrick on 2022-09-13 19:51_

Most linters have a flag (pylint: `-f`) that allows to definition of a structured format in which the listing errors are output. This allows the integration of `ruff` in editor plugins such as vscode (#118) and neovim. 

In the next days I would give it a go and would make a PR for this. 

---

_Comment by @charliermarsh on 2022-09-13 20:20_

Yeah happy to support this. Is there a common standard for how the output should be formatted? E.g. is it JSON? Is it expected to follow any specific schema?

---

_Label `enhancement` added by @charliermarsh on 2022-09-13 20:20_

---

_Comment by @Jackenmen on 2022-09-13 23:19_

I don't know about other tools but GitHub supports single-line (and partially multi-line) regexp matching with their GitHub Actions Problem Matchers:
https://github.com/actions/toolkit/blob/main/docs/problem-matchers.md

This should probably already work fine with ruff as its output is `filename:line:column:error message` which can be matched easily, assuming that ruff can guarantee that this format will be kept (or if it could give an option that will ensure that this format is always used). For ease of parsing, ruff should probably have some auto-detection of terminal color support which would make it easy to make such problem matchers.

If in the future, you would have something like a severity (warning vs error) similar to i.e. eslint, GitHub's problem matchers supports "warning" and "error" literal strings as severity and shows it accordingly in line annotations.

As an example, pylint gives a few examples of message templates that work well with other tooling:
https://pylint.pycqa.org/en/latest/user_guide/usage/output.html



---

_Comment by @HallerPatrick on 2022-09-14 07:44_

> Yeah happy to support this. Is there a common standard for how the output should be formatted? E.g. is it JSON? Is it expected to follow any specific schema?

I don't think there is a universal standard. But, IMO, having the linting errors in a JSON format is a safe bet, as every common language has no problem parsing it. Pylint supports text, json, parseable, colorized and msvs. Every further format can be implemented when there is a demand for it.

@jack1142 This is limited to Github Actions, so in the context of CI/CD? I am not sure if any editor supports regexp matching of output formats for parsing.

---

_Comment by @HallerPatrick on 2022-09-14 12:23_

Opened a PR. I encapsulated the write out logic into a own Printer struct. It is now easy to extend the printer with new formats through the `SerializationFormat` enum. 

One open question is: What is the output if `ruff` is in watch mode? Should it still output json, if set? For now it just writes to "normal" text to stdout and not json.

---

_Comment by @charliermarsh on 2022-09-14 12:44_

I think it's fine for ruff to throw a warning when a non-default format is combined with watch mode (and then ignore that format). It's similar behavior to `--autofix`.

---

_Closed by @charliermarsh on 2022-09-14 14:43_

---
