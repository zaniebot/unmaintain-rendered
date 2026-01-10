```yaml
number: 11002
title: Display enabled rules
type: issue
state: closed
author: BlindChickens
labels:
  - question
assignees: []
created_at: 2024-04-17T17:33:12Z
updated_at: 2024-04-18T00:09:14Z
url: https://github.com/astral-sh/ruff/issues/11002
synced_at: 2026-01-10T11:09:53Z
```

# Display enabled rules

---

_Issue opened by @BlindChickens on 2024-04-17 17:33_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "enabled rules"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Is there any way to just see the enabled rules in a list like this with name and code(s)?

```
('comparison-with-itself', 'R0124')
('comparison-of-constants', 'R0133')
('no-classmethod-decorator', 'R0202')
('no-staticmethod-decorator', 'R0203')
('property-with-parameters', 'R0206')
('consider-using-from-import', 'R0402')
('too-many-public-methods', 'R0904')
('too-many-return-statements', 'R0911')
('too-many-branches', 'R0912')
...
 ```
 
 I have seen the other discussions regarding issues resembling this that mention improving the categorisation of the rules etcetera.
 And all those improvements will be welcomed. But internally when linting there must be a list after config is parsed that can just be shard with me on the terminal :)
 
 If there is any way to get it that I am missing, please share.

---

_Renamed from "Ruff display enabled rules" to "Display enabled rules" by @BlindChickens on 2024-04-17 17:33_

---

_Comment by @MichaReiser on 2024-04-17 17:43_

You can run `ruff check --show-settings` It shows the entire resolved configuration and I think there's a `lint.rule` key somewhere

---

_Label `question` added by @zanieb on 2024-04-17 18:59_

---

_Comment by @BlindChickens on 2024-04-17 19:09_

> You can run `ruff check --show-settings` It shows the entire resolved configuration and I think there's a `lint.rule` key somewhere

Yes I've seen that. And the whole output is useful actually for everything it tells me.
But the output of the rules are displayed like this:

```
	LoggingStringFormat,
	LoggingPercentFormat,
	LoggingStringConcat,
	LoggingFString,
	LoggingWarn,
	LoggingExtraAttrClash,
	LoggingExcInfo,
	LoggingRedundantExcInfo,
	BadQuotesInlineString,
	BadQuotesMultilineString,
	BadQuotesDocstring,
	AvoidableEscapedQuote,
	UnnecessaryEscapedQuote,
	UnsortedImports,
	MissingRequiredImport,
```

Which I cannot even map to a rule in the output of the `ruff rule --output-format=json --all`. That would have sufficed, would just have had a piece of glue-code to get from the one to the other.

Let me also just add what I need it for. We want to move as much rules from pylint over to ruff. I have a script checking current enabled pylint rules, then checking in the full list of ruff rules if it is there, and then it prints it all in a list. This list is then something we can systematically take and enable in ruff, and disable in pylint.

And it will be nice if I can have the already enabled rules in ruff to better structure the output of the script. And for added sanity make sure there are no rules enabled in both.

---

_Comment by @zanieb on 2024-04-17 19:54_

Hi! I've opened a fix for this at https://github.com/astral-sh/ruff/pull/11003

---

_Closed by @zanieb on 2024-04-17 20:18_

---

_Comment by @BlindChickens on 2024-04-17 22:04_

Aah! This is perfect thanks. When does it release? ;)

---

_Comment by @zanieb on 2024-04-18 00:09_

Soon! We're planning on releasing 0.4.0 this week.

---
