```yaml
number: 13197
title: Add meta descriptions to rule pages
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - documentation
assignees: []
created_at: 2024-09-01T18:23:07Z
updated_at: 2024-09-09T14:02:01Z
url: https://github.com/astral-sh/ruff/issues/13197
synced_at: 2026-01-12T15:54:52Z
```

# Add meta descriptions to rule pages

---

_@charliermarsh_

Sometimes we're showing the Markdown right now?!

![Screenshot 2024-09-01 at 2 21 57 PM](https://github.com/user-attachments/assets/2ec3915e-6d88-4a6e-b9c6-52282da2d087)


---

_Label `bug` added by @charliermarsh on 2024-09-01 18:23_

---

_Label `documentation` added by @AlexWaygood on 2024-09-01 19:21_

---

_Comment by @charliermarsh on 2024-09-03 00:48_

I'm very confused about where the `#.` is coming from...?

---

_Comment by @MichaReiser on 2024-09-03 05:54_

Maybe from the `headerlink` element? Although I'm not sure where the `.` is coming from.

```html
<h1 id="f-string-up032">
  f-string (UP032)<a
    class="headerlink"
    href="#f-string-up032"
    title="Permanent link"
    >#</a
  >
</h1>
<p>Derived from the <strong>pyupgrade</strong> linter.</p>
<p>Fix is sometimes available.</p>
<h2 id="what-it-does">
  What it does<a class="headerlink" href="#what-it-does" title="Permanent link"
    >#</a
  >
</h2>
<p>
  Checks for <code>str.format</code> calls that can be replaced with f-strings.
</p>
<h2 id="why-is-this-bad">
  Why is this bad?<a
    class="headerlink"
    href="#why-is-this-bad"
    title="Permanent link"
    >#</a
  >
</h2>
<p>
  f-strings are more readable and generally preferred over
  <code>str.format</code> calls.
</p>
<h2 id="example">
  Example<a class="headerlink" href="#example" title="Permanent link">#</a>
</h2>
<div class="highlight">
  <pre><span></span><code><a id="__codelineno-0-1" name="__codelineno-0-1" href="#__codelineno-0-1"></a><span class="s2">&quot;</span><span class="si">{}</span><span class="s2">&quot;</span><span class="o">.</span><span class="n">format</span><span class="p">(</span><span class="n">foo</span><span class="p">)</span>
</code></pre>
</div>
<p>Use instead:</p>
<div class="highlight">
  <pre><span></span><code><a id="__codelineno-1-1" name="__codelineno-1-1" href="#__codelineno-1-1"></a><span class="sa">f</span><span class="s2">&quot;</span><span class="si">{</span><span class="n">foo</span><span class="si">}</span><span class="s2">&quot;</span>
</code></pre>
</div>
<h2 id="references">
  References<a class="headerlink" href="#references" title="Permanent link"
    >#</a
  >
</h2>
<ul>
  <li>
    <a
      href="https://docs.python.org/3/reference/lexical_analysis.html#f-strings"
      >Python documentation: f-strings</a
    >
  </li>
</ul>

```

---

_Comment by @charliermarsh on 2024-09-03 18:06_

Oh interesting, that's a good call...

---

_Closed by @charliermarsh on 2024-09-09 14:02_

---
