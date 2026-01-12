```yaml
number: 18146
title: "ruff format breaking # noqa: B008"
type: issue
state: closed
author: waterworthd-cim
labels: []
assignees: []
created_at: 2025-05-17T01:41:30Z
updated_at: 2025-05-17T14:26:57Z
url: https://github.com/astral-sh/ruff/issues/18146
synced_at: 2026-01-12T15:54:56Z
```

# ruff format breaking # noqa: B008

---

_@waterworthd-cim_

I'm not sure if this is a duplicate, I'm not sure what to search for

I'm using dlthub, which makes heavy use of decorators, dependency inject and defaults which violate B008, i.e.

```
@dlt.resource()
def data_reader(
    credentials: ConnectionStringCredentials = dlt.secrets.value,
    incremental: dlt.sources.incremental = dlt.sources.incremental(  "xmin", initial_value=0) # noqa: B008
) -> Iterable[pyarrow.Table]:
    pass
```

The issue is after adding `# noqa: B008` my `ruff format` pre-commit hook reformatted the line to 

```
@dlt.resource()
def data_reader(
    credentials: ConnectionStringCredentials = dlt.secrets.value,
    incremental: dlt.sources.incremental = dlt.sources.incremental(  
        "xmin", initial_value=0
    ) # noqa: B008
) -> Iterable[pyarrow.Table]:
    pass
```

And then triggered B008 again on the first line, to fix I had to move to # noqa, i.e.

```
@dlt.resource()
def data_reader(
    credentials: ConnectionStringCredentials = dlt.secrets.value,
    incremental: dlt.sources.incremental = dlt.sources.incremental(  # noqa: B008
        "xmin", initial_value=0
    ) 
) -> Iterable[pyarrow.Table]:
    pass
```

It seems that it's not correctly identifying where the "line" ends - the hint seems to have to be applied at the end of the first line of a multi-line statement, but that's not safe if `ruff format` breaks a hinted line?

---

_Closed by @MichaReiser on 2025-05-17 14:26_

---
