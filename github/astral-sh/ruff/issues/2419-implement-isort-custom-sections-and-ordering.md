---
number: 2419
title: "Implement `isort` custom sections and ordering"
type: issue
state: closed
author: ngnpope
labels:
  - isort
assignees: []
created_at: 2023-01-31T21:46:43Z
updated_at: 2023-05-04T13:03:10Z
url: https://github.com/astral-sh/ruff/issues/2419
synced_at: 2026-01-07T13:12:14-06:00
---

# Implement `isort` custom sections and ordering

---

_Issue opened by @ngnpope on 2023-01-31 21:46_

`isort` has support for configuring [custom sections and ordering](https://pycqa.github.io/isort/docs/configuration/custom_sections_and_ordering.html).

This support would also need to be tied in with [`no-lines-before`](https://github.com/charliermarsh/ruff#no-lines-before) for the custom sections.

I would imagine that the following `isort` configuration:

```ini
known_django=django
known_pandas=pandas,numpy
sections=FUTURE,STDLIB,DJANGO,THIRDPARTY,PANDAS,FIRSTPARTY,LOCALFOLDER
no_lines_before=DJANGO,LOCALFOLDER
```

Would become the following in `ruff`:

```toml
[tool.ruff.isort]
no-lines-before = ["django", "local-folder"]
section-order = ["future", "standard-library", "django", "third-party", "pandas", "first-party", "local-folder"]

[tool.ruff.isort.sections]
django = ["django"]
pandas = ["pandas", "numpy"]
```

---

_Label `isort` added by @charliermarsh on 2023-01-31 23:06_

---

_Comment by @spaceone on 2023-02-01 18:31_

Support for `known-local-folder` would be nice.

---

_Comment by @charliermarsh on 2023-02-01 18:40_

Isn't `local-folder` always denoted by a relative import? What does `known-local-folder` do?

---

_Comment by @spaceone on 2023-02-01 18:51_

local-folder is the name of the section/category while known-local-folder are the modules/packages which should be treated for the local-folder section.

e.g.:
```
sections=FUTURE,STDLIB,FIRSTPARTY,THIRDPARTY,LOCALFOLDER
known_local_folder=myproject,test,foo
```

it has nothing to do with relative imports.

---

_Comment by @g-as on 2023-02-01 18:57_

Aren't you mixing `known_first_party` and `known_local_folder`?

https://pycqa.github.io/isort/docs/configuration/options.html#known-first-party

https://pycqa.github.io/isort/docs/configuration/options.html#known-local-folder
> Force isort to recognize a module as being a local folder. Generally, this is reserved for relative imports (from . import module).



---

_Comment by @charliermarsh on 2023-02-01 19:04_

I guess `isort` supports including non-relative imports in local folder? Strikes me as unusual!

---

_Comment by @spaceone on 2023-02-08 12:24_

> Aren't you mixing `known_first_party` and `known_local_folder`?

Kind of yes, but we use this to separate our "known first party" modules from some special modules (some only for "testing" and one which can only be imported in a special C application): We use it as visual separation from other modules.

> I guess `isort` supports including non-relative imports in local folder? Strikes me as unusual!
yes, I think the implementation of it is just a regular "section", so no special behavior.

→ So for me it's not important if I just add another section which I name by myself or use the standard isort section "known-local-folder".

---

_Referenced in [astral-sh/ruff#2657](../../astral-sh/ruff/pulls/2657.md) on 2023-02-08 13:31_

---

_Comment by @QuentinN42 on 2023-02-26 21:09_

Hey, I'll try to implement the `[tool.ruff.isort.sections]` feature :)

---

_Comment by @QuentinN42 on 2023-02-27 00:05_

After reading the code, I think I first need to pass an MR about allowing section order.

If I understand right the code in the [PyCQA/isort GH](https://github.com/PyCQA/isort), they [return a str in the place module](https://github.com/PyCQA/isort/blob/3a72e069635a865a92b8a0273aa829f630cbcd6f/isort/place.py#L15) to be able to [order it dynamically in the place module](https://github.com/PyCQA/isort/blob/3a72e069635a865a92b8a0273aa829f630cbcd6f/isort/parse.py#L455).

@charliermarsh today we use an enum `ImportType` to order all imports :

https://github.com/charliermarsh/ruff/blob/36d134fd41a8378f1c3f7bb69a7053ecefb17cff/crates/ruff/src/rules/isort/categorize.rs#L14-L24

What do you think about changing that to a BTreeMap or an HashMap to allow runtime update of it (custom categorize) ?

---

_Comment by @charliermarsh on 2023-02-28 17:32_

I wish I had some way to assess the importance or popularity of this functionality, since it does introduce a bunch of complexity. Does anyone want to make a strong case for it?

(I personally tend not to use any of isort's configuration options, but I've been open to adding them over time as other's have put together PRs, since they generally haven't introduced _too_ much complexity. Though I may end up regretting supporting those over time, we'll see :))

(Having not looked at the code much...) if we do go forward with this, I might suggest adding a `UserDefined(String)` variant to `ImportType`, rather than changing to to a stringy-typed map. Or even treating that as an entirely separate struct, like:

```rs
pub enum Section {
  KnownType(ImportType),
  UserDefined(String),
}
```

Since we're dealing with two orthogonal concepts: whether a module is first-part or third-party etc., vs. whether it should be categorized in a certain section.


---

_Comment by @Redoubts on 2023-02-28 19:28_

I suspect this is going to be popular in the dark matter that is closed-source business code. 

But I think it’s also good if you have something like 

‘’’
from my_special_loader import a_module
import a_module.submodule
‘’’
 isort would reverse the order without the ability to configure sections. 

---

_Comment by @Atilla on 2023-02-28 20:28_

This might not be a very helpful contribution, but:

I am following this because I want to replace our closed-source business code repo linters with ruff. We have a bunch of those `isort` rules, mostly to split django-related 3rd party stuff from the rest of it, as far as I can observe. 

People looked at the diff without this on, and I'm pretty sure I could convince them to just stop using that grouping, some even prefer it. But it's a very large diff in the repo :)  

---

_Comment by @charliermarsh on 2023-02-28 20:42_

No, that's very helpful! I'm just trying to build some intuition for how important it is, so any data points are welcome (this goes for all commenters and onlookers :)).

---

_Comment by @mastacheata on 2023-02-28 22:37_

We have the django stuff as a separate section cause there's usually a lot of that and having that grouped together just looks "nicer".  

Obviously there's no technical need for changing the order or the spacing for such a block, it's just a convention we have in our repo of about 900 files which would be changed should we replace isort with the corresponding selector in ruff.

I guess the same can be argued for pandas as that's the other example in the isort docs.

Just noticed, we could pretty much achieve the same thing by adding django to force-separate, now this adds it to the bottom of the list behind our local packages.
<details>
  <summary>Here's an example of how one of our typical viewsets in DRF looks like: (click to expand)</summary>

```
import csv

from django.db.models import Case, CharField, F, Q, When
from django.http import HttpResponse
from django.shortcuts import get_object_or_404
from django.utils import timezone
from django.utils.timezone import now

from rest_framework import status
from rest_framework.decorators import action
from rest_framework.response import Response
from rest_framework.serializers import Serializer
from rest_framework_serializer_extensions.views import SerializerExtensionsAPIViewMixin

from empto.company.api.filters import CertificateFilter
from empto.company.api.permissions import (
    ChangeCompanyPermission,
    ExportCertificatesPermission,
    ListCertificatePermission,
    ListMyCertificatePermission,
    ReviewCertificatePermission,
)
from empto.company.api.serializers import CertificateListSerializer, CertificateReviewSerializer, CertificateSerializer
from empto.company.models import Certificate, Company
from empto.company.services import CertificateCSVExportService, CompanyCertificateEmailService
from empto.core.api.pagination import StandardResultsSetPagination
from empto.core.api.permissions import EmptoDefaultPermissions
from empto.core.api.views import NoDeleteModelViewSet, UserSpecificQuerySetMixin
from empto.dsz.models import DszContract
from empto.notification.models import Notification, NotificationSubscription
```

</details>

---

_Comment by @QuentinN42 on 2023-02-28 23:53_

It's particulary usefull in the monorepo / big projects : 
You quickly get like 30 lines of imports...

Being able to create some groups like :

- Datascience : `numpy`, `pandas` ...
- Software: `redis`, `pika` ...
- Core: `my_s3`, `my_logger` ...
- Metier: `my_worker`, `scheduler` ...

Transform this :
```python
import numpy
import my_s3
import my_logger
import my_worker
import pandas
import pika
import redis
import scheduler
```
Into this :
```python
import numpy
import pandas

import pika
import redis

import my_s3
import my_logger

import my_worker
import scheduler
```

---

_Comment by @QuentinN42 on 2023-02-28 23:57_

But yes I'ts not required at all as a priority.
I think we can take some time to think about the best way of implementing it here without copying 1 to 1 isort features.
I will read the release notes and issues of this feature on the isort repo and place a summary here so we have some base work material.

---

_Comment by @charliermarsh on 2023-03-01 03:28_

Seems like there's enough demand + concrete use-cases for this to at least be worth exploring, and scoping out how much complexity it actually adds to the implementation.

---

_Referenced in [quantumjot/btrack#249](../../quantumjot/btrack/issues/249.md) on 2023-03-01 16:59_

---

_Comment by @g-as on 2023-03-02 19:23_

My 2c:

one of my use cases, that has not been mentioned yet, is adding a test deps section to isolate test tooling imports in test files:
```toml

known_tests_deps = ["factory", "faker", "pytest", "pytest_django", "respx", "testfixtures", "time_machine"]

```

---

_Comment by @QuentinN42 on 2023-03-06 18:24_

Here is the first MRs in the isort repo : https://github.com/PyCQA/isort/pull/275

I don't find any discussions about the implementation.

Maybe we want to define some sections in a list : thx like

```toml
[[tool.ruff.import-section]]
name="my_section_name"
line-before = true  # or no-lines-before
packages = [ "pandas", "..." ]
```

Then we can order section with a list of string.

Or we can add a `level` (int) in this section that allow us to get rid of this section order string.

PS : this is not a real proposition, I just want to put here as most ideas to be able to take the better decision

---

_Referenced in [theislab/scvelo#1013](../../theislab/scvelo/pulls/1013.md) on 2023-03-15 11:37_

---

_Comment by @vviikk on 2023-03-31 11:26_

Is anyone working on this? We are now using Ruff in a huge repo ❤️ . But `isort` remains in use because have a custom section ordering.

Thanks!

---

_Comment by @charliermarsh on 2023-03-31 14:47_

@vviikk - To my knowledge it's not currently being worked on. Up for grabs if someone wants to take it :)

---

_Comment by @QuentinN42 on 2023-03-31 18:04_

I will be happy to contribute on this but I didn't have much time last month.
Happy to have ideas of implementations or discussions about the following options :

- The "copy isort features 1 to 1"
  - simple migration
  - not really scalable
- The sections as a list with `section-order` (section define an import block but we keep the `section-order` in the `isort` block)
  - Can have custom params in each section
  - Can copy section from others repo easily
  - The import order is easy to view / edit
  - The import order needs to be edited each time a new section is added
- The sections as a list with `level` (section define an import block with a level and the section ordering is computed at runtime)
  - Can have custom params in each section
  - Can copy section from others repo easily
  - The import order may be hard to view / edit in repo with a lot of sections (but can be mitigated with levels with large padding)
  - The import order is automatically computed each time a new section is added

---

_Comment by @rpep on 2023-04-04 10:39_

> I wish I had some way to assess the importance or popularity of this functionality, since it does introduce a bunch of complexity. Does anyone want to make a strong case for it

Just wanted to add a +1 for this - at the moment this would be something that blocks us from adopting Ruff wholesale on a large commercial project. For us, we want (as we do this with isort already) separation out of core Django and Django REST Framework imports from OS imports and then some other tools we use (Pandas, NumPy).

---

_Referenced in [astral-sh/ruff#3900](../../astral-sh/ruff/pulls/3900.md) on 2023-04-06 13:06_

---

_Referenced in [Toufool/AutoSplit#207](../../Toufool/AutoSplit/pulls/207.md) on 2023-04-08 00:24_

---

_Comment by @charliermarsh on 2023-04-13 21:52_

Implemented in #3900.

The API looks like this:

```toml
[tool.ruff.isort]
section-order = ["future", "standard-library", "third-party", "django", "first-party", "local-folder"]

[tool.ruff.isort.sections]
django = ["django"]
```

---

_Closed by @charliermarsh on 2023-04-13 21:52_

---

_Referenced in [epitools/epitools#56](../../epitools/epitools/pulls/56.md) on 2023-04-14 09:11_

---

_Referenced in [quantumjot/btrack#299](../../quantumjot/btrack/pulls/299.md) on 2023-04-14 09:47_

---

_Referenced in [paddyroddy/python-template#22](../../paddyroddy/python-template/pulls/22.md) on 2023-04-14 09:53_

---

_Comment by @mastacheata on 2023-04-14 16:07_

@charliermarsh I think your example for how the API looks is missing the django section in the section-order part (unless this changed between creating the PR and accepting it)

---

_Comment by @charliermarsh on 2023-04-14 16:09_

@mastacheata - Hah, thank you, fixed 🤦 

---

_Comment by @rpep on 2023-04-17 13:42_

Looks great! Thank you for implementing this so quickly :) 

---

_Comment by @charliermarsh on 2023-04-17 18:33_

I think someone commented-then-deleted but yes, this'll go out in the next release this week :)

---

_Comment by @huynguyengl99 on 2023-04-17 18:43_

Hi, the comment was mine, but after checking that the main code is not the pypi latest distribution, I have deleted it 😂 But anyway, thank you a lot for the awesome tool, I will completely migrate to Ruff after the Isort custom feature release 😁. 

---

_Comment by @ddahan on 2023-05-04 10:23_

Thanks for this, it's amazing to be able to get rid of another package!

Is there any plan to update the related JSON schema? I wrote an issue about it: https://github.com/charliermarsh/ruff/issues/4218

---

_Comment by @JonathanPlasse on 2023-05-04 13:03_

- There is a PR opened SchemaStore/schemastore#2923

---

_Referenced in [odoo/odoo#205038](../../odoo/odoo/pulls/205038.md) on 2025-04-08 06:53_

---
