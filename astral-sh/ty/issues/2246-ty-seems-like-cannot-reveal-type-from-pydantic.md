---
number: 2246
title: Ty Seems Like Cannot Reveal Type From Pydantic Validation
type: issue
state: closed
author: scelikcapa
labels:
  - question
assignees: []
created_at: 2025-12-28T09:55:12Z
updated_at: 2025-12-30T07:06:44Z
url: https://github.com/astral-sh/ty/issues/2246
synced_at: 2026-01-10T01:51:14Z
---

# Ty Seems Like Cannot Reveal Type From Pydantic Validation

---

_Issue opened by @scelikcapa on 2025-12-28 09:55_

### Summary

I have validated the request response with a Pydantic model. And explicitly give a return type to my request method. But ty still adds "Unknown" type to the return type

I have added 3 screenshots. 

- One of them is the model
- The second one is the request method
- The third one is the place where I use the request method. And you can see "Unknown" type here. **So I have to type check again with isinstance() unnecessarily**

1- MODEL
<img width="489" height="88" alt="Image" src="https://github.com/user-attachments/assets/cc5001ca-ee34-483e-a43b-5cae66b18ae7" />

2- CLIENT REQUEST METHOD
<img width="821" height="99" alt="Image" src="https://github.com/user-attachments/assets/9f9b14e7-7060-4ee2-8718-e5b8ba31c1fd" />

3- WRONG TYPE
<img width="829" height="230" alt="Image" src="https://github.com/user-attachments/assets/4d0549f6-3d56-4764-899c-b6798c3e0b82" />
### Version

v2025.76.0

---

_Renamed from "Ty Seems Like Cannot Reveal Type From Pdantic Validation" to "Ty Seems Like Cannot Reveal Type From Pydantic Validation" by @scelikcapa on 2025-12-28 10:13_

---

_Comment by @AshishT112203 on 2025-12-28 23:35_

out of curiosity, what's the indicated type when you hover over `result` after the `result = ZohoEstimateGetResponse.model_validate(obj=result)` within the `get_estimate` function

---

_Comment by @MichaReiser on 2025-12-29 10:31_

Can you hover over `self.zoho_books_client` and share what type ty reveals for it? I wonder if it's a union with `Unknown` (or some other type that isn't guaranteed to have a `get_estimate` function.

---

_Label `question` added by @MichaReiser on 2025-12-29 10:31_

---

_Comment by @carljm on 2025-12-29 19:48_

Yes, I suspect that you need something like `zoho_books_client: ZohoBooksClient` (or whatever the type is actually called) on the class body of whatever class includes the `get_quote` method, so we know the type of `self.zoho_books_client` for sure.

---

_Closed by @carljm on 2025-12-29 19:48_

---

_Comment by @scelikcapa on 2025-12-30 06:44_

> out of curiosity, what's the indicated type when you hover over result after the result = ZohoEstimateGetResponse.model_validate(obj=result) within the get_estimate function

hello @AshishT112203 
ZohoEstimate|None

---

_Comment by @scelikcapa on 2025-12-30 06:49_

> Can you hover over `self.zoho_books_client` and share what type ty reveals for it? I wonder if it's a union with `Unknown` (or some other type that isn't guaranteed to have a `get_estimate` function. @MichaReiser @carljm 

yes, it's **Unknown|ZohoBooksClien**t. But this is weird because I see it as **ZohoBooksClient** only in __init__()

<img width="892" height="122" alt="Image" src="https://github.com/user-attachments/assets/a1d8e371-3cf3-4a30-90eb-85390570e40d" />

<img width="851" height="307" alt="Image" src="https://github.com/user-attachments/assets/1183a3e7-235f-4a3c-8127-68fc6c7ef84f" />

---

_Comment by @scelikcapa on 2025-12-30 06:53_

I have a type annotation, so this is not the same case as another issue, I think @carljm @MichaReiser 

But Ty change the type and adds Unknown to my type annotation in __init__() when self.zoho_books_client is used in another function



---

_Comment by @carljm on 2025-12-30 06:56_

@scelikcapa can you show the actual code of your `__init__` method? A type annotation on the parameter to `__init__` is not (currently) enough, there would need to be one on the `self.zoho_books_client = ...` assignment. Or else a class-level `zoho_books_client: ZohoBooksClient` annotation. 

---

_Comment by @scelikcapa on 2025-12-30 06:58_

@carljm 

```
class ZohoService:
    def __init__(self, zoho_crm_client: ZohoCrmClient, zoho_books_client: ZohoBooksClient):
        self.zoho_crm_client = zoho_crm_client
        self.zoho_books_client = zoho_books_client


    async def get_quote(self, quote_id: int) -> ZaqQuote:
        logger.info({"event": "fetching_zoho_quote", "quote_id": quote_id})
        zoho_estimate = await self.zoho_books_client.get_estimate(estimate_id=quote_id)
        if not isinstance(zoho_estimate, ZohoEstimate):
            raise ZohoQuoteNotFoundException(quote_id=quote_id)

        if not zoho_estimate.custom_field_hash.aq_project_id:
            raise ZohoQuoteDoNotLinkToAqProjectException(quote_id=quote_id)

        quote = Mapper.ZohoToDomain.estimate_to_zaq_quote(estimate=zoho_estimate)
        return quote
```

---

_Comment by @scelikcapa on 2025-12-30 07:02_

@carljm yes, when I add to type directly to self.zoho_books_client like below, it solves the issue

```
class ZohoService:
    def __init__(self, zoho_crm_client: ZohoCrmClient, zoho_books_client: ZohoBooksClient):
        self.zoho_crm_client = zoho_crm_client
        self.zoho_books_client: ZohoBooksClient = zoho_books_client
```

---

_Comment by @scelikcapa on 2025-12-30 07:06_

@carljm It would be better if ty reveals a type from __init__. 

But by the way, great language server. Very appreciated.

I remove Pylance. It kills my RAM and computer, which have 16GB. Very happy to find a solution with ty

---
