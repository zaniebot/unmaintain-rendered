```yaml
number: 12502
title: "--disable-noqa unsupported / Code quality statistics?"
type: issue
state: closed
author: FrederikLauber
labels:
  - question
assignees: []
created_at: 2024-07-25T06:06:16Z
updated_at: 2024-07-25T08:16:13Z
url: https://github.com/astral-sh/ruff/issues/12502
synced_at: 2026-01-12T15:54:51Z
```

# --disable-noqa unsupported / Code quality statistics?

---

_@FrederikLauber_

Hi,

I am experimenting with Ruff to speed up our Build process for a python app.
As of now, we are using flake8 and run it twice, once without --disable-noqa and once with.
The idea is, that the first run makes sure that all issues have been checked by a human and the second run gives us statistics about the 
codebase.

Switching to ruff, --disable-noqa seems to not be supported.
Is there a way around this?
The obious way would be to support --disable-noqa. Alternativly, is there a way to get statistics abhout the usage of noqa throughout the code base?
We are basically using it as a metric for Code quality as people tendet to overuse noqa in the past quite alot.

Thanks in advance!



---

_Comment by @MichaReiser on 2024-07-25 06:16_

Hi

There's a `--ignore-noqa` option that makes ruff ignore all noqa comments. You can combine it with `--statistics` to get some numbers of the violations in your code. 

```shell
❯ uvx ruff check --ignore-noqa --statistics
warning: `uvx` is experimental and may change without warning
Resolved 1 package in 3ms
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
12	CPY001	[ ] missing-copyright-notice
 7	D100  	[ ] undocumented-public-module
 6	D101  	[ ] undocumented-public-class
 5	ANN201	[ ] missing-return-type-undocumented-public-function
 4	F401  	[*] unused-import
 3	W292  	[*] missing-newline-at-end-of-file
 3	W391  	[*] too-many-newlines-at-end-of-file
 3	D104  	[ ] undocumented-public-package
 2	ERA001	[*] commented-out-code
 2	ANN001	[ ] missing-type-function-argument
 2	ARG001	[ ] unused-function-argument
 2	I001  	[*] unsorted-imports
 1	E261  	[*] too-few-spaces-before-inline-comment
 1	E302  	[*] blank-lines-top-level
 1	D103  	[ ] undocumented-public-function
 1	F811  	[*] redefined-while-unused
 1	PGH004	[ ] blanket-noqa
```

---

_Label `question` added by @MichaReiser on 2024-07-25 06:16_

---

_Comment by @FrederikLauber on 2024-07-25 08:16_

Thanks, that seems to do exactly what I need. 

---

_Closed by @FrederikLauber on 2024-07-25 08:16_

---
