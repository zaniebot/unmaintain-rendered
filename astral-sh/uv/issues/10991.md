```yaml
number: 10991
title: "Support disabling workspace and dev installation of `uv pip install`"
type: issue
state: closed
author: potiuk
labels:
  - enhancement
assignees: []
created_at: 2025-01-27T16:25:59Z
updated_at: 2025-01-27T18:55:37Z
url: https://github.com/astral-sh/uv/issues/10991
synced_at: 2026-01-10T04:27:58Z
```

# Support disabling workspace and dev installation of `uv pip install`

---

_Issue opened by @potiuk on 2025-01-27 16:25_

### Summary

While moving Airlfow to use workspace (which is great feature of uv) - https://github.com/apache/airflow/issues/46045 I foud out that `uv pip install` behaviour for NON EDITABLE installation when project has workspace defined in `pyproject.toml` is somewhat broken. It's not a really a bug, just the behaviour is different to what you would expect and it lacks control of what you would like to install there.

## A bit of context

On of the usages of `uv` we have in airflow, is to prepare constraints resulting in:

* installing airflow IN NON-EDITABLE MODE from local sources + pyproject.toml
* Installing all providers from PyPI (!) in NON-EDITABLE MODE but with --resolution highest
* freeze all those installed packages to a file and publish it as "latest constraints"

There are good reasons we are doing it - mostly we do it in order to produce a somewhat reproducible installation for our users when we release airlfow - and the version we do in `main` is more of "let's see if it still works" - we only actually use those `pypi` constraints when we release airflow - so we have a variant of such constraints for every release of airlfow and every python version supported frozen in our GitHub repository so our users can refer to it if they want to somewhat reproducibly install airflow with dependencies and providers that we tested at the moment of release of Airflow.

We do it with a command (auto-generated) similar to this:


```
uv pip install --system '.[all-core]' --reinstall apache-airflow-providers-airbyte==5.0.0 apache-airflow-providers-alibaba==3.0.0 ... --resolution highest
```

## The problem

The problem is that this does not really pull the providers from pypi as expected - but uses the ones which are defined in the workspace.

Our workspace looks more or less like this:

```toml

[dependency-groups]
dev = [
  "apache-airflow-providers-airbyte",
  "apache-airflow-providers-alibaba",
]

[tool.uv.sources]
apache-airflow-providers-airbyte = {workspace = true}
apache-airflow-providers-alibaba = { workspace = true }
# more providers

[tool.uv.workspace]
members = [
    "providers/airbyte",
    "providers/alibaba",
# more providers

```

And the problem is tha when I run pip install command above, it will install and use all the providers and all the devel dependencies defined in the pyproject.toml  - and not "bare" "airlfow" package + all the packages I requested to install.

For example - this is what happens when I run another version of this:

```
root@00376a013642:/opt/airflow# uv pip install ".[all-core]" apache-airflow-providers-airbyte --system
Using Python 3.9.21 environment at: /usr/local
   Built apache-airflow @ file:///opt/airflow
⠦ Resolving dependencies...                                                                                                                                                                                                                                                                                                                                                                   warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-ftp`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-smtp`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-sqlite`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-amazon`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-apache-cassandra`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-apache-drill`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-apache-druid`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-apache-flink`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-apache-hdfs`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-apache-hive`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-apache-impala`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-apache-kafka`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-apache-livy`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-apache-pinot`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-apprise`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-arangodb`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-atlassian-jira`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-cloudant`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-cncf-kubernetes`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-databricks`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-dbt-cloud`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-dingding`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-discord`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-docker`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-elasticsearch`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-fab`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-github`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-google`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-grpc`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-influxdb`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-jdbc`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-microsoft-azure`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-microsoft-mssql`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-microsoft-psrp`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-microsoft-winrm`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-mongo`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-opensearch`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-oracle`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-redis`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-salesforce`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-sendgrid`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-sftp`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-slack`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-trino`
warning: Missing version constraint (e.g., a lower bound) for `apache-airflow-providers-yandex`
Resolved 209 packages in 1m 02s
   Built apache-airflow-providers-common-compat @ file:///opt/airflow/providers/common/compat
   Built apache-airflow-providers-standard @ file:///opt/airflow/providers/standard
   Built apache-airflow-providers-common-sql @ file:///opt/airflow/providers/common/sql
   Built apache-airflow-providers-http @ file:///opt/airflow/providers/http
   Built apache-airflow-providers-common-io @ file:///opt/airflow/providers/common/io
   Built apache-airflow-providers-imap @ file:///opt/airflow/providers/imap
Prepared 12 packages in 3.12s
Uninstalled 8 packages in 16ms
Installed 12 packages in 1.53s
 ~ apache-airflow==3.0.0.dev0 (from file:///opt/airflow)
 ~ apache-airflow-providers-common-compat==1.4.0 (from file:///opt/airflow/providers/common/compat)
 ~ apache-airflow-providers-common-io==1.5.0 (from file:///opt/airflow/providers/common/io)
 ~ apache-airflow-providers-common-sql==1.21.0 (from file:///opt/airflow/providers/common/sql)
 + apache-airflow-providers-fab==1.5.2
 + apache-airflow-providers-ftp==3.12.0
 ~ apache-airflow-providers-http==5.0.0 (from file:///opt/airflow/providers/http)
 ~ apache-airflow-providers-imap==3.8.0 (from file:///opt/airflow/providers/imap)
 + apache-airflow-providers-smtp==1.9.0
 + apache-airflow-providers-sqlite==4.0.0
 ~ apache-airflow-providers-standard==0.0.3 (from file:///opt/airflow/providers/standard)
 - flask-appbuilder==4.5.3
 + flask-appbuilder==4.5.2
```

