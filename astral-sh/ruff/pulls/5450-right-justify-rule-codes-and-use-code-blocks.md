```yaml
number: 5450
title: Right-justify rule codes and use code blocks around them
type: pull_request
state: closed
author: MicaelJarniac
labels: []
assignees: []
base: main
head: main
created_at: 2023-06-30T03:09:48Z
updated_at: 2023-07-06T22:07:02Z
url: https://github.com/astral-sh/ruff/pull/5450
synced_at: 2026-01-12T15:55:18Z
```

# Right-justify rule codes and use code blocks around them

---

_@MicaelJarniac_

Right-justify rule codes on the Table of Contents (ToC), and wrap them in code blocks.

This should make it a lot easier and faster to visually scan through the many rule codes and find the one you're looking for.

I often found myself looking for a specific rule code, not knowing its name, and having to move my eyes something like this:
<img src="https://github.com/astral-sh/ruff/assets/19514231/c50fcd1a-e58e-4115-b0c0-3886e543783b" height="500">

This made it very difficult to scan the rules, and very easy to accidentally miss the one I was looking for.

This does not break the anchors (`#pycodestyle-e-w`, for example) for the different rule sections.

This seems to work well on mobile.

## Before and After
<img src="https://github.com/astral-sh/ruff/assets/19514231/1ed45ba4-a6af-44db-b4a8-6226e069f1c5" height="500"> <img src="https://github.com/astral-sh/ruff/assets/19514231/a6113084-493c-4475-8fdb-c5840edc2878" height="500">

