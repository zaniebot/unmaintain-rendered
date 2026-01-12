```yaml
number: 5718
title: Improvements to the documentation
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: zb/docs-v
created_at: 2024-08-02T02:15:11Z
updated_at: 2024-08-03T13:41:34Z
url: https://github.com/astral-sh/uv/pull/5718
synced_at: 2026-01-12T16:06:58Z
```

# Improvements to the documentation

---

_@zanieb_

_No description provided._

---

_Label `documentation` added by @zanieb on 2024-08-02 02:15_

---

_Label `preview` added by @zanieb on 2024-08-02 02:15_

---

_@eth3lbert reviewed on 2024-08-02 04:08_

---

_Review comment by @eth3lbert on `docs/pip/environments.md`:16 on 2024-08-02 04:08_

``` suggestion
$ # Create a virtual environment at `.venv`
$ uv venv
```

---

_Review comment by @eth3lbert on `docs/pip/environments.md`:23 on 2024-08-02 04:08_

``` suggestion
$ # Create a virtual environment at `my-name`
$ uv venv my-name
```

---

_@eth3lbert reviewed on 2024-08-02 04:08_

---

_@eth3lbert reviewed on 2024-08-02 04:09_

---

_Review comment by @eth3lbert on `docs/pip/environments.md`:30 on 2024-08-02 04:09_

``` suggestion
$ # Create a virtual environment with Python 3.11
$ uv venv --python 3.11
```

---

_@eth3lbert reviewed on 2024-08-02 04:09_

---

_Review comment by @eth3lbert on `docs/pip/environments.md`:53 on 2024-08-02 04:09_

``` suggestion
$ # On macOS and Linux.
$ source .venv/bin/activate
```

---

_Review comment by @eth3lbert on `docs/pip/packages.md`:51 on 2024-08-02 04:10_

```suggestion
$ # Install a tag
$ uv pip install "git+https://github.com/astral-sh/ruff@v0.2.0"
```

---

_@eth3lbert reviewed on 2024-08-02 04:10_

---

_Review comment by @eth3lbert on `docs/guides/integration/alternative-indexes.md`:24 on 2024-08-02 04:11_

``` suggestion
$ # With the token stored in the `ADO_PAT` environment variable
$ export UV_EXTRA_INDEX_URL=https://dummy:$ADO_PAT@pkgs.dev.azure.com/{organisation}/{project}/_packaging/{feedName}/pypi/simple/
```

---

_@eth3lbert reviewed on 2024-08-02 04:11_

---

_@eth3lbert reviewed on 2024-08-02 04:13_

---

_Review comment by @eth3lbert on `docs/guides/integration/alternative-indexes.md`:49 on 2024-08-02 04:13_

ditto. (but should also expand to unchanged lines.)

---

_Comment by @zanieb on 2024-08-02 22:21_

@eth3lbert thank you! I fixed those, though I mostly tried to move content outside of the codeblock.

---

_Comment by @eth3lbert on 2024-08-02 22:36_

@zanieb You should probably also add a `$` at the beginning of `#` comments.

While serving this locally, I noticed it transforms the code block into the following on the website (http://localhost:8000/uv/guides/integration/alternative-indexes/#using-keyring):
```html
<code tabindex="0"><a id="__codelineno-1-1" name="__codelineno-1-1" href="http://localhost:8000/uv/guides/integration/alternative-indexes/#__codelineno-1-1"></a><span class="gp"># </span>Pre-install<span class="w"> </span>keyring<span class="w"> </span>and<span class="w"> </span>the<span class="w"> </span>Artifacts<span class="w"> </span>plugin<span class="w"> </span>from<span class="w"> </span>the<span class="w"> </span>public<span class="w"> </span>PyPI
<a id="__codelineno-1-2" name="__codelineno-1-2" href="http://localhost:8000/uv/guides/integration/alternative-indexes/#__codelineno-1-2"></a><span class="gp">$ </span>uv<span class="w"> </span>tool<span class="w"> </span>install<span class="w"> </span>keyring<span class="w"> </span>--with<span class="w"> </span>artifacts-keyring
<a id="__codelineno-1-3" name="__codelineno-1-3" href="http://localhost:8000/uv/guides/integration/alternative-indexes/#__codelineno-1-3"></a>
<a id="__codelineno-1-4" name="__codelineno-1-4" href="http://localhost:8000/uv/guides/integration/alternative-indexes/#__codelineno-1-4"></a><span class="gp"># </span>Enable<span class="w"> </span>keyring<span class="w"> </span>authentication
<a id="__codelineno-1-5" name="__codelineno-1-5" href="http://localhost:8000/uv/guides/integration/alternative-indexes/#__codelineno-1-5"></a><span class="gp">$ </span><span class="nb">export</span><span class="w"> </span><span class="nv">UV_KEYRING_PROVIDER</span><span class="o">=</span>subprocess
<a id="__codelineno-1-6" name="__codelineno-1-6" href="http://localhost:8000/uv/guides/integration/alternative-indexes/#__codelineno-1-6"></a>
<a id="__codelineno-1-7" name="__codelineno-1-7" href="http://localhost:8000/uv/guides/integration/alternative-indexes/#__codelineno-1-7"></a><span class="gp"># </span>Configure<span class="w"> </span>the<span class="w"> </span>index<span class="w"> </span>URL<span class="w"> </span>with<span class="w"> </span>the<span class="w"> </span>username
<a id="__codelineno-1-8" name="__codelineno-1-8" href="http://localhost:8000/uv/guides/integration/alternative-indexes/#__codelineno-1-8"></a><span class="gp">$ </span><span class="nb">export</span><span class="w"> </span><span class="nv">UV_EXTRA_INDEX_URL</span><span class="o">=</span>https://VssSessionToken@pkgs.dev.azure.com/<span class="o">{</span>organisation<span class="o">}</span>/<span class="o">{</span>project<span class="o">}</span>/_packaging/<span class="o">{</span>feedName<span class="o">}</span>/pypi/simple/
</code>
```

And it shows that the `#` symbol has the `gp` classname, which could cause issues if copied, as the `#` would be ignored!

---

_Comment by @zanieb on 2024-08-02 22:59_

😭 that's tragic.

---

_Comment by @eth3lbert on 2024-08-02 23:29_

Perhaps adding a note to `STYLE.md`? Alternatively, modify the copy logic to strictly ignore just `$`, not all `go` or `gp` classes.

---

_Marked ready for review by @zanieb on 2024-08-03 13:41_

---

_Merged by @zanieb on 2024-08-03 13:41_

---

_Closed by @zanieb on 2024-08-03 13:41_

---

_Branch deleted on 2024-08-03 13:41_

---
