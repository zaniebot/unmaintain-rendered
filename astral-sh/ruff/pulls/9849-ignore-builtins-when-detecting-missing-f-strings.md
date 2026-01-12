```yaml
number: 9849
title: Ignore builtins when detecting missing f-strings
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/b
created_at: 2024-02-06T00:23:21Z
updated_at: 2024-02-06T04:49:57Z
url: https://github.com/astral-sh/ruff/pull/9849
synced_at: 2026-01-12T15:55:30Z
```

# Ignore builtins when detecting missing f-strings

---

_@charliermarsh_

## Summary

Reported on Discord: if the name maps to a builtin, it's not bound locally, so is very unlikely to be intended as an f-string expression.

---

_Review requested from @snowsignal by @charliermarsh on 2024-02-06 00:23_

---

_Label `bug` added by @charliermarsh on 2024-02-06 00:23_

---

_Comment by @ThiefMaster on 2024-02-06 00:26_

Some real code to test against (I think in all 3 cases it was triggered because of builtins):

- https://github.com/indico/indico/blob/c4e45df45639efd6744307780189c4a3d9b134a5/indico/web/forms/validators.py#L346-L357
- https://github.com/indico/indico/blob/c4e45df45639efd6744307780189c4a3d9b134a5/indico/modules/events/management/program_codes.py#L50
- https://github.com/indico/indico/blob/c4e45df45639efd6744307780189c4a3d9b134a5/indico/util/mdx_latex.py#L394

Errors with the current stable release:

```
[adrian@eluvian:~/dev/indico/py3/src:master $]> ruff check --select RUF027
indico/modules/events/management/program_codes.py:50:21: RUF027 Possible f-string without an `f` prefix
indico/util/mdx_latex.py:393:23: RUF027 Possible f-string without an `f` prefix
indico/web/forms/validators.py:350:36: RUF027 Possible f-string without an `f` prefix
indico/web/forms/validators.py:351:36: RUF027 Possible f-string without an `f` prefix
indico/web/forms/validators.py:353:36: RUF027 Possible f-string without an `f` prefix
indico/web/forms/validators.py:354:36: RUF027 Possible f-string without an `f` prefix
indico/web/forms/validators.py:356:29: RUF027 Possible f-string without an `f` prefix
Found 7 errors.
No fixes available (7 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

---

_Comment by @charliermarsh on 2024-02-06 00:31_

I think we should also ignore `ngettext` calls as you mention, but this would've prevented those false positives 👍 

---

_Comment by @github-actions[bot] on 2024-02-06 00:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -36 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/8baee5def5ab9a592c68f5f9b3ad0688ef329493/pandas/tests/indexes/base_class/test_formats.py#L138'>pandas/tests/indexes/base_class/test_formats.py:138:35:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/pandas-dev/pandas/blob/8baee5def5ab9a592c68f5f9b3ad0688ef329493/pandas/tests/indexes/base_class/test_formats.py#L141'>pandas/tests/indexes/base_class/test_formats.py:141:16:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -34 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/management/commands/edit_linkifiers.py#L14'>zerver/management/commands/edit_linkifiers.py:14:12:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/migrations/0423_fix_email_gateway_attachment_owner.py#L93'>zerver/migrations/0423_fix_email_gateway_attachment_owner.py:93:21:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/openapi/curl_param_value_generators.py#L281'>zerver/openapi/curl_param_value_generators.py:281:9:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/openapi/python_examples.py#L454'>zerver/openapi/python_examples.py:454:25:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/openapi/python_examples.py#L469'>zerver/openapi/python_examples.py:469:25:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_audit_log.py#L914'>zerver/tests/test_audit_log.py:914:26:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_audit_log.py#L974'>zerver/tests/test_audit_log.py:974:29:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_event_system.py#L762'>zerver/tests/test_event_system.py:762:29:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_events.py#L2747'>zerver/tests/test_events.py:2747:15:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_markdown.py#L2506'>zerver/tests/test_markdown.py:2506:24:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_markdown.py#L2604'>zerver/tests/test_markdown.py:2604:24:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_markdown.py#L2892'>zerver/tests/test_markdown.py:2892:24:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_markdown.py#L2962'>zerver/tests/test_markdown.py:2962:24:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_message_dict.py#L246'>zerver/tests/test_message_dict.py:246:24:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L101'>zerver/tests/test_realm_linkifiers.py:101:32:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L134'>zerver/tests/test_realm_linkifiers.py:134:32:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L164'>zerver/tests/test_realm_linkifiers.py:164:29:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L180'>zerver/tests/test_realm_linkifiers.py:180:29:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L186'>zerver/tests/test_realm_linkifiers.py:186:29:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L203'>zerver/tests/test_realm_linkifiers.py:203:29:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L216'>zerver/tests/test_realm_linkifiers.py:216:32:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L24'>zerver/tests/test_realm_linkifiers.py:24:29:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L319'>zerver/tests/test_realm_linkifiers.py:319:33:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L323'>zerver/tests/test_realm_linkifiers.py:323:33:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L327'>zerver/tests/test_realm_linkifiers.py:327:33:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L37'>zerver/tests/test_realm_linkifiers.py:37:48:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L63'>zerver/tests/test_realm_linkifiers.py:63:32:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L69'>zerver/tests/test_realm_linkifiers.py:69:32:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L75'>zerver/tests/test_realm_linkifiers.py:75:32:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L82'>zerver/tests/test_realm_linkifiers.py:82:13:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L89'>zerver/tests/test_realm_linkifiers.py:89:32:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zerver/tests/test_realm_linkifiers.py#L95'>zerver/tests/test_realm_linkifiers.py:95:32:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zilencer/management/commands/populate_db.py#L832'>zilencer/management/commands/populate_db.py:832:17:</a> RUF027 Possible f-string without an `f` prefix
- <a href='https://github.com/zulip/zulip/blob/49e3e6da062c6d9d9f336935493d3c3131066893/zilencer/management/commands/populate_db.py#L838'>zilencer/management/commands/populate_db.py:838:17:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF027 | 36 | 0 | 36 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-02-06 00:38_

Ecosystem checks look like improvements to me.

---

_@snowsignal approved on 2024-02-06 04:23_

Looks good to me! I'm really happy to see fewer false positives in the ecosystem check.

---

_Merged by @charliermarsh on 2024-02-06 04:49_

---

_Closed by @charliermarsh on 2024-02-06 04:49_

---

_Branch deleted on 2024-02-06 04:49_

---
