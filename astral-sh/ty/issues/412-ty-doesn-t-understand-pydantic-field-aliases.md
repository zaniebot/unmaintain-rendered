```yaml
number: 412
title: "ty doesn't understand pydantic field aliases"
type: issue
state: closed
author: jlorenzini-itpie
labels:
  - dataclasses
assignees: []
created_at: 2025-05-15T16:45:55Z
updated_at: 2025-05-15T17:55:47Z
url: https://github.com/astral-sh/ty/issues/412
synced_at: 2026-01-10T02:34:09Z
```

# ty doesn't understand pydantic field aliases

---

_Issue opened by @jlorenzini-itpie on 2025-05-15 16:45_

### Summary

I am writing a python program to consume LLM models through openai. I've installed this package: `langchain-openai==0.3.17`

I am instantiating a class named `langchain_openai.chat_models.base.ChatOpenAI`. The ChatOpenAI is a sub class of `langchain_openai.chat_models.base.BaseChatOpenAI`.

In the BaseChatOpenAI, there's the following class definitions:

```
class BaseChatOpenAI(BaseChatModel):
    client: Any = Field(default=None, exclude=True)  #: :meta private:
    async_client: Any = Field(default=None, exclude=True)  #: :meta private:
    root_client: Any = Field(default=None, exclude=True)  #: :meta private:
    root_async_client: Any = Field(default=None, exclude=True)  #: :meta private:
    model_name: str = Field(default="gpt-3.5-turbo", alias="model")
    """Model name to use."""
```

note the model_name has a pydantic field assigned to it where there's an alias of model for `model_name`. ty check fails if model is used:

```
error[unknown-argument]: Argument `model` does not match any known parameter
  --> netdiscover/chat_host/llm_funcs.py:19:27
   |
17 |         return ChatOllama(model=model_name)
18 |     elif provider == "openai":
19 |         return ChatOpenAI(model=model_name)
   |                           ^^^^^^^^^^^^^^^^
20 |     else:
21 |         raise ValueError(f"Unsupported provider: {provider}. Set LLM_PROVIDER to 'ollama' or 'openai'")
```

But succeeds if the model_name is used.

### Version

0.0.1-alpha.2 (59c45cc60 2025-05-14)

---

_Label `dataclasses` added by @sharkdp on 2025-05-15 17:55_

---

_Comment by @sharkdp on 2025-05-15 17:55_

Thank you for reporting this.

We do not support some of the more advanced features of dataclass fields, yet. See #111 for the issue that keeps track of this feature.

---

_Closed by @sharkdp on 2025-05-15 17:55_

---
