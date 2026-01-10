```yaml
number: 1850
title: Ignoring module types entirely
type: issue
state: closed
author: galah92
labels:
  - question
assignees: []
created_at: 2025-12-11T07:43:21Z
updated_at: 2025-12-11T09:29:20Z
url: https://github.com/astral-sh/ty/issues/1850
synced_at: 2026-01-10T01:56:41Z
```

# Ignoring module types entirely

---

_Issue opened by @galah92 on 2025-12-11 07:43_

### Question

Using `from transformers import AutoModel, AutoTokenizer`, since `transformers` is poorly typed, I'm getting many usage errors from `AutoModel` and `AutoTokenizer`. Is it possible to ignore the entire `transformers` package from `ty` perspective?

### Version

_No response_

---

_Label `question` added by @galah92 on 2025-12-11 07:43_

---

_Comment by @sharkdp on 2025-12-11 07:47_

Thank you for reporting this.

> I'm getting many usage errors from `AutoModel` and `AutoTokenizer`

Can you please share some code and some of the errors that you are seeing?

> since `transformers` is poorly typed

Ideally, ty shouldn't raise a lot of errors when using classes and functions from *untyped* libraries. What exactly do you mean by "poorly typed"?

---

_Comment by @galah92 on 2025-12-11 08:10_

> Can you please share some code and some of the errors that you are seeing?

```python
from transformers import AutoModel, AutoTokenizer

# Global cache pattern (common in ML code)
_model: AutoModel | None = None
_tokenizer: AutoTokenizer | None = None


def load_model() -> tuple[AutoModel, AutoTokenizer]:
    """ty error: invalid-return-type due to None narrowing not working."""
    global _model, _tokenizer
    if _model is not None:
        # ty: tuple[..., AutoTokenizer | None] not assignable to tuple[..., AutoTokenizer]
        return _model, _tokenizer
    _tokenizer = AutoTokenizer.from_pretrained("bert-base-uncased")
    # ty warning: possibly-missing-attribute on from_pretrained
    _model = AutoModel.from_pretrained("bert-base-uncased")
    return _model, _tokenizer


def count_tokens(text: str, tokenizer: AutoTokenizer | None) -> int:
    """ty error: unresolved-attribute on encode()."""
    if tokenizer:
        # ty: Object of type `AutoTokenizer` has no attribute `encode`
        return len(tokenizer.encode(text))
    return len(text.split())


def run_inference(model: AutoModel, tokenizer: AutoTokenizer) -> str:
    """ty error: unresolved-attribute on custom methods like infer()."""
    # ty: Object of type `AutoModel | AutoModel` has no attribute `infer`
    # (DeepSeek-OCR adds infer() method, but even standard methods cause issues)
    output = model.infer(tokenizer, prompt="Hello")
    return output
```

It looks like `transformers` doesn't provide better classes to use that has `from_pretrained` / `encode`.

> Ideally, ty shouldn't raise a lot of errors when using classes and functions from _untyped_ libraries. What exactly do you mean by "poorly typed"?

It's not that classes are untyped entirely, it's just "poorly". I expected `AutoModel` to have `from_pretrained` or to inherit from a base class that has that, for example.

---

_Comment by @sharkdp on 2025-12-11 08:29_

I see, thank you for providing examples.

> ```py
> global _model, _tokenizer
>     if _model is not None:
>         # ty: tuple[..., AutoTokenizer | None] not assignable to tuple[..., AutoTokenizer]
> ```

This is not related to `transformers`. You probably want to check `if _model is not None and _tokenizer is not None`, or otherwise `ty` can't narrow `_tokenizer` from `AutoTokenizer | None` to `AutoTokenizer`



> ```py
>     _tokenizer = AutoTokenizer.from_pretrained("bert-base-uncased")
>     # ty warning: possibly-missing-attribute on from_pretrained
> ```

I can't reproduce this. It also doesn't seem like an error that ty would throw? Did you see a `possibly-missing-attribute` on `AutoTokenizer` or `AutoModel` instead?


> ```py
>         # ty: Object of type `AutoTokenizer` has no attribute `encode`
>         return len(tokenizer.encode(text))
> ```