Long rule names now look like this: 
![image](https://github.com/astral-sh/ruff/assets/19514231/8e2e66ad-8a54-492c-b195-9e02d5e01607)

And section names look like this:
![image](https://github.com/astral-sh/ruff/assets/19514231/9f117178-6ce6-448a-b121-d9d7510f79ad)


---

_Comment by @MicaelJarniac on 2023-06-30 03:14_

Minor issue with one of the changes, the browser tab title shows the backticks:
![image](https://github.com/astral-sh/ruff/assets/19514231/951242ec-4bef-44de-887b-032ab993c8d4)


---

_@MicaelJarniac reviewed on 2023-06-30 03:16_

---

_Review comment by @MicaelJarniac on `crates/ruff_dev/src/generate_docs.rs`:28 on 2023-06-30 03:16_

```suggestion
            output.push_str(&format!("# ({}) {}", rule.noqa_code(), rule.as_ref()));
```
To fix the browser tab title showing the backticks:
![image](https://github.com/astral-sh/ruff/assets/19514231/951242ec-4bef-44de-887b-032ab993c8d4)
I'm not sure which would be preferable, having the code block or fixing the tab title.

---

_Comment by @github-actions[bot] on 2023-06-30 03:24_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.6±0.21ms     4.7 MB/sec    1.01      8.7±0.26ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1933.2±46.49µs     8.6 MB/sec    1.00  1926.2±64.47µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    225.1±7.03µs    13.1 MB/sec    1.05   236.1±23.00µs    12.5 MB/sec
formatter/pydantic/types.py                1.04      4.3±0.09ms     5.9 MB/sec    1.00      4.2±0.14ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.02     15.0±0.34ms     2.7 MB/sec    1.00     14.7±0.35ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.10ms     4.5 MB/sec    1.04      3.8±0.07ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   483.5±14.90µs     6.1 MB/sec    1.05   506.4±29.37µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.27ms     3.8 MB/sec    1.04      6.9±0.24ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.15ms     5.6 MB/sec    1.02      7.4±0.16ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1587.8±46.43µs    10.5 MB/sec    1.00  1589.0±44.29µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    185.8±4.96µs    15.9 MB/sec    1.00    183.5±8.17µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.09ms     7.7 MB/sec    1.02      3.4±0.07ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     12.7±0.61ms     3.2 MB/sec    1.00     12.6±0.58ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.6±0.10ms     6.3 MB/sec    1.02      2.7±0.29ms     6.2 MB/sec
formatter/numpy/globals.py                 1.00   310.9±19.09µs     9.5 MB/sec    1.06   328.9±22.52µs     9.0 MB/sec
formatter/pydantic/types.py                1.00      5.8±0.22ms     4.4 MB/sec    1.01      5.8±0.22ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.01     21.9±0.84ms  1904.5 KB/sec    1.00     21.7±0.95ms  1919.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.5±0.20ms     3.0 MB/sec    1.02      5.6±0.22ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   647.5±28.20µs     4.6 MB/sec    1.03   666.5±26.26µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.3±0.46ms     2.7 MB/sec    1.02      9.5±0.31ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     11.0±0.36ms     3.7 MB/sec    1.01     11.1±0.31ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.15ms     7.4 MB/sec    1.05      2.3±0.12ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.06   288.3±12.95µs    10.2 MB/sec    1.00   273.3±13.86µs    10.8 MB/sec
linter/default-rules/pydantic/types.py     1.03      5.0±0.20ms     5.1 MB/sec    1.00      4.8±0.17ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-06-30 07:54_

Would it be possible to right algin the rule codes instead? Because we now have the opposite issue: The rule names are no longer aligned.

![image](https://github.com/astral-sh/ruff/assets/1203881/0478477d-d977-44ad-8b04-c405c2dd0f87)

I was too lazy to change all of them but like this

---

_Comment by @MicaelJarniac on 2023-06-30 13:32_

> Would it be possible to right algin the rule codes instead? Because we now have the opposite issue: The rule names are no longer aligned.

As far as I can tell, the sidebar is automatically generated based on the header tags of the page, that are also automatically generated, and they're Markdown.

I'll see if I can figure out something, but I suspect it won't be easy to do.

---

_Renamed from "Show rule code before rule name on sidebar and title" to "Left-justify rule codes and use code blocks around them" by @MicaelJarniac on 2023-07-03 18:50_

---

_Comment by @MicaelJarniac on 2023-07-03 18:55_

@MichaReiser thanks for your suggestion! It took quite a while, but I managed to do it, and I agree it's much better.

---

_Comment by @MicaelJarniac on 2023-07-03 19:19_

Docs generation isn't working on the GitHub Action, for whatever reason. I'm trying to figure it out...

---

_Comment by @MicaelJarniac on 2023-07-03 19:57_

I haven't managed to figure it out yet.
[Error](https://github.com/astral-sh/ruff/actions/runs/5447999581/jobs/9910664860?pr=5450#step:10:14)
```
ERROR    -  Config value 'markdown_extensions': Failed to load extension 'custom-toc'.
ModuleNotFoundError: No module named 'custom-toc'
Aborted with 1 Configuration Errors!
Error: Process completed with exit code 1.
```

---

_Comment by @MicaelJarniac on 2023-07-03 20:45_

Okay, so it seems like setting a Python version for the docs workflow explicitly fixed it.
I've set it to `3.11`, but I suspect earlier versions _might_ work.
If `3.11` is too bleeding-edge for building the docs, we could try another version.

---

_Renamed from "Left-justify rule codes and use code blocks around them" to "Right-justify rule codes and use code blocks around them" by @MicaelJarniac on 2023-07-03 20:54_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-07-03 23:37_

---

_Comment by @charliermarsh on 2023-07-04 00:07_

I'm getting:

```
ERROR    -  Config value 'markdown_extensions': Failed to load extension
            'custom-toc'.
            ModuleNotFoundError: No module named 'custom-toc
```

When running locally. I ran `pip install -r docs/requirements.txt` on `Python 3.11.2`. Any ideas?

---

_Comment by @MicaelJarniac on 2023-07-04 00:26_

This is the error that was happening on the GitHub Action for `mkdocs`, and I kinda "fixed" it by forcing the workflow to Python 3.11.

Locally I can install it with Python 3.11.3 and it works fine.

Could you try installing the subpackage directly, with `pip install docs/custom-extension`, and share the output?

I'm not entirely sure what's the problem, or how to fix it.

One thing I considered was to actually publish this custom extension, but I'm not sure if it'd be desirable, as it'd then become another dependency.

I'll try installing Python 3.11.2 here and see if I can reproduce it.

---

_Comment by @MicaelJarniac on 2023-07-04 00:27_

Another thing worth testing, `pip install -U pip setuptools wheel`.

---

_Comment by @MicaelJarniac on 2023-07-04 00:48_

Tested with Python 3.11.2 and it still works here. I have no idea what's going on.

---

_Comment by @charliermarsh on 2023-07-04 00:53_

All good, I will take another look.

---

_@MichaReiser reviewed on 2023-07-04 08:18_

---

_Review comment by @MichaReiser on `docs/custom-extensions/src/custom_extensions/toc.py`:2 on 2023-07-04 08:18_

Do you know if there's a way to avoid copying the plugin into our repository? What I understand from the documentation is that it is shipped as a built-in extension: https://python-markdown.github.io/extensions/

---

_@MicaelJarniac reviewed on 2023-07-04 14:22_

---

_Review comment by @MicaelJarniac on `docs/custom-extensions/src/custom_extensions/toc.py`:2 on 2023-07-04 14:22_

The original plugin is indeed shipped as a built-in extension, and it was being used before.

I did, however, need to modify it quite a bit, to allow for some HTML tags to be kept on the generated TOC. The original plugin removes all tags.

You can see the modifications I did to the plugin on this commit: https://github.com/astral-sh/ruff/pull/5450/commits/bbbb31b6fe4a51fda4ac6fc4cfe5e3d083f7b3d0

I believe it might be possible to "monkeypatch" the original plugin somehow, but it'd end up way messier than just shipping a modified copy of it, I think.

---

_Comment by @MicaelJarniac on 2023-07-06 02:30_

I tried merging `main` back here and now it's giving me SSH key errors of some sort.
`Error: The ssh-private-key argument is empty. Maybe the secret has not been configured, or you are using a wrong secret name in your workflow file.`

---

_Comment by @MichaReiser on 2023-07-06 12:25_

I like the new layout. @charliermarsh how do you feel about copying over the extension and modifying it in place?

---

_Comment by @charliermarsh on 2023-07-06 21:22_

I appreciate this work, but my current feeling is that it's not worth forking + maintaining a fork for this level of improvement. I marginally prefer the new layout, but to put it differently, I'm not sure that the complexity outweighs the benefit. If we could upstream the change somehow, I'd be open to reconsidering. But just as an example, as-is, this doesn't work with the `typeset` plugin, which is sadly on Insiders (I know, I know) but will be mainstreamed in a future release, and which is useful to enable for (e.g.) the settings page:

<img width="265" alt="Screen Shot 2023-07-06 at 5 15 16 PM" src="https://github.com/astral-sh/ruff/assets/1309177/0bd6afb9-8d3c-417d-a883-f3ecea37a058">

So we'd then need to get rid of the `typeset` plugin to support this. Overall, it feels heavyweight to maintain this fork, custom package, etc., and we'll have to think about compatibility going forward. I'm sorry for this outcome, I hate saying no to PRs, but it feels like the responsible decision here from a maintainability perspective.


---

_Closed by @charliermarsh on 2023-07-06 21:22_

---

_Comment by @MicaelJarniac on 2023-07-06 22:03_

@charliermarsh what about the first version of this PR, that simply puts the rule code before the rule name?

![image](https://github.com/astral-sh/ruff/assets/19514231/362c4b6e-8393-4405-9f7d-76d5151e936c)


---

_Comment by @MicaelJarniac on 2023-07-06 22:07_

Also, I'd like to look into upstreaming the patched `toc` plugin. Is there a way I can experiment with this incompatible `typeset` plugin you mentioned, so I can try to make it compatible?

---
