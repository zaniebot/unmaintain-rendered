```yaml
number: 21633
title: Add Ability to Output to Multiple Files/Formats
type: pull_request
state: closed
author: celestialorb
labels:
  - cli
  - needs-decision
  - needs-design
assignees: []
base: main
head: user/celestialorb/cli/output/format/multiple
created_at: 2025-11-25T23:23:52Z
updated_at: 2025-12-26T11:50:42Z
url: https://github.com/astral-sh/ruff/pull/21633
synced_at: 2026-01-12T15:57:29Z
```

# Add Ability to Output to Multiple Files/Formats

---

_@celestialorb_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
I have written many CI jobs where Ruff outputs in the GitLab format to a file, but this suppresses outputting to `stdout`/`stderr`. If Ruff finds an issue this will fail the CI job and it won't be immediately obvious to the developer what the lint finding was -- they'd have to go and crack open the GitLab JSON artifact, or view it in GitLab's merge request view.

This change allows us to write both a machine-readable artifact (e.g. GitLab JSON format) as well as output Ruff's findings in a human-readable format without needing to run it twice.

This PR should resolve #2388.

## Test Plan

<!-- How was it tested? -->
Tested locally, and hopefully tests will also be run in a CI pipeline.


---

_Review requested from @ntBre by @amyreese on 2025-11-26 01:49_

---

_Review requested from @MichaReiser by @amyreese on 2025-11-26 01:49_

---

_Label `cli` added by @amyreese on 2025-11-26 01:49_

---

_Label `needs-decision` added by @amyreese on 2025-11-26 01:50_

---

_Comment by @penguinolog on 2025-11-26 07:20_

Have to added context: GitLab comment code in PR only for the most expensive subscription and not all developers immediately rushing to read CodeQuality report.
Having issues displayed at CI CLI output will be serious time saver.

---

_Comment by @MichaReiser on 2025-11-26 07:51_

I'd like to explore some more designs on the CLI argument. I don't think what's recommended in this PR has good ergonomics. 

E.g. some alternatives:

* allow `--output-file` multiple times where the value can be `<path>` or `<format:path>`. 
* match `--output-file` with their corresponding `--output-format`: `--output-file path --output-format concise`: This might be tricky to implement and understand

What bothers me a bit is that `--output-format` mostly becomes irrelevant when using `--output-file=<format:path>` 

Have you looked at what other tools do (not just pylint)?


For example, this is what vitest does:

```
--outputFile <filename/-s>                            Write test results to a file when supporter reporter is also specified, use cac's dot notation for individual outputs of multiple reporters (example: --outputFile.tap=./tap.txt)
```

**golang CLI** 

```
golangci-lint run \
  --output.text.path=stdout \
  --output.json.path=./report.json \
  --output.checkstyle.path=./report.xml
```

We might also be able to get away with something easier because what I understand is that this is a CI specific issue and the only thing people need is to allow one output file, but keeping the default CLI output. I didn't see any request where someone actually wants to write 3 outputs at once.



---

_Label `needs-design` added by @MichaReiser on 2025-11-26 07:51_

---

_Comment by @celestialorb on 2025-11-26 19:04_

> I'd like to explore some more designs on the CLI argument. I don't think what's recommended in this PR has good ergonomics.
> 
> E.g. some alternatives:
> 
> * allow `--output-file` multiple times where the value can be `<path>` or `<format:path>`.
> * match `--output-file` with their corresponding `--output-format`: `--output-file path --output-format concise`: This might be tricky to implement and understand
> 
> What bothers me a bit is that `--output-format` mostly becomes irrelevant when using `--output-file=<format:path>`
> 
> Have you looked at what other tools do (not just pylint)?
> 
> For example, this is what vitest does:
> 
> ```
> --outputFile <filename/-s>                            Write test results to a file when supporter reporter is also specified, use cac's dot notation for individual outputs of multiple reporters (example: --outputFile.tap=./tap.txt)
> ```
> 
> **golang CLI**
> 
> ```
> golangci-lint run \
>   --output.text.path=stdout \
>   --output.json.path=./report.json \
>   --output.checkstyle.path=./report.xml
> ```
> 
> We might also be able to get away with something easier because what I understand is that this is a CI specific issue and the only thing people need is to allow one output file, but keeping the default CLI output. I didn't see any request where someone actually wants to write 3 outputs at once.

Yeah, the main use case here is to produce two outputs, a human-readable (`stdout/stderr`) report for developers and a machine-readable report for integration into development systems (e.g. GitHub, GitLab). I can't envision someone needing to produce multiple machine-readable reports of different formats in a single job/context.

What do you think of an approach where we tweak the behavior to _always_ produce a human-readable report in the console? The invocation might be akin to:
```
ruff check
  --output-console-format=concise
  --output-file-format=gitlab
  --output-file-path=./reports/codequality.json
```

We could also introduce a new console format to disable reporting findings out to the console, should that behavior be desired. Thus we'd have a few different options for this proposed `--output-console-format` parameter: `none`, `text`, `concise`, `full`, etc.

The minor downside here is that we're introducing new parameters and we'd probably want to deprecate the old ones if we're to use something like this.

---

_Comment by @MichaReiser on 2025-11-27 07:20_

I'd prefer not to introduce new parameters if possible. I also think that `--output-format-console` is too long for a parameter that isn't that uncommonly used. 

---

_Comment by @MichaReiser on 2025-11-27 20:44_

It might be worth taking a look at what basedpyright does here https://docs.basedpyright.com/latest/benefits-over-pyright/improved-ci-integration/

We also discussed in the past to automatically change the behavior based on wheter we dedect that ruff runs in CI (where the CI variable is set)

---

_Comment by @MichaReiser on 2025-12-26 11:50_

I'll close this PR as it has stalled. We can continue the design discussion on the issue. 

---

_Closed by @MichaReiser on 2025-12-26 11:50_

---