Does this code work? I couldn't find this method in the documentation, and if I try it at runtime, it fails:

```pycon
>>> from transformers import AutoTokenizer
None of PyTorch, TensorFlow >= 2.0, or Flax have been found. Models won't be available and only tokenizers, configuration and file/data utilities can be used.
>>> AutoTokenizer.encode
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    AutoTokenizer.encode
AttributeError: type object 'AutoTokenizer' has no attribute 'encode'
```

---

_Comment by @galah92 on 2025-12-11 08:49_

> > global _model, _tokenizer
> >     if _model is not None:
> >         # ty: tuple[..., AutoTokenizer | None] not assignable to tuple[..., AutoTokenizer]
> 
> This is not related to `transformers`. You probably want to check `if _model is not None and _tokenizer is not None`, or otherwise `ty` can't narrow `_tokenizer` from `AutoTokenizer | None` to `AutoTokenizer`

Correct, sorry for that.

> > _tokenizer = AutoTokenizer.from_pretrained("bert-base-uncased")
> >     # ty warning: possibly-missing-attribute on from_pretrained
> 
> I can't reproduce this. It also doesn't seem like an error that ty would throw? Did you see a `possibly-missing-attribute` on `AutoTokenizer` or `AutoModel` instead?

Yes, on `AutoModel`. 

> > ```python
> > # ty: Object of type `AutoTokenizer` has no attribute `encode`
> >         return len(tokenizer.encode(text))
> > ```
> 
> Does this code work? I couldn't find this method in the documentation, and if I try it at runtime, it fails:

It works at runtime, I guess it depends on the exact tokenizer that's being loaded, so it's like a factory.

---

EDIT: to clarify, I'm working with https://huggingface.co/deepseek-ai/DeepSeek-OCR.

It looks like they defined their own `DeepSeek` types but these might not be available until a newer `transformers` version will be released, which is why `AutoModel` & `AutoTokenizer` are helpful. So they aim to operate without type checking, and given that, I wonder if it's possible to "shut-down" `ty` for the entire package.

---

_Comment by @sharkdp on 2025-12-11 09:08_

> > I can't reproduce this. It also doesn't seem like an error that ty would throw? Did you see a `possibly-missing-attribute` on `AutoTokenizer` or `AutoModel` instead?
> 
> Yes, on `AutoModel`.

In that case, can you please share the actual ty error message? Ideally, the full diagnostic that you get when running `ty check` from the terminal?


> > > ```py
> > > # ty: Object of type `AutoTokenizer` has no attribute `encode`
> > >         return len(tokenizer.encode(text))
> > > ```
> > 
> > 
> > Does this code work? I couldn't find this method in the documentation, and if I try it at runtime, it fails:
> 
> It works at runtime, I guess it depends on the exact tokenizer that's being loaded, so it's like a factory.

One trick to work around this would be to add a new class that inherits from `AutoTokenizer` and `Any`. This will still give you autocompletion for all methods that actually exist on `AutoTokenizer`, but silence any errors if you access attributes that do not (seem to) exist:

```py
from transformers import AutoTokenizer as _AutoTokenizer

class AutoTokenizerAutoTokenizer, Any):
    pass

def f(tokenizer: AutoTokenizer):
    tokenizer.encode()  # no error here
```
See full example in this playground: https://play.ty.dev/625acadd-e05c-40cb-a547-d2029ee43282

A related method that is specific to ty is to use intersection types. This way, you could intersect `AutoTokenizer & Any` to achieve the same effect:

```py
if TYPE_CHECKING:
    from ty_extensions import Intersection
    type AutoTokenizer = Intersection[_AutoTokenizer, Any]
```
See full example in this playground: https://play.ty.dev/392d85ad-5916-42fe-be92-6289b111c604

And finally, you can always just type `tokenizer` as `Any`. This does not give you auto-completion, but also silences all errors.

---

_Comment by @galah92 on 2025-12-11 09:28_

Ok cool, this definitely answers my use case. Thanks!

---

_Closed by @sharkdp on 2025-12-11 09:29_

---
