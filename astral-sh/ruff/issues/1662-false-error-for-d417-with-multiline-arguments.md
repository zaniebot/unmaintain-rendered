```yaml
number: 1662
title: False error for D417 with multiline arguments
type: issue
state: closed
author: cazador481
labels:
  - docstring
assignees: []
created_at: 2023-01-05T14:51:39Z
updated_at: 2023-01-05T19:36:20Z
url: https://github.com/astral-sh/ruff/issues/1662
synced_at: 2026-01-12T15:54:41Z
```

# False error for D417 with multiline arguments

---

_@cazador481_

I am getting the following error:
```
D417 Missing argument descriptions in the docstring: `endpoint\`, \`token\`
```
when running ruff with 
```toml
[tool.ruff.pydocstyle]
convention = 'google'
```
On the following code block:
```python
def init_otel(
    console: bool = False,
    default_provider: bool = True,
    service_name: Optional[str] = None,
    endpoint: Optional[str] = None,
    token: Optional[str] = None,
    version: Optional[str] = None,
    resource: Optional[resources.Resource] = None,
) -> Optional[TracerProvider]:
    """Initialize otel telemetry connections.

    Args:
        console: Send to console if true, otherwise send to endpoint
        service_name: Service name, picks up the default HWINFCI's service name
        version: service version. Will try to automatically pick this up.
        default_provider: Is this the default provider for OTEL
        endpoint:
            http otel endpoint
            Overridden with ENV[HWINFCI_OTEL_ENDPONT].  If set to console, will output to console
        resource: Additional resources to add
        token:
            OTEL authorization token
            Overridden with ENV[HWINFCI_OTEL_TOKEN]

    Returns:
        Optionally returns a TracerProvider
    """
```

When running through flake8 I don't get that error.

---

_Label `docstring` added by @charliermarsh on 2023-01-05 16:03_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-05 19:10_

---

_Comment by @charliermarsh on 2023-01-05 19:11_

Will fix this today, thanks.

---

_Closed by @charliermarsh on 2023-01-05 19:36_

---
