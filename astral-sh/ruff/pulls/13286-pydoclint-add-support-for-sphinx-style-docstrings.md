```yaml
number: 13286
title: "[`pydoclint`] Add support for Sphinx-style docstrings in the `pydoclint` rules."
type: pull_request
state: open
author: augustelalande
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: sphinx
created_at: 2024-09-08T21:00:58Z
updated_at: 2025-10-12T06:26:55Z
url: https://github.com/astral-sh/ruff/pull/13286
synced_at: 2026-01-10T17:34:34Z
```

# [`pydoclint`] Add support for Sphinx-style docstrings in the `pydoclint` rules.

---

_Pull request opened by @augustelalande on 2024-09-08 21:00_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add support for Sphinx-style docstrings in the `pydoclint` rules. This also defines (out of necessity) the set of pydocstyle rules enforced when the sphinx style is selected. For lack of a good reference I just copied the exclusions from the pep257 selection, minus `Rule::SectionNotOverIndented`.

Part of #12434.

Resolves #12520.

## Test Plan

Test cases added.


---

_Comment by @github-actions[bot] on 2024-09-08 21:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -36 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/document.py#L767'>src/bokeh/document/document.py:767:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/models.py#L160'>src/bokeh/document/models.py:160:9:</a> DOC201 `return` is not documented in docstring
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -34 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- doc/source/user_guide/style.ipynb:cell 113:31:89: E501 Line too long (90 > 88)
- doc/source/user_guide/style.ipynb:cell 113:33:89: E501 Line too long (98 > 88)
- doc/source/user_guide/style.ipynb:cell 123:5:56: E741 Ambiguous variable name: `l`
- doc/source/user_guide/style.ipynb:cell 125:2:13: C408 Unnecessary `dict` call (rewrite as a literal)
- doc/source/user_guide/style.ipynb:cell 125:4:13: C408 Unnecessary `dict` call (rewrite as a literal)
- doc/source/user_guide/style.ipynb:cell 125:6:13: C408 Unnecessary `dict` call (rewrite as a literal)
- doc/source/user_guide/style.ipynb:cell 125:8:13: C408 Unnecessary `dict` call (rewrite as a literal)
- doc/source/user_guide/style.ipynb:cell 126:1:1: NPY002 Replace legacy `np.random.seed` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 126:2:1: PLW0127 Self-assignment of variable `cmap`
- doc/source/user_guide/style.ipynb:cell 126:2:8: PLW0128 Redeclared variable `cmap` in assignment
- doc/source/user_guide/style.ipynb:cell 126:3:22: NPY002 Replace legacy `np.random.randn` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 128:1:22: NPY002 Replace legacy `np.random.randn` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 134:1:89: E501 Line too long (93 > 88)
- doc/source/user_guide/style.ipynb:cell 141:1:89: E501 Line too long (95 > 88)
- doc/source/user_guide/style.ipynb:cell 157:1:38: F811 Redefinition of unused `f` from cell 123, line 5
- doc/source/user_guide/style.ipynb:cell 15:2:89: E501 Line too long (103 > 88)
- doc/source/user_guide/style.ipynb:cell 15:3:89: E501 Line too long (156 > 88)
- doc/source/user_guide/style.ipynb:cell 16:48:89: E501 Line too long (103 > 88)
... 12 additional changes omitted for rule E501
- doc/source/user_guide/style.ipynb:cell 37:1:1: NPY002 Replace legacy `np.random.seed` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 37:2:20: NPY002 Replace legacy `np.random.randn` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 60:1:20: NPY002 Replace legacy `np.random.randn` call with `np.random.Generator`
- doc/source/user_guide/style.ipynb:cell 6:1:27: NPY002 Replace legacy `np.random.rand` call with `np.random.Generator`
... 2 additional changes omitted for rule NPY002
</pre>