When you look at this - even if I requested a non-editable version of "airflow" from sources, it still instas editable versions of packages referred to by the workspace - even though what I **REALLY** want in this case I would like to install those packages from PyPI.

This might or might not be a reasonable default, not sure. Depends what your philosophy of workspace is - the fact that you have `pyproject.toml` locally that determines workspace, might mean that you want to use the workspace-packages even in non-editable mode, And this is fine.

But the real problem is that I currently have no way to add parameters to `uv pip install` to change this behaviour. I miss specifically those two:

* --no-install-workspace
* --no-dev

Also others (`--only-dev`) that `uv sync` supports.

Now - why can't I use `uv sync` ? One could think that I could use:

```
uv sync --no-editable --no-install-workspace --extra "[all]" --resolution highest
```

or

```
uv sync --no-editable --no-install-workspace --all-extras --resolution highest
```

But I really can't - because in this case I do NOT want to install the provider packages from my workspace - I want to install them from pypi AND I want to explicitly specify which provider packages I want to use, individually, so `uv pip install` is much more suitable for it.

Currently what I am doing to workaround it is I modify the `pyproject.toml` dynamically before running the `uv pip install` command and remove all providers from the workspace definition. This works as expected. The above command will install packages from PyPI rather than from local workspace and life is good. But it's a nasty hack.

Would you please consider adding extra flags to `pip` install to control the workspace/dev installation behaviour when `uv pip install ,` is run for project which is workspace-enabled in `pyproject.toml`?

This is behaviour observed in 0.5.11 (latest at the time of this issue writing) version.







### Example

_No response_

---

_Label `enhancement` added by @potiuk on 2025-01-27 16:25_

---

_Comment by @konstin on 2025-01-27 16:48_

Does `--no-sources` work for you?

---

_Comment by @potiuk on 2025-01-27 18:07_

AAAAAAhhhh 🤦 .... 

You are a lifesaver @konstin !

I totally, totally missed it when I looked at the commands available and I have not realized it does what I am doing by manually removing the workspaces... 

I checked it and it seems I can get it to work, with few small other tweaks (for example I need to add `--reinstall` flag to make sure that the `workspace` packages are actually replaced by the packages from `PyPI`. 

There are few more tweaks I have to add but generally speaking it does, indeed, solve my problem.

Thank you !

---

_Closed by @potiuk on 2025-01-27 18:07_

---

_Comment by @charliermarsh on 2025-01-27 18:08_

Maybe we should add the word "workspace" somewhere in the help for `--no-sources`.

---

_Comment by @potiuk on 2025-01-27 18:39_

> Maybe we should add the word "workspace" somewhere in the help for `--no-sources`.

Yes. That would be great :)

But actually ... I found a bug with how `--no-sources` implementation ... I will open a separate issue.



---

_Comment by @potiuk on 2025-01-27 18:55_

@charliermarsh @konstin => Issue here: https://github.com/astral-sh/uv/issues/10999

---
