```yaml
number: 7506
title: "Deprecate `PGH001` in favor of `S307`"
type: pull_request
state: closed
author: charliermarsh
labels:
  - rule
  - breaking
assignees: []
base: main
head: charlie/PGH001
created_at: 2023-09-19T03:32:55Z
updated_at: 2023-11-30T22:20:58Z
url: https://github.com/astral-sh/ruff/pull/7506
synced_at: 2026-01-12T15:55:24Z
```

# Deprecate `PGH001` in favor of `S307`

---

_@charliermarsh_

## Summary

These rules are identical. Bandit is the more popular and more well-defined category, so we remove `PGH001` and add a redirect to `S307`. (By adding the redirect, we ensure that users see a warning but not an error when they reference `PGH001` in their configuration or in a `# noqa`, and that those codes seamlessly redirect to `S307`.)

Closes https://github.com/astral-sh/ruff/issues/7283.

---

_Label `rule` added by @charliermarsh on 2023-09-19 03:32_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-09-19 03:33_

---

_@dhruvmanila approved on 2023-09-19 03:38_

---

_Comment by @github-actions[bot] on 2023-09-19 03:49_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -3, 0 error(s))

<details><summary>disnake (+0, -1)</summary>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/1f871ff51f9583881b2ad51a6a9aa0d70dafe9dd/disnake/utils.py#L1137'>disnake/utils.py:1137:21:</a> S307 Use of possibly insecure function; consider using `ast.literal_eval`
</pre>

</p>
</details>
<details><summary>airflow (+0, -1)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/c55fd77f76aafc76463e3dd2a6ecaa29e56bd967/dev/stats/get_important_pr_candidates.py#L131'>dev/stats/get_important_pr_candidates.py:131:32:</a> PGH001 No builtin `eval()` allowed
</pre>

</p>
</details>
<details><summary>content (+0, -1)</summary>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/d3c24571dfedc1a124a5b49c470932b340ff19ef/Packs/Malwr/Integrations/Malwr/Malwr.py#L96'>Packs/Malwr/Integrations/Malwr/Malwr.py:96:18:</a> PGH001 No builtin `eval()` allowed
</pre>

</p>
</details>
<details><summary>ibis (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/36270a994a5009732e7dc13d1737942551384ec1/ibis/common/typing.py#L197'>ibis/common/typing.py:197:69:</a> RUF100 Unused `noqa` directive (non-enabled: `S307`)
</pre>

</p>
</details>
Rules changed: 3

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PGH001 | 2 | 0 | 2 |
| S307 | 1 | 0 | 1 |
| RUF100 | 1 | 1 | 0 |



---

_Label `breaking` added by @MichaReiser on 2023-09-19 06:12_

---

_Comment by @MichaReiser on 2023-09-19 06:12_

Is this a breaking change? What do we need to document in the changelog?

---

_@konstin approved on 2023-09-19 06:56_

---

_Comment by @charliermarsh on 2023-09-19 19:29_

Yes and no...

If users have `PGH001` explicitly selected or ignored in their configuration file, we'll show a warning but automatically redirect it to `S307`.

If users have `# noqa: PGH001`, we'll similarly warn-and-redirect to `S307`.

The main _problem_ here is that if users have `PGH` in their `select`, but _not_ `S`, this rule will no longer run on their code. That's kind of an issue, because the rule is basically being silently turned off (and is the source of the ecosystem changes).

I'm not sure how best to resolve -- any ideas?

---

_Comment by @charliermarsh on 2023-09-27 04:37_

@zanieb - This one actually might be interesting to you too (especially my comment about regarding redirects).

---

_Comment by @Skylion007 on 2023-09-30 15:58_

We really should implement proper rule aliasing, this getting more and more unwieldy as we add new rules. It's really surprising when a new user migrates a flake8-plugin to ruff only to find they also have to enable a bunch of rules in different other categories to make functionality of the original plugin.

---

_Comment by @zanieb on 2023-09-30 16:03_

> The main problem here is that if users have PGH in their select, but not S, this rule will no longer run on their code. That's kind of an issue, because the rule is basically being silently turned off

This sounds like a breaking change to me. Proper aliasing does sound like the better solution and probably something we need for recategorization anyway?

---

_Comment by @Skylion007 on 2023-09-30 16:08_

@zanieb Agree, public rule aliasing is probably one of the largest painpoints right now and it will only become a bigger maintenance burden on us later the more rules get added without this feature.

---

_Comment by @charliermarsh on 2023-09-30 16:59_

Separate from the larger conversation around rule aliasing (see my comment in the related issue), we should consider removing this whole category and marking it as a breaking change. There are so few rules, multiple of them are duplicates of other existing rules, and there's no semantic relationship _between_ the various rules.


---

_Comment by @charliermarsh on 2023-09-30 17:01_

> Proper aliasing does sound like the better solution and probably something we need for recategorization anyway?

I think there are ways for us to do a recategorization without aliasing, because it doesn't require that we support _multiple_ codes at once for a given rule, only that we provide tooling to remap from existing to updated codes and categories.

---

_Comment by @zanieb on 2023-11-10 22:17_

Should we remove the whole category instead of this pull request? Either way I think we should probably gate removal or this alias with preview.

---

_Comment by @charliermarsh on 2023-11-10 22:31_

I’m in favor of that.

---

_Comment by @zanieb on 2023-11-30 22:20_

Closing in favor of https://github.com/astral-sh/ruff/issues/8931

---

_Closed by @zanieb on 2023-11-30 22:20_

---
