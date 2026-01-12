```yaml
number: 401
title: "`unresolved attribute` for `requests.exceptions`"
type: issue
state: closed
author: stevelizcano
labels:
  - imports
assignees: []
created_at: 2025-05-15T07:03:27Z
updated_at: 2025-05-15T12:40:42Z
url: https://github.com/astral-sh/ty/issues/401
synced_at: 2026-01-12T15:54:23Z
```

# `unresolved attribute` for `requests.exceptions`

---

_@stevelizcano_

### Summary

When running `uv add requests` to my project, I get the `unresolved attribute` error. I am however able to follow and click the module, and see the code as well.

Specifically:

```
Type `<module 'requests'>` has no attribute `exceptions`ty(unresolved-attribute)
Unknown
(module) exceptions
requests.exceptions
This module contains the set of Requests' exceptions.
```

For the following code:

```py
    except requests.exceptions.RequestException as e:
        logging.error(f"Error downloading PDF from {pdf_url}: {e}")
        raise RuntimeError(f"Failed to download PDF: {e}")
```

Full output with `ty check`:

```sh
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-attribute]: Type `<module 'requests'>` has no attribute `exceptions`
  --> process.py:86:16
   |
84 |             except UnicodeDecodeError:
85 |                 return response.content.decode(response.apparent_encoding, errors='replace')
86 |         except requests.exceptions.RequestException as e:
   |                ^^^^^^^^^^^^^^^^^^^
87 |             print(f"Request failed for {url} (attempt {attempt + 1}/{retries}): {e}")
88 |             if attempt < retries - 1:
   |
info: rule `unresolved-attribute` is enabled by default

error[unresolved-attribute]: Type `<module 'requests'>` has no attribute `exceptions`
   --> process.py:198:12
    |
196 |         print(f"Saved PDF: {file_path}")
197 |         return True
198 |     except requests.exceptions.RequestException as e:
    |            ^^^^^^^^^^^^^^^^^^^
199 |         print(f"Request failed for PDF {url}: {e}")
200 |         return False
    |
info: rule `unresolved-attribute` is enabled by default
```

### Version

ty 0.0.1-alpha.2 (59c45cc60 2025-05-14)

---

_Label `imports` added by @sharkdp on 2025-05-15 08:15_

---

_Comment by @sharkdp on 2025-05-15 08:22_

Thank you for reporting this!

I'm assuming you are attempting to access `requests.exceptions.RequestException` without explicitly importing `requests.exceptions`? Like this?
```py
import requests

requests.exceptions.RequestException  # ty: unresolved-attribute
```
We realize that this works at runtime, but that is just a side-effect from the re-exports [here](https://github.com/psf/requests/blob/c65c780849563c891f35ffc98d3198b71011c012/src/requests/__init__.py#L165-L176). It would be better to use that re-export directly:
```py
requests.RequestException  # works fine
```
or to import `requests.exceptions` explicitly:
```py
import requests
import requests.exceptions

requests.exceptions.RequestException  # works fine
```


That said, we realize that this is a common thing to do (because it happens to work at runtime, in some cases). See https://github.com/astral-sh/ty/issues/133 and #313 for related discussions.

---

_Comment by @stevelizcano on 2025-05-15 11:29_

Ah yes that's right! Thank you

Not sure I should just close the issue then or?

---

_Comment by @sharkdp on 2025-05-15 11:33_

I think we can probably close it in favor of #133 — thank you!

---

_Closed by @sharkdp on 2025-05-15 11:33_

---
