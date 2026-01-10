```yaml
number: 9211
title: Lint rule shortlinks
type: issue
state: closed
author: notpushkin
labels: []
assignees: []
created_at: 2023-12-20T12:12:10Z
updated_at: 2024-01-02T18:10:30Z
url: https://github.com/astral-sh/ruff/issues/9211
synced_at: 2026-01-10T11:09:51Z
```

# Lint rule shortlinks

---

_Issue opened by @notpushkin on 2023-12-20 12:12_

I've set up a short URL for Flake8 rules at [pyflak.es](https://pyflak.es/). It redirects from `pyflak.es/CODE` to the rule page on 
Flake8 Rules (e.g. [pyflak.es/E302](https://pyflak.es/E302)) or the respective plugin documentation (e.g. [pyflak.es/RST202](https://pyflak.es/RST202)). It should work with all rules supported by Ruff.

This can be handy when you want to quickly look up a specific rule. For example, the `wemake` formatter for Flake8 can print out the links, which you can click (in modern terminals) to open the respective docs page:

<pre>
» flake8 . --format=wemake --show-source --show-violation-links
./wemake_python_styleguide/formatter.py
  107:32   E231  missing whitespace after ':'
           <strong>-> <a href="https://pyflak.es/E231">https://pyflak.es/E231</a></strong>
  def show_source(self, error:Violation) -> str:
                              ^
</pre>

Would you be interested in integrating this, too? Let me know and I can try and work out a PR :-)


---

_Comment by @notpushkin on 2023-12-26 17:08_

I've updated all rules supported by Ruff to link to `https://docs.astral.sh/ruff/rules/*` instead of the respective Flake8 plugin docs (as for most plugins Ruff docs are way more polished :-) Example: [pyflak.es/D100](https://pyflak.es/D100)

The only exception is pycodestyle errors and warnings, which still link to `https://www.flake8rules.com/rules/*`. And of course, any rules not supported by Ruff will still open the plugin page.


---

_Comment by @charliermarsh on 2024-01-02 18:09_

That's really neat, thanks!


---

_Comment by @charliermarsh on 2024-01-02 18:10_

We're likely gonna make some changes to the output format in the near-ish future to show source by default which will also give us an opportunity to incorporate documentation links -- but for now I'd rather omit, I think.

---

_Closed by @charliermarsh on 2024-01-02 18:10_

---
