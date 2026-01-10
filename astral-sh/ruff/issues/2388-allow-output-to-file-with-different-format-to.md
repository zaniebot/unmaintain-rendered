---
number: 2388
title: Allow output to file with different format to output to console
type: issue
state: open
author: ngnpope
labels:
  - cli
  - wish
assignees: []
created_at: 2023-01-31T11:42:16Z
updated_at: 2025-11-12T22:07:07Z
url: https://github.com/astral-sh/ruff/issues/2388
synced_at: 2026-01-10T01:22:40Z
---

# Allow output to file with different format to output to console

---

_Issue opened by @ngnpope on 2023-01-31 11:42_

Currently, when running in CI I have to do the following, e.g. in a (simplified) `.gitlab-ci.yml`:

```yaml
lint:
  script:
    - ...
    - ruff .
    - ruff --format=gitlab . > quality.json
  artifacts:
    reports:
      codequality: quality.json
```

I'd like to keep the text output in the job output, but also make use of the capabilities GitLab provides to display violations.

Now, `ruff` is super fast ⚡, so running twice isn't a pain, but it seems suboptimal. Could we enable one of the following?

- `ruff --format=text --output=gitlab:/path/to/quality.json .` ← dedicated `--output` option
- `ruff --format=text --format=gitlab:/path/to/quality.json .` ← allow multiple `--format` with filename
- `ruff --format=text,gitlab:/path/to/quality.json .` ← allow multiple values in `--format`

---

_Comment by @not-my-profile on 2023-01-31 12:16_

I'd rather avoid CLI options with such a complex syntax, since it complicates the CLI and also doesn't work with the shell autocompletion generation by `clap_complete`.

How about instead introducing a new `--stderr-format` option that accepts the same values as the current `--format` option and would additionally output the specified format on stderr? ... then you could replace your two commands with `ruff --stderr-format=gitlab . 2> quality.json` (`2>` is shell syntax for stderr redirection).

---

_Comment by @ngnpope on 2023-01-31 12:31_

I guess we could do that, but that would only work as long as `ruff` never chose to output something else to stderr...

It seems a little irregular. Normally stdout is used for the normal output from a program and stderr is use to output errors or diagnostic information about the execution of that program.

---

_Comment by @not-my-profile on 2023-01-31 12:48_

Fair point ... and it also wouldn't support outputting to more than two formats. I think it's probably best to just implement a new `convert` subcommand that can consume the `json` output and convert it to any of the other supported output formats ... although for that to work nice with shell pipes we'd need a stream-friendly JSON format (where each message is on one line).

---

_Comment by @ngnpope on 2023-01-31 13:09_

So `--format=jsonl` then? (See https://jsonlines.org/)

I guess that allows for using `tee` to redirect to multiple processors then?

It would work, but does just seem much more complex than a command line option... 🤔 

---

_Comment by @not-my-profile on 2023-01-31 13:27_

The advantages I see are that 1) it wouldn't complicate the `ruff check` CLI for users who don't need this feature, 2) shell completion would work fine and 3) it's not unthinkable that we end up introducing more format-specific options (for `--format text` we already have several), having one ruff command call always only produce one output avoids the ambiguity of which output option belongs to which output.

I agree that how to use this would be less obvious for users unfamiliar with shell piping/redirection, however I think that could be accommodated via an example in the documentation ... and I wouldn't consider this to be something users commonly use but rather something you set up once for a CI, so I think that should be alright.

---

_Comment by @ekbfh on 2023-03-20 16:44_

Also think about this situation
Is it possible to have: 
1) always have stdout with some default format, ex. `grouped/text`
2) if `--outputfile=/tmp/file` than apply `--format` to file, not to stdout
3) disable stdout with `-q`, like it should be

I prefer not to run ruff more than one time and not to have any `|jq` processors 

---

_Comment by @MichaReiser on 2023-03-20 17:15_

Having a dedicated `output` (or `output-file`) option has precedence in 

* ESLint (`output-file` | `-o`)
* `dotnet format --report`
* Jest (`--outputFile`) in combination with `--json`

---

_Comment by @charliermarsh on 2023-03-20 19:09_

