```yaml
number: 3335
title: "Add Azure Devops as a `-format` option."
type: pull_request
state: merged
author: StefanBRas
labels: []
assignees: []
merged: true
base: main
head: stefanbras/azure-devops-format
created_at: 2023-03-04T13:17:11Z
updated_at: 2023-03-06T02:48:40Z
url: https://github.com/astral-sh/ruff/pull/3335
synced_at: 2026-01-12T15:55:12Z
```

# Add Azure Devops as a `-format` option.

---

_@StefanBRas_

Adds option to print messages in an error format for azure pipelines, following https://learn.microsoft.com/en-us/azure/devops/pipelines/scripts/logging-commands?view=azure-devops&tabs=bash#usage.

In the command line it looks like:
```bash
> cargo run -p ruff_cli -- check ../python_testing/example_tag.py --no-cache --format azure-devops
##vso[task.logissue type=error;sourcepath=/home/stefan/dev/python_testing/example_tag.py;linenumber=2;columnnumber=8;code=F401;]/home/stefan/dev/python_testing/example_tag.py:2:8: F401 `math` imported but unused
##vso[task.logissue type=error;sourcepath=/home/stefan/dev/python_testing/example_tag.py;linenumber=3;columnnumber=8;code=F401;]/home/stefan/dev/python_testing/example_tag.py:3:8: F401 `sys` imported but unused
```

In the pipeline logs it shows as:
![image](https://user-images.githubusercontent.com/28559054/222903613-7322cf5a-70eb-4cfd-8cbc-ce492395e5b4.png)
And in the pipeline summary it shows as:
![image](https://user-images.githubusercontent.com/28559054/222903639-b90ea063-0286-4afb-b4af-1b1bda1ca3e4.png)

Which isn't particularly pretty but apparently how Azure Devops think it should be displayed.

---

_Comment by @charliermarsh on 2023-03-04 19:18_

Thanks!

> I'm not sure if it makes sense have the error information in both the message and in the logging metadata, as it's then displayed twice but I just followed the same structure as the Github formatting. I can remove it from the message if you want :)

If the messages appear in the logs without repeating, then they can be omitted. I think we repeat them with GitHub because if you _only_ output the annotations, the messages appear in the PR view but not in the raw logs.


---

_Comment by @charliermarsh on 2023-03-04 19:18_

Would it be reasonable to shorthand this to `azure`, rather than `azure-devops`?

---

_Comment by @StefanBRas on 2023-03-04 22:22_

Great, I'll remove the duplicate info then :) 

I'm not sure about the renaming, since just "azure" refers to lots of different services where DevOps is one of them. But I can't come up with any other service that needs to emit logs from ruff, so it's probably fine.

I have no strong opinions either way, so I'll just rename it :)

---

_Merged by @charliermarsh on 2023-03-06 02:48_

---

_Closed by @charliermarsh on 2023-03-06 02:48_

---
