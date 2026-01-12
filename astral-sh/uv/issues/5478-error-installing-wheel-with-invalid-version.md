```yaml
number: 5478
title: "Error installing wheel with \"invalid version\""
type: issue
state: closed
author: albertotb
labels:
  - compatibility
assignees: []
created_at: 2024-07-26T12:40:27Z
updated_at: 2025-02-19T01:24:49Z
url: https://github.com/astral-sh/uv/issues/5478
synced_at: 2026-01-12T15:58:56Z
```

# Error installing wheel with "invalid version"

---

_@albertotb_

Version: `uv 0.2.29`

I'm not sure if this is intended or not:

```
uv pip install https://huggingface.co/PlanTL-GOB-ES/es_anonimization_core_lg/resolve/main/es_anonimization_core_lg-any-py3-none-any.whl
```

Fails with

```
error: The wheel filename "es_anonimization_core_lg-any-py3-none-any.whl" has an invalid version: expected version to start with a number, but no leading ASCII digits were found
```

However, the equivalent pip command is able to install the wheel:

```
pip install https://huggingface.co/PlanTL-GOB-ES/es_anonimization_core_lg/resolve/main/es_anonimization_core_lg-any-py3-none-any.whl
Collecting es-anonimization-core-lg==any
  Downloading https://huggingface.co/PlanTL-GOB-ES/es_anonimization_core_lg/resolve/main/es_anonimization_core_lg-any-py3-none-any.whl (587.8 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 587.8/587.8 MB 1.5 MB/s eta 0:00:00
```


---

_Comment by @charliermarsh on 2024-07-26 18:34_

In our current design this is intentional -- we require that wheels always have valid wheel filenames, and that isn't a valid filename (since it's omitting the version).

---

_Label `compatibility` added by @charliermarsh on 2024-07-26 18:35_

---

_Comment by @charliermarsh on 2024-07-29 00:51_

We should support this, requires some refactors though.

---

_Comment by @eth3lbert on 2024-07-29 06:42_

> Version: `uv 0.2.29`
> 
> I'm not sure if this is intended or not:
> 
> ```
> uv pip install https://huggingface.co/PlanTL-GOB-ES/es_anonimization_core_lg/resolve/main/es_anonimization_core_lg-any-py3-none-any.whl
> ```
> 
> Fails with
> 
> ```
> error: The wheel filename "es_anonimization_core_lg-any-py3-none-any.whl" has an invalid version: expected version to start with a number, but no leading ASCII digits were found
> ```
> 
> However, the equivalent pip command is able to install the wheel:
> 
> ```
> pip install https://huggingface.co/PlanTL-GOB-ES/es_anonimization_core_lg/resolve/main/es_anonimization_core_lg-any-py3-none-any.whl
> Collecting es-anonimization-core-lg==any
>   Downloading https://huggingface.co/PlanTL-GOB-ES/es_anonimization_core_lg/resolve/main/es_anonimization_core_lg-any-py3-none-any.whl (587.8 MB)
>      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 587.8/587.8 MB 1.5 MB/s eta 0:00:00
> ```

Apologies for the intrusion. Could you please specify which version of `pip` you are using?

I have tested this with `pip` version `24.2` and encountered the following error:
```shell
pip install https://huggingface.co/PlanTL-GOB-ES/es_anonimization_core_lg/resolve/main/es_anonimization_core_lg-any-py3-none-any.whl               
ERROR: Invalid requirement: 'es-anonimization-core-lg==any': Expected end or semicolon (after name and no valid version specifier)
    es-anonimization-core-lg==any
```

---

_Comment by @albertotb on 2024-07-29 13:47_

Sure, I'm using pip 24.0, which seems to work fine. I will report to the creator of the wheel to see if they can follow the naming conventions. The confusing part here is that with pip seems to work (at least in some versions), so the wheel mantainers may not have noticed that the naming is incorrect.

---

_Comment by @albertotb on 2024-07-29 22:35_

Looking at pip release notes maybe this is related to this issue https://github.com/pypa/pip/issues/12063

It seems that starting with pip 24.1b1 the wheel I used as an example is no longer installable with pip https://pip.pypa.io/en/stable/news/#b1-2024-05-06

---

_Comment by @jcorrie on 2024-10-27 15:40_

I'm experiencing the same issue using uv pip to install a spacy model via hugging face. The url given by hugging face for the wheels file seems only (?) to provide "any" as the version number. Agreed with @albertotb that from the pip issue referenced it seems like this is probably something hugging face will need to update. I've open a new issue [huggingface/huggingface_hub#2637](https://github.com/huggingface/huggingface_hub/issues/2637)

---

_Comment by @albertotb on 2024-10-27 16:03_

Yes, the workaround I'm using is to stick with pip to install those HuggingFace models and pin pip<=24.0. I guess you could also host a clone of such HuggingFacel models but with the correct naming. That is not ideal if they are being updated regularly, but it's probably fine for most models.

---

_Comment by @jtauber on 2024-11-09 18:36_

can the wheel also be downloaded and renamed?

---

_Comment by @albertotb on 2024-11-16 12:51_

> can the wheel also be downloaded and renamed?

Probably, although not an ideal solution. You would have to re-host it somewhere or pass it around, and you also miss any updates.

---

_Comment by @notatallshaw on 2025-02-19 00:44_

> We should support this, requires some refactors though.

Does uv have a strong reason to support this other than it happens to work with pip?

Pip is moving towards only allowing valid wheels, in fact this was going to be broken by https://github.com/pypa/pip/pull/12917, but as there was no deprication warning I'm going to push it out till pip 25.3: https://github.com/pypa/pip/pull/12917#issuecomment-2667222833, after which, like uv, pip will require wheels to have valid names in all cases.

---

_Comment by @charliermarsh on 2025-02-19 01:24_

Looking back at this, I agree with you that we shouldn't support it. Especially if pip is moving in that direction.

---

_Closed by @charliermarsh on 2025-02-19 01:24_

---