</p>
</details>
<details><summary>Changes by rule (8 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E501 | 18 | 0 | 18 | 0 | 0 |
| NPY002 | 8 | 0 | 8 | 0 | 0 |
| C408 | 4 | 0 | 4 | 0 | 0 |
| DOC201 | 2 | 0 | 2 | 0 | 0 |
| E741 | 1 | 0 | 1 | 0 | 0 |
| PLW0127 | 1 | 0 | 1 | 0 | 0 |
| PLW0128 | 1 | 0 | 1 | 0 | 0 |
| F811 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2024-09-10 18:22_

Wow thanks. This is great.

There are a lot of new ecosystem reports. Some of the superset violations look somewhat suspicious: 


+ [RELEASING/generate_email.py:47:5:](https://github.com/apache/superset/blob/0744abe87bacd1ff79672106b9bcaf93e6e4b816/RELEASING/generate_email.py#L47) D405 [*] Section name should be properly capitalized ("")
+ [RELEASING/generate_email.py:47:5:](https://github.com/apache/superset/blob/0744abe87bacd1ff79672106b9bcaf93e6e4b816/RELEASING/generate_email.py#L47) D410 [*] Missing blank line after section ("")
+ [RELEASING/generate_email.py:49:5:](https://github.com/apache/superset/blob/0744abe87bacd1ff79672106b9bcaf93e6e4b816/RELEASING/generate_email.py#L49) D405 [*] Section name should be properly capitalized ("")
+ [RELEASING/generate_email.py:49:5:](https://github.com/apache/superset/blob/0744abe87bacd1ff79672106b9bcaf93e6e4b816/RELEASING/generate_email.py#L49) D411 [*] Missing blank line before section ("")
+ [RELEASING/generate_email.py:49:5:](https://github.com/apache/superset/blob/0744abe87bacd1ff79672106b9bcaf93e6e4b816/RELEASING/generate_email.py#L49) D413 [*] Missing blank line after last section ("")
+ [RELEASING/generate_email.py:49:5:](https://github.com/apache/superset/blob/0744abe87bacd1ff79672106b9bcaf93e6e4b816/RELEASING/generate_email.py#L49) D414 Section has no content ("")

Like why are all the names empty?

---

_Comment by @MichaReiser on 2024-09-10 18:31_

It might just be that the docstyle is a bit too aggressive right now. Should [this](https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/app.py#L4) be considered a sphinx docstring?

Before I try to figure it out from the code. Did you gate the new code style behind preview mode? I think we have to because this change greatly increases the scope of the rule and it's probably good to get some feedback before rolling it out to everyone


---

_Comment by @augustelalande on 2024-09-10 19:24_

All the `pydoclint` rules are in preview

---

_Comment by @MichaReiser on 2024-09-11 20:10_

Looking at the airflow result I'm unsure if the parsing is correct. Could you take a look at them? 

Why is `param` over indented?

* + [airflow/providers/google/leveldb/operators/leveldb.py:36:10:](https://github.com/apache/airflow/blob/a71944f8c401ce82a2c3a920cb2eab128f8215ad/airflow/providers/google/leveldb/operators/leveldb.py#L36) D214 [*] Section is over-indented ("param")

Why is `param` considered a section?

+ [airflow/api/common/delete_dag.py:47:6:](https://github.com/apache/airflow/blob/a71944f8c401ce82a2c3a920cb2eab128f8215ad/airflow/api/common/delete_dag.py#L47) D410 [*] Missing blank line after section ("param")

The other thing I'm unsure about is whether the blank line rule between parameters should be disabled by default. All the examples I found don't require blank lines between parameters. What do you think?



---

_Comment by @augustelalande on 2024-09-11 20:37_

> Why is param over indented?

It is over indented relative to the docstring as a whole, since it is indented within the `.. seealso::` section

> Why is param considered a section?

Sphinx doesn't have sections per se, so to make the sphinxs docstrings fit into the mold in use for google and numpy docstrings, I consider each `:param title: Text` to be a section.

Note: It's not possible to combine all the param entries into a single param sections, since as far as I know there is no obligation that params must follow each other, although in practice they usually would.

---

_Comment by @MichaReiser on 2024-09-11 20:46_

> Sphinx doesn't have sections per se, so to make the sphinxs docstrings fit into the mold in use for google and numpy docstrings, I consider each :param title: Text to be a section.

That makes sense but it seems confusing to users because I wouldn't consider those sections and it seems to cause many false positives in other rules. I would have to think this through more carefully but would we emit different diagnostics if we designed this from scratch?

---

_Comment by @tmke8 on 2024-09-11 21:12_

Everything sphinx can parse successfully with default settings should be fine, no? And sphinx definitely doesn't require blank lines between `:param ...:` entries.

Here is a docstring that uses a lot of sphinx features and which is parsed correctly by sphinx.

```python
class SAM(Optimizer):
    """Sharpness Aware Minimization

    Implements the 'Sharpness Aware Minimization' (SAM) algorithm introducued in
    `Sharpness Aware Minimization`_) and the adaptive variant of it introduced in `ASAM`_.

    We can use inline math: :math:`\\alpha`. You don't need the double backslash if
    the python docstring is a raw string (r-string).

    Block math:

    .. math::

       (a + b)^2 = a^2 + 2ab + b^2

    .. note::
        Implementation based on: https://github.com/cybertronai/pytorch-lamb
    .. seealso:: :class:`LAMB`

    .. _Sharpness Aware Minimization:
        https://arxiv.org/abs/2010.01412
    .. _ASAM:
        https://arxiv.org/abs/2102.11600

    :param base_optimizer: Base optimizer for SAM. (If the explanation goes
        over two lines, the second line needs to be indented.
    :param rho: Neighborhood size.

    :raises ValueError: if ``rho`` is negative.

    :example:
        Everything associated with the example section needs to be indented, so that Sphinx
        knows what belongs to the example.

        .. code-block:: python

           # Use AdamW as the base optimizer.
           base_optimizer = AdamW(model.parameters())
           # Wrap the base optimizer in SAM.
           optimizer = SAM(base_optimizer)

           # Closure required for recomputing the loss after computing epsilon(w).
           def _closure():
               return loss_function(logits=model(input), targets=targets)

           loss = _closure()
           loss.backward()

           optimizer.step(closure=_closure)
           optimizer.zero_grad()

        You can also write it in this syntax:

        >>> import template
        >>> a = template.MainClass1()
        >>> a.function1(1,1,1)
        2
    """
```

---

_Comment by @augustelalande on 2024-09-11 21:25_

> > Sphinx doesn't have sections per se, so to make the sphinxs docstrings fit into the mold in use for google and numpy docstrings, I consider each :param title: Text to be a section.
> 
> That makes sense but it seems confusing to users because I wouldn't consider those sections and it seems to cause many false positives in other rules. I would have to think this through more carefully but would we emit different diagnostics if we designed this from scratch?

We probably would not emit D410 among others for sphinx style docstrings. And in fact if the user specifies sphinx as their docstring convention then those rules get disabled (each convention runs a different subset of D). The problem is when the convention is unspecified we still run it.

One solution would be to disable innapropriate rules if sphinx is detected. There is some precendence for that, and I have already done it for some of them.

I don't know what a "proper" implementation of "D410" would look like for sphinx, since there are no "sections" per se

---

_Comment by @augustelalande on 2024-09-11 21:29_

One of the problems is that `pydocstyle` doesn't support sphinx so there is nothing to reference. We could choose to not run `pydocstyle` rules if sphinx is detected or specified.

---

_Label `rule` added by @MichaReiser on 2024-09-12 18:55_

---

_Label `needs-decision` added by @MichaReiser on 2024-09-12 18:55_

---

_Comment by @tmke8 on 2024-09-19 13:35_

So, is the problem here that pydocstyle and pydoclint are incompatible? I see a `--convention=pep257` flag in pydocstyle – this sounds like the convention that should be compatible with "sphinx-style"?

---

_Assigned to @MichaReiser by @MichaReiser on 2024-09-20 08:40_

---

_Comment by @charliermarsh on 2024-09-23 23:27_

One thing I'm not following from the conversation is why there are so many `D` rule changes in stable. I know the pydoclint rules are in preview, but I think this Sphinx detection should be too, right? In other words, we shouldn't see any non-preview changes in this PR for the `D` rules.

Another thing we could do is: never _infer_ Sphinx. Require that users set it via a `convention`. That would help a lot with backwards compatibility while still giving us these benefits. On top of that, I'd err on the side of enabling a small set of rules for Sphinx, those we are confident in.

Finally... I think pydocstyle _did_ add Sphinx support, at least for D417? Can we make sure that whatever we do here is consistent with the pydocstyle implementation? See: https://github.com/PyCQA/pydocstyle/pull/595


---

_Comment by @augustelalande on 2024-09-28 05:33_

Per above, I have improved the gating so sphinx will only be active in preview mode.

I have also implemented the change to never infer sphinx, which should prevent a lot of the issues. But I am concerned that this prevents the ecosystem check to work properly, since they don't specify sphinx in their configs. Would love to hear some insights on this.

---

_Comment by @augustelalande on 2024-09-28 05:39_

Ok so D417 is undocumented-param, which I think I can address in #13280 if this gets merged first (otherwise I'll add it here). Note, we already don't have full parity with D417 since the ruff version only supports google style.

---

_Comment by @CommentatorForAll on 2025-05-05 09:06_

what is the state of this pull request and when can a merge be expected?

---

_Comment by @afonsotrepa on 2025-10-02 17:57_

Is there anything left (for a non-maintainer) to do to get this merged?
I'm willing to put in some cycles if needed.

Thanks for the great work @augustelalande 

---

_Comment by @MichaReiser on 2025-10-12 06:26_

It's mainly that someone from the team has to find the time to go through all the changes again. We'll do some roadmapping next week. I'll try to bring it up there

---
