```yaml
number: 2025
title: Overload not found when calling method that uses Unpack
type: issue
state: closed
author: steved-stripe
labels: []
assignees: []
created_at: 2025-12-17T17:44:46Z
updated_at: 2025-12-17T17:46:02Z
url: https://github.com/astral-sh/ty/issues/2025
synced_at: 2026-01-10T01:53:59Z
```

# Overload not found when calling method that uses Unpack

---

_Issue opened by @steved-stripe on 2025-12-17 17:44_

### Summary

```python
import stripe
# ---snip---
customer_id: str = customer.id
payment_method = stripe.PaymentMethod.create(
    type="card",
    card={
        "number": "4242424242424242",
        "exp_month": 8,
        "exp_year": 2027,
        "cvc": "314",
    },
)
payment_method.attach(customer=customer)
# ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

```
No overload of function `attach` matches argumentsty[no-matching-overload](https://ty.dev/rules#no-matching-overload)
_payment_method.py(2463, 9): First overload defined here
_payment_method.py(2505, 9): Overload implementation defined here
```


where the `attach` method looks like:

```py
    @overload
    def attach(
        self, **params: Unpack["PaymentMethod.AttachParams"]
    ) -> "PaymentMethod":
        ...
```


and then:

```py
class PaymentMethod(
    CreateableAPIResource["PaymentMethod"],
    ListableAPIResource["PaymentMethod"],
    UpdateableAPIResource["PaymentMethod"],
):
    # ---snip---
    class AttachParams(RequestOptions):
        customer: str
        expand: NotRequired[List[str]]
```


https://play.ty.dev/bd02de97-6f9c-42ec-8031-961704966f24


hovering over the params I see:

```py
(
    payment_method: str,
    **params: @Todo
) -> PaymentMethod
(
    self,
    **params: @Todo
) -> PaymentMethod
```

### Version

astral-sh.ty 2025.72.0

---

_Comment by @AlexWaygood on 2025-12-17 17:46_

Thanks for the report. I think this can be folded into #1746

---

_Closed by @AlexWaygood on 2025-12-17 17:46_

---
