---
number: 9103
title: Import detection and resolution via Python resolver for pylint
type: issue
state: closed
author: pwithams
labels:
  - type-inference
assignees: []
created_at: 2023-12-12T06:19:42Z
updated_at: 2025-06-25T13:58:35Z
url: https://github.com/astral-sh/ruff/issues/9103
synced_at: 2026-01-07T13:12:15-06:00
---

# Import detection and resolution via Python resolver for pylint

---

_Issue opened by @pwithams on 2023-12-12 06:19_

I'm interested in implementing `no-name-in-module` (E0611) and `import-error` (E0401) for pylint (https://github.com/astral-sh/ruff/issues/970).

I think this would handle this issue https://github.com/astral-sh/ruff/issues/6327 but as mentioned in that issue thread it might get a little complex with things like virtual environments.

I've taken a brief look at the resolver but wondering if there has been any other work into this issue before continuing. A couple of my thoughts:

 - I believe pylint essentially runs the code and catches the exceptions `ImportError` and `ModuleNotFoundError`, although it will return all errors, not just the first, with a line like `import os.foobar` triggering both errors
 - ruff appears to be so fast currently due to avoiding (or at least minimizing) invoking any python
 - is there any way to achieve this functionality without actually calling python, such as somehow determining a list of available modules (for `import-error` at least)? Could this ever be fully trustworthy? I'm guessing not.
 - if python does have to be called (can't really see a way to avoid it for `no-name-in-module`, especially when it comes to things like c libraries), are there ways to optimize/minimize python usage/runtime?

---

_Comment by @charliermarsh on 2024-01-10 20:02_

Have you taken a look at the resolver we aded in [`ruff_python_resolver`](https://github.com/astral-sh/ruff/tree/main/crates/ruff_python_resolver)? It's based off Pyright's implementation.

---

_Label `question` added by @charliermarsh on 2024-01-10 20:02_

---

_Comment by @walsha2 on 2024-04-03 00:15_

@charliermarsh is the [`ruff_python_resolver`](https://github.com/astral-sh/ruff/tree/main/crates/ruff_python_resolver) intended to be a solution to `E0611`? I cant seem to find any documentation on what it is currently being used for.

Sure would be nice to get `E0611` and `E0401` implemented. I just ran into an edge case where these would have caught a file with some bad imports (as `pylint` would have), but it was not invoked in any tests so it never got caught - until a user runtime error.

---

_Label `multifile-analysis` added by @MichaReiser on 2024-09-09 12:09_

---

_Label `question` removed by @MichaReiser on 2024-09-09 12:09_

---

_Comment by @vinitkumar on 2024-09-09 17:40_

@charliermarsh In my opinion it should be implemented as well. We just missed a bug due to faulty import and ruff didn't catch the issue. Would be great to have a compatible implementation in our setup as ruff is our primary linter here. 

https://github.com/django-cms/django-cms/issues/7992
We have this new issue because we added this faulty imported while backporting a feature, and this module is not in the 3.11 version of the project. Ruff passed the lint with flying colours where it should have pointed it out. 


---

_Comment by @vinitkumar on 2024-09-23 10:07_

@MichaReiser Can this be prioritised, please?

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---

_Comment by @pespinel on 2025-03-11 11:14_

> [@charliermarsh](https://github.com/charliermarsh) In my opinion it should be implemented as well. We just missed a bug due to faulty import and ruff didn't catch the issue. Would be great to have a compatible implementation in our setup as ruff is our primary linter here.
> 
> [django-cms/django-cms#7992](https://github.com/django-cms/django-cms/issues/7992) We have this new issue because we added this faulty imported while backporting a feature, and this module is not in the 3.11 version of the project. Ruff passed the lint with flying colours where it should have pointed it out.

We've just had the same issue: a faulty import that wasn't caught by Ruff. Any news on this?

---

_Comment by @boringplay on 2025-04-30 03:54_

Any news on this?

---

_Comment by @vinitkumar on 2025-06-06 12:41_

@charliermarsh It would be great if this one gets picked up.

---

_Comment by @MichaReiser on 2025-06-06 13:21_

ty, our in-progress type checker can detect invalid imports for you. We may decide to bring this to Ruff but it isn't on our immediate roadmap

---

_Comment by @vinitkumar on 2025-06-06 13:24_

> ty, our in-progress type checker can detect invalid imports for you. We may decide to bring this to Ruff but it isn't on our immediate roadmap

@MichaReiser Thanks for your reply. Is `ty` any close to being used in production codebases? If it's good enough, we can use that in combination with ruff.

---

_Comment by @pwithams on 2025-06-25 01:38_

> ty, our in-progress type checker can detect invalid imports for you. We may decide to bring this to Ruff but it isn't on our immediate roadmap

I just started looking at this again. I see two main approaches:
1. use `ruff_python_resolver` - I have a working proof of concept for this, but it has limitations. For example, I don't see a clear way to check against stdlib modules beyond their root name, and `resolver::resolve_import` doesn't seem to work with attributes imported via `from`. However, it does support basic checks like `import doesnotexist` but would not catch things like `import os.bad.sub.path` or `from valid_module import undefined_attribute`. Virtualenv path discovery would need to be added, similar to `ty`.
2. use logic from `ty` - as you mentioned it already implemented and solves the problems above. However, I'm not sure how easy it would be to integrate this logic into the linting checker, or even if that is something desirable in terms of design. I see there is some usage of `ty_python_semantic` in `ruff_graph`.

Any thoughts on these approaches?

---

_Comment by @MichaReiser on 2025-06-25 06:27_

Hi @pwithams 

Thanks for looking into this. I don't think it makes sense for us to invest into this right now, given that we have a working solution for this in ty and integrating any multi-file aspect into Ruff requires changes in our caching system and LSP server too (a file's diagnostics can now change when other files change too).




---

_Referenced in [astral-sh/ruff#18933](../../astral-sh/ruff/pulls/18933.md) on 2025-06-25 10:53_

---

_Comment by @pwithams on 2025-06-25 13:58_

That makes sense, thanks!

The main driver was continuing to replicate `pylint -E` behavior, which I find useful as a simple unified tool/CLI command for checking fatal errors.

For more formal applications chaining in something like `ty check --error` shouldn't be an issue - I'm excited to try it out.

---

_Closed by @pwithams on 2025-06-25 13:58_

---

_Referenced in [apache/airflow#53627](../../apache/airflow/pulls/53627.md) on 2025-07-22 21:01_

---
