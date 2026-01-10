---
number: 6761
title: "Formatter: Omit empty line for trailing comments before class/function"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-08-22T09:21:50Z
updated_at: 2023-08-31T20:55:07Z
url: https://github.com/astral-sh/ruff/issues/6761
synced_at: 2026-01-10T01:22:45Z
---

# Formatter: Omit empty line for trailing comments before class/function

---

_Issue opened by @MichaReiser on 2023-08-22 09:21_

Ruff separates trailing own line comments of a statement before a class or function definition with one or two empty lines. Black avoids adding additional empty lines. [Playground](https://play.ruff.rs/7ce5a691-e463-4d0f-aae5-938a66fe9750)

## Input / Expected

```python
DEFAULT_TEMPLATE = "flatpages/default.html"

# This view is called from FlatpageFallbackMiddleware.process_response
# when a 404 is raised, which often means CsrfViewMiddleware.process_view
# has not been called even if CsrfViewMiddleware is installed. So we need
# to use @csrf_protect, in case the template needs {% csrf_token %}.
# However, we can't just wrap this view; if no matching flatpage exists,
# or a redirect is required for authentication, the 404 needs to be returned
# without any CSRF checks. Therefore, we only
# CSRF protect the internal implementation.


def flatpage(request, url):
    pass

class OracleGeometryColumns(models.Model):
    "Maps to the Oracle USER_SDO_GEOM_METADATA table."
    table_name = models.CharField(max_length=32)
    column_name = models.CharField(max_length=1024)
    srid = models.IntegerField(primary_key=True)
    # TODO: Add support for `diminfo` column (type MDSYS.SDO_DIM_ARRAY).

    class Meta:
        app_label = "gis"
        db_table = "USER_SDO_GEOM_METADATA"
        managed = False
```

## Ruff / Actual
```python
DEFAULT_TEMPLATE = "flatpages/default.html"


# This view is called from FlatpageFallbackMiddleware.process_response
# when a 404 is raised, which often means CsrfViewMiddleware.process_view
# has not been called even if CsrfViewMiddleware is installed. So we need
# to use @csrf_protect, in case the template needs {% csrf_token %}.
# However, we can't just wrap this view; if no matching flatpage exists,
# or a redirect is required for authentication, the 404 needs to be returned
# without any CSRF checks. Therefore, we only
# CSRF protect the internal implementation.


def flatpage(request, url):
    pass


class OracleGeometryColumns(models.Model):
    srid = models.IntegerField(primary_key=True)

    # TODO: Add support for `diminfo` column (type MDSYS.SDO_DIM_ARRAY).

    class Meta:
        app_label = "gis"

```




---

_Label `formatter` added by @MichaReiser on 2023-08-22 09:21_

---

_Comment by @MichaReiser on 2023-08-22 09:22_

Or is this an intentional deviation @charliermarsh ?

---

_Comment by @charliermarsh on 2023-08-22 13:14_

I can't find the PR but I believe this was known and accepted at time of implementation, although I think it's worth fixing.

Note that the behavior is that Black treats the comment differently based on whether there are any empty lines between the comment and the function. If there _aren't_, Black will add blank lines before the comment: https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AF2AMFdAD2IimZxl1N_WjYyyr4iqitRzzCWxGFAvKJnUXkiuPUCGcvBibnBc2lXPBkOM_Uv5mTMePWWoOm57otZ_15QE-R8a6mjfCdIipaT4wf49FfFG_EOYGrjI_oG_zU682rpWS2HX6ez5R4M74gPZ5LCXBL4dSXbWboILAswPa9HMzARHAB6opF0txsiYPupVB_8e59KyKenIY-DACBsY5noOZqBtLAGE1glPL2tGDKd78gE_D4nb3Qk8zUGGs5XAaFfz0UAAAAAdWWhgtJpvi8AAd0B9wIAAOwIoEixxGf7AgAAAAAEWVo=


---

_Comment by @charliermarsh on 2023-08-22 13:14_

I think ghost of @MichaReiser past said this behavior was strange and so we left it as-is.

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-22 14:15_

---

_Comment by @MichaReiser on 2023-08-22 14:26_

> I think ghost of @MichaReiser past said this behavior was strange and so we left it as-is.

Ahh @MichaReiser ... He says a lot when the day is long

We can decide not to fix this but it shows up a lot in the django diff.

---

_Comment by @charliermarsh on 2023-08-22 14:32_

I think we should probably fix, though it might be annoying.

---

_Comment by @MichaReiser on 2023-08-29 14:30_

@charliermarsh this is the issue we talked about

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-29 14:56_

---

_Comment by @charliermarsh on 2023-08-30 00:44_

This is quite tricky but I am working on it :)

---

_Referenced in [astral-sh/ruff#6999](../../astral-sh/ruff/pulls/6999.md) on 2023-08-30 02:15_

---

_Closed by @charliermarsh on 2023-08-31 20:55_

---
