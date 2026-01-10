```yaml
number: 16969
title: "`F841 and F811` On match-case expression"
type: issue
state: closed
author: slothyrulez
labels:
  - question
assignees: []
created_at: 2025-03-25T14:16:09Z
updated_at: 2025-03-27T19:48:53Z
url: https://github.com/astral-sh/ruff/issues/16969
synced_at: 2026-01-10T11:09:58Z
```

# `F841 and F811` On match-case expression

---

_Issue opened by @slothyrulez on 2025-03-25 14:16_

### Summary

Getting a F841 and F811 in a match-case expression:

```
from typing import Literal

TLLM_PROVIDER_OPENAI = Literal['openai']
LLM_PROVIDER_OPENAI: TLLM_PROVIDER_OPENAI = 'openai'
TLLM_PROVIDER_OPENAI_WITH_AZURE = Literal['openai_azure']
LLM_PROVIDER_OPENAI_WITH_AZURE: TLLM_PROVIDER_OPENAI_WITH_AZURE = 'openai_azure'
TLLM_PROVIDER_ANTHROPIC = Literal['anthropic']
LLM_PROVIDER_ANTHROPIC: TLLM_PROVIDER_ANTHROPIC = 'anthropic'

TLLMProvider = Literal[
    TLLM_PROVIDER_OPENAI, TLLM_PROVIDER_OPENAI_WITH_AZURE, TLLM_PROVIDER_ANTHROPIC
]


def get_provider(provider: TLLMProvider) -> TLLMProvider:
    from . import LLM_PROVIDER_OPENAI, LLM_PROVIDER_OPENAI_WITH_AZURE, LLM_PROVIDER_ANTHROPIC
    match provider:
        case LLM_PROVIDER_OPENAI:
            return 'LLM_PROVIDER_OPENAI'
        case LLM_PROVIDER_OPENAI_WITH_AZURE:
            return 'LLM_PROVIDER_OPENAI_WITH_AZURE'
        case LLM_PROVIDER_ANTHROPIC:
            return 'LLM_PROVIDER_ANTHROPIC'
        case _:
            raise ValueError(f'Unknown LLM provider: {provider}')

```

https://play.ruff.rs/1cdedaef-a8c2-4248-ab86-08121086cf06

```
... F841 Local variable `LLM_PROVIDER_ANTHROPIC` is assigned to but never used
    |
123 |         case LLM_PROVIDER_OPENAI_WITH_AZURE:
124 |             return openai.LLM.with_azure(model=model_id, temperature=0, api_key=get_config('AZURE_OPENAI_API_KEY'))
125 |         case LLM_PROVIDER_ANTHROPIC:
    |              ^^^^^^^^^^^^^^^^^^^^^^ F841
126 |             return anthropic_llm.LLM(model=model_id, temperature=0, api_key=get_config('ANTHROPIC_API_KEY'))
127 |         case _:
    |
    = help: Remove assignment to unused variable `LLM_PROVIDER_ANTHROPIC`

... F811 Redefinition of unused `LLM_PROVIDER_OPENAI` from line 142
    |
148 |     match active_provider:
149 |         case LLM_PROVIDER_OPENAI:
    |              ^^^^^^^^^^^^^^^^^^^ F811
150 |             failover_providers = [LLM_PROVIDER_OPENAI_WITH_AZURE, LLM_PROVIDER_ANTHROPIC]
151 |         case LLM_PROVIDER_OPENAI_WITH_AZURE:
    |
    = help: Remove definition: `LLM_PROVIDER_OPENAI`
```

The second error, F811 comes from inside the function defining the match-case, that imports the literal



### Version
0.9.9 and 0.11.2

---

_Renamed from "`F841 and F811` On match-case exp`ression" to "`F841 and F811` On match-case expression" by @slothyrulez on 2025-03-25 14:16_

---

_Comment by @ntBre on 2025-03-25 17:34_

The `case`s are shadowing the variables declared above, not treating them as constants. See the section on `match` statements just above [this link](https://docs.python.org/3/tutorial/controlflow.html#defining-functions) in the docs:

> Patterns may use named constants. These must be dotted names to prevent them from being interpreted as capture variable

Here's another example:

```pycon
>>> LLM_PROVIDER_OPENAI = 4
>>> match 2:
...     case LLM_PROVIDER_OPENAI:
...         print(LLM_PROVIDER_OPENAI)
...
2
```

---

_Label `question` added by @ntBre on 2025-03-25 17:34_

---

_Comment by @slothyrulez on 2025-03-27 17:21_

TIL! thanks @ntBre Sorry for your lost time

---

_Closed by @slothyrulez on 2025-03-27 17:21_

---

_Comment by @slothyrulez on 2025-03-27 17:23_

For future references, I fixed it like:

```
def get_provider(provider: TLLMProvider) -> TLLMProvider:
    from fu import constants
    match provider:
        case constants.LLM_PROVIDER_OPENAI:
            return 'LLM_PROVIDER_OPENAI'
        case constants.LLM_PROVIDER_OPENAI_WITH_AZURE:
            return 'LLM_PROVIDER_OPENAI_WITH_AZURE'
        case constants.LLM_PROVIDER_ANTHROPIC:
            return 'LLM_PROVIDER_ANTHROPIC'
        case _:
            raise ValueError(f'Unknown LLM provider: {provider}')
```

---

_Comment by @ntBre on 2025-03-27 19:48_

No worries, this has tripped me up before too!

---
