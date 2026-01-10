```yaml
number: 2606
title: Allow explicit select of docstring rule to override configured convention
type: issue
state: closed
author: ngnpope
labels:
  - configuration
assignees: []
created_at: 2023-02-06T12:07:46Z
updated_at: 2024-08-15T09:04:30Z
url: https://github.com/astral-sh/ruff/issues/2606
synced_at: 2026-01-10T11:09:45Z
```

# Allow explicit select of docstring rule to override configured convention

---

_Issue opened by @ngnpope on 2023-02-06 12:07_

I know this is [documented](https://github.com/charliermarsh/ruff#does-ruff-support-numpy--or-google-style-docstrings)...

> Setting a `convention` force-disables any rules that are incompatible with that convention, no matter how they're provided, which avoids accidental incompatibilities and simplifies configuration.

...but it seems a little excessive in some cases.

In particular, `convention = "pep257"` disables `D212` and `D213`. [PEP 257](https://peps.python.org/pep-0257/#multi-line-docstrings:~:text=The%20summary%20line%20may%20be%20on%20the%20same%20line%20as%20the%20opening%20quotes%20or%20on%20the%20next%20line.) states:

> The summary line may be on the same line as the opening quotes or on the next line.

But I'd like to be a little stricter. I'd like to have `convention = "pep257"` and force enable `D213`. Not sure whether this is best as a new configuration option to make it something you have to explicitly opt in to, e.g. `force-enable = ["D213"]`?

_(Using v0.0.241.)_

---

_Comment by @charliermarsh on 2023-02-06 15:38_

I think this was overly aggressive. We can reconsider. It was done to avoid things like users accidentally enabling both `D212` and `D213`, which led to infinite autofix loops. But we since "solved" that with #2369. So we could instead treat conventions slightly differently.

---

_Label `configuration` added by @charliermarsh on 2023-02-06 15:38_

---

_Label `core` added by @charliermarsh on 2023-02-06 15:38_

---

_Label `core` removed by @charliermarsh on 2023-02-06 15:38_

---

_Comment by @charliermarsh on 2023-02-06 17:59_

It probably makes sense to treat `convention` as equivalent to inlining an `extend-select`.

---

_Comment by @ngnpope on 2023-02-06 18:37_

Aye, that sounds like how I expected it to work

---

_Comment by @smackesey on 2023-03-10 20:29_

Came here to say that I added `convention = "google"` but was unsure how it would interact with my existing set of `D` ignores. Went looking for answers under `convention` in [the docs](https://beta.ruff.rs/docs/settings/#convention) but didn't find anything. What's the current behavior?

---

_Comment by @charliermarsh on 2023-03-10 20:49_

The current behavior is that it turns off any rules that are incompatible with the convention.

So, specifically, `convention = "google"` is equivalent to ignoring `D203`, `D204`, `D213`, `D215`, `D400`, `D401`, `D404`, `D406`, `D407`, `D408`, `D409`, and `D413`.

So for Dagster, you can remove that first block of `(non-google docstring)` ignores. That is, I think you'd want:

```toml
[tool.ruff]
select = ["D"]
ignore = [
  # (missing public docstrings) These work off of the Python sense of "public", rather than our
  # bespoke definition based off of `@public`. When ruff supports custom plugins then we can write
  # appropriate rules to require docstrings for `@public`.
  "D100",
  "D101",
  "D102",
  "D103",
  "D104",
  "D105",
  "D106",
  "D107",

  ##### TEMPORARY DISABLES

  # (assorted docstring rules) There are too many violations of these to enable
  # right now, but we should enable after fixing the violations.
  "D200",  # (one-line docstring should fit)
  "D205",  # (blank line after summary)
  "D212",  # (multi-line docstring summary should start at 1st line)
  "D415",  # (docstring summary should end with a period)
  "D417",  # (missing arg in docstring)

  # (indent docstring rules) These rules should be enabled when ruff lands
  # on/off style comments, which would allow turning them off for specific
  # docstrings.
  "D207", # (under-indented docstring)
  "D208", # (over-indented docstring)
  "D402", # (first line should not be the function's "signature")
]

[tool.ruff.pydocstyle]
convention = "google"


---

_Comment by @ssbarnea on 2023-04-29 10:27_

The situation seem to be worse for those deciding to use `select = ["ALL"]` as apparently you get two warnings that you cannot silence in any way (adding to ignore does not work), neither trying to define `convention = "pep257"` - even if documentation states that it should work.

```
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
```

Does anyone know a way to keep using ALL and avoid these confusing warnings? 

---

_Comment by @charliermarsh on 2023-04-29 16:05_

@ssbarnea - Do you mind sharing your configuration? For me, I don't see any warnings with this `pyproject.toml`:

```toml
[tool.ruff]
select = ["ALL"]

[tool.ruff.pydocstyle]
convention = "pep257"
```

---

_Comment by @ssbarnea on 2023-05-16 16:19_

@charliermarsh Sure, look at https://github.com/ansible/ansible-compat/blob/main/pyproject.toml#L138 -- apparently I get this warning with current ruff too:

```
$ ruff .
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
 
$ ruff --version
ruff 0.0.267
```

---

_Comment by @charliermarsh on 2023-05-16 16:27_

@ssbarnea - Thanks, I'll take a look now.

---

_Comment by @charliermarsh on 2023-05-16 16:31_

I'm not seeing those warnings, need to think on why they could be showing up for you:

```
ansible-compat on  main via 🐍 v3.8.16
❯ ruff . --no-cache
ansible-compat on  main via 🐍 v3.8.16
❯ ruff --version
ruff 0.0.267
```

---

_Comment by @charliermarsh on 2023-05-16 16:33_

Could you share the output of `ruff --show-settings .`, if you have time?

---

_Comment by @ssbarnea on 2023-05-16 19:24_

Something is confusing ruff, and apparently is not the cache:
```
ssbarnea@m1: ~/c/a/ansible-compat chore/test
$ ruff .
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.

ssbarnea@m1: ~/c/a/ansible-compat chore/test
$ ruff --version
ruff 0.0.267

ssbarnea@m1: ~/c/a/ansible-compat chore/test
$ ruff --no-cache .
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
```

Result of settings dump https://gist.github.com/ssbarnea/18819983d585ad1ed674e1a2e2b49616 

---

_Comment by @charliermarsh on 2023-05-16 20:41_

Is there any way you have something like a `ruff.toml` or `.ruff.toml` or `pyproject.toml` in the filesystem (under `ansible-compat`) that's not tracked by Git? That _could_ trigger this message but wouldn't show up with `--show-settings`.

(Do you not see the warnings when you run `--show-settings`? Or were they omitted from the settings dump?)

---

_Comment by @ssbarnea on 2023-05-19 10:11_

You are right, I see the warning without even doing anything:
```
$ ruff --show-settings
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
error: Expected at least one path to search for Python files
FAIL: 2
```

I do not have any other ruff config file in my filesystem other than the git-tracked one at https://github.com/ansible/ansible-lint/blob/main/pyproject.toml#L225-L262

---

_Comment by @tlambert03 on 2023-06-30 10:38_

just wanna add my +1 to this request.  Specifically, I'd love to be able to simply add D417 to numpy docstrings.  

```py
[tool.ruff]
extend-select = ["D417"]
[tool.ruff.pydocstyle]
convention = "numpy"
```

Currently, I do this by manually including the full numpy include/exclude spec just so I can keep D417.

---

_Comment by @smackesey on 2023-07-16 20:30_

Revisiting to say that IMO the current docs are still too uninformative-- if I read https://beta.ruff.rs/docs/settings/#pydocstyle-convention it is not obvious what this does in terms of `select`/`ignore`.

If the content from https://github.com/astral-sh/ruff/issues/2606#issuecomment-1464451372 were posted in the docs that would go a ways.

---

_Comment by @charliermarsh on 2023-07-17 02:51_

For now, I expanded on the documentation in #5819.

---

_Comment by @alanhdu on 2023-11-09 03:21_

I've been taking a look at this and quite surprised by the behavior. From a code POV, I think the fix should be simple: changing
https://github.com/astral-sh/ruff/blob/4760af3dcbcf772fdcc1cae9aa9d0c4b8037c132/crates/ruff_workspace/src/configuration.rs#L891-L893

to something more like:

```rust
 for rule in convention.rules_to_be_ignored() { 
    if !rules.enabled(*rule) {
        rules.disable(*rule);
    }     
 } 
```

which would turn the rules off-by-default but allow users to force override it. My question is whether that would be the right path forward, or if we want something more complicated (e.g. if there are rules that are truly incompatible with a convention, then we can distinguish `convention.incompatilbe_rules()` from `convention.off_by_default_rules()`. @charliermarsh -- do you have any guidance on what the desired outcome here is? I'm happy to send a PR either way.

(I also have, uestions aboutf where these rules for the conventions came from -- I would've expected several of them to have been *on* by default for the numpy convention at least, but I'm happy to move that out of this issue's discussion).


---

_Comment by @charliermarsh on 2023-11-09 05:00_

@alanhdu -- Thanks for taking a look! I think the fix isn't quite that simple. We probably need some logic like: if the rule wasn't _explicitly_ selected (like `--select D200`, as opposed to `--select D`), then we should force it off. For each `rules_to_be_ignored`, we could check if the rule was included verbatim in any of the `select` or `extend_select` selectors?


---

_Comment by @alanhdu on 2023-11-09 17:42_

Yeah, I agree this is a little trickier than I first thought. I filed https://github.com/astral-sh/ruff/pull/8586 with what I think is a reasonable approach -- would be happy to discuss more about the implementation there!

---

_Comment by @charliermarsh on 2023-11-09 19:31_

Nice, thanks @alanhdu! Will take a look at that PR later today or tomorrow.

---

_Closed by @charliermarsh on 2023-11-10 18:47_

---

_Comment by @tlambert03 on 2023-11-10 18:54_

hooray!!  Thanks @alanhdu!

---

_Comment by @sorenmc on 2024-08-15 09:04_

For anyone else wanting to add D417 while using numpy convention you have to explicitly include it as

```toml
[tool.ruff.lint]
extend-select = ["D", <other selections>, "D417"]

[tool.ruff.lint.pydocstyle]
convention = "numpy" 
```



---