Ah good call. I'd want to look at what ESLint does in various cases. If you use `--fix`, does it write the fixed code to the output file? (Does it write the fixed code to stdout, if you write via stdin? I never checked that). What should go to stderr vs. stdout? Etc.

---

_Label `cli` added by @charliermarsh on 2023-03-22 03:35_

---

_Label `wishlist` added by @charliermarsh on 2023-03-22 03:35_

---

_Referenced in [astral-sh/ruff#4754](../../astral-sh/ruff/issues/4754.md) on 2023-05-31 19:01_

---

_Comment by @charliermarsh on 2023-06-20 16:53_

We now support writing to a file (#4950), but not to both standard output _and_ a file. I'm going to close this, and we can open a fresh issue if that additional behavior is desired.

---

_Closed by @charliermarsh on 2023-06-20 16:53_

---

_Comment by @ngnpope on 2023-06-22 09:25_

@charliermarsh Could you re-open this? I'm not sure why it makes sense to create a new issue when it would be exactly the same description... This issue was clearly stated as allowing the usual text output to standard output **while also** allowing a separate output to file for a specific report format.

The use case in question is for runs in CI where we want the usual report to the job output but would also like to generate a report file, e.g. for GitLab, so that artifact can be used to integrate the results with their UI. _(This could/would also apply for services other than GitLab.)_ Currently this is impossible to achieve without running `ruff` twice. 

---

_Comment by @ngnpope on 2023-06-22 09:27_

I should follow up that, while the PR you linked to (#4950) does support writing to an output file with a flag, it doesn't do anything other than remove the need for shell redirection.

---

_Comment by @MichaReiser on 2023-06-22 09:34_

CC: @dhruvmanila 

---

_Reopened by @MichaReiser on 2023-06-22 09:34_

---

_Comment by @mgorfer on 2023-08-10 01:37_

I would also be very glad to have this feature included in Ruff (https://github.com/astral-sh/ruff/discussions/6467).

---

_Assigned to @zanieb by @zanieb on 2023-08-14 14:23_

---

_Unassigned @zanieb by @zanieb on 2023-10-26 13:45_

---

_Referenced in [astral-sh/ruff#8251](../../astral-sh/ruff/issues/8251.md) on 2023-10-26 13:46_

---

_Comment by @ekbfh on 2024-02-12 11:04_

@charliermarsh Good day! Is any new thoughts about CI usage of ruff? We stiil have to run it twice in a row

---

_Referenced in [astral-sh/ruff#12237](../../astral-sh/ruff/issues/12237.md) on 2024-07-08 07:50_

---

_Comment by @gmetzker-4c on 2024-08-19 18:25_

Agreed this would be a nice feature.  
Pylint now supports multiple output formats as follows:  
`pylint --output-format=colorized,parseable:pylint.log`

That's not to say their syntax is the best but the ability target multiple outputs would be great for CI systems.


---

_Referenced in [astral-sh/ruff#13997](../../astral-sh/ruff/issues/13997.md) on 2024-10-30 13:51_

---

_Referenced in [astral-sh/ruff#14878](../../astral-sh/ruff/issues/14878.md) on 2024-12-09 15:46_

---

_Comment by @MarcBresson on 2025-02-05 14:22_

Hello, any news on that issue ?

---

_Comment by @wkoot on 2025-11-12 22:07_

I would like to use ruff to replace pylint in a pipeline, but having to run several times seems a bit of a shame.
The sonar-scanner provides the option to pass a pylint report via `sonar.python.pylint.reportPaths`, which ruff can produce.
However, this would mean I'd need to run 3 times; one for immediate readable output, one for gitlab annotation and one for pylint.

I think that the use case is rather specific so I don't think making the cli overly complicated is needed, toml config should suffice.
For instance, something like this:
```toml
[tool.ruff]
output-format = "concise"

[tool.ruff.per-file-output]
ruff_gitlab.json = {output-format = "gitlab"}
ruff_junit.xml = {output-format = "junit"}
ruff_pylint.txt = {output-format = "pylint"}
```

---

_Referenced in [astral-sh/ruff#21633](../../astral-sh/ruff/pulls/21633.md) on 2025-11-25 23:23_

---

_Referenced in [astral-sh/ty#1691](../../astral-sh/ty/issues/1691.md) on 2025-12-01 07:10_

---
