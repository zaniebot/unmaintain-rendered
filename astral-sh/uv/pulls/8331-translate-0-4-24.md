```yaml
number: 8331
title: Translate 0.4.24
type: pull_request
state: closed
author: MtkN1
labels: []
assignees: []
base: main
head: translate-0.4.24
created_at: 2024-10-18T14:36:46Z
updated_at: 2024-10-18T14:57:18Z
url: https://github.com/astral-sh/uv/pull/8331
synced_at: 2026-01-12T16:08:16Z
```

# Translate 0.4.24

---

_@MtkN1_

uv 0.4.24 documentation translated into Japanese.

Known issues:
- Markdown headers are translated and some links are broken
- No space between English and Japanese words

```log
WARNING -  Doc file 'index.md' contains a link '#python-management', but there is no such anchor on this page.
WARNING -  Doc file 'index.md' contains a link '#tool-management', but there is no such anchor on this page.
WARNING -  Doc file 'index.md' contains a link '#script-support', but there is no such anchor on this page.
WARNING -  Doc file 'index.md' contains a link '#project-management', but there is no such anchor on this page.
WARNING -  Doc file 'index.md' contains a link '#the-pip-interface', but there is no such anchor on this page.
WARNING -  Doc file 'index.md' contains a link './guides/scripts.md#declaring-script-dependencies', but the doc 'guides/scripts.md' does not contain an anchor '#declaring-script-dependencies'.
WARNING -  Doc file 'index.md' contains a link './concepts/projects.md#project-lockfile', but the doc 'concepts/projects.md' does not contain an anchor '#project-lockfile'.
WARNING -  Doc file 'concepts/cache.md' contains a link '../guides/integration/github.md#caching', but the doc 'guides/integration/github.md' does not contain an anchor '#caching'.
WARNING -  Doc file 'concepts/dependencies.md' contains a link '#workspace-member', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/dependencies.md' contains a link '#editable-dependencies', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/dependencies.md' contains a link './projects.md#managing-dependencies', but the doc 'concepts/projects.md' does not contain an anchor '#managing-dependencies'.
WARNING -  Doc file 'concepts/dependencies.md' contains a link './projects.md#build-systems', but the doc 'concepts/projects.md' does not contain an anchor '#build-systems'.
WARNING -  Doc file 'concepts/dependencies.md' contains a link '../concepts/resolution.md#source-distribution', but the doc 'concepts/resolution.md' does not contain an anchor
           '#source-distribution'.
WARNING -  Doc file 'concepts/projects.md' contains a link '#creating-projects', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/projects.md' contains a link '#build-systems', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/projects.md' contains a link '#applications', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/projects.md' contains a link '#libraries', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/projects.md' contains a link '#running-commands-with-additional-dependencies', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/projects.md' contains a link '#upgrading-locked-package-versions', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/projects.md' contains a link './dependencies.md#editable-dependencies', but the doc 'concepts/dependencies.md' does not contain an anchor
           '#editable-dependencies'.
WARNING -  Doc file 'concepts/projects.md' contains a link './dependencies.md#development-dependencies', but the doc 'concepts/dependencies.md' does not contain an anchor
           '#development-dependencies'.
WARNING -  Doc file 'concepts/projects.md' contains a link './dependencies.md#optional-dependencies', but the doc 'concepts/dependencies.md' does not contain an anchor
           '#optional-dependencies'.
WARNING -  Doc file 'concepts/projects.md' contains a link './dependencies.md#dependency-sources', but the doc 'concepts/dependencies.md' does not contain an anchor '#dependency-sources'.
WARNING -  Doc file 'concepts/projects.md' contains a link '../guides/scripts.md#declaring-script-dependencies', but the doc 'guides/scripts.md' does not contain an anchor
           '#declaring-script-dependencies'.
WARNING -  Doc file 'concepts/python-versions.md' contains a link '#discovery-of-python-versions', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/python-versions.md' contains a link '#installing-a-python-version', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/python-versions.md' contains a link '#disabling-automatic-python-downloads', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/python-versions.md' contains a link '#requesting-a-version', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/python-versions.md' contains a link '../pip/environments.md#discovery-of-python-environments', but the doc 'pip/environments.md' does not contain an anchor
           '#discovery-of-python-environments'.
WARNING -  Doc file 'concepts/resolution.md' contains a link '#platform-specific-resolution', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/resolution.md' contains a link '#universal-resolution', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/resolution.md' contains a link '#dependency-constraints', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/resolution.md' contains a link '#platform-markers', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/resolution.md' contains a link '../pip/compatibility.md#pre-release-compatibility', but the doc 'pip/compatibility.md' does not contain an anchor
           '#pre-release-compatibility'.
WARNING -  Doc file 'concepts/tools.md' contains a link '#the-path', but there is no such anchor on this page.
WARNING -  Doc file 'concepts/workspaces.md' contains a link './projects.md#applications', but the doc 'concepts/projects.md' does not contain an anchor '#applications'.
WARNING -  Doc file 'concepts/workspaces.md' contains a link './projects.md#libraries', but the doc 'concepts/projects.md' does not contain an anchor '#libraries'.
WARNING -  Doc file 'configuration/authentication.md' contains a link '../pip/compatibility.md#registry-authentication', but the doc 'pip/compatibility.md' does not contain an anchor
           '#registry-authentication'.
WARNING -  Doc file 'configuration/environment.md' contains a link '../concepts/projects.md#configuring-the-project-environment-path', but the doc 'concepts/projects.md' does not contain an
           anchor '#configuring-the-project-environment-path'.
WARNING -  Doc file 'getting-started/installation.md' contains a link '#github-releases', but there is no such anchor on this page.
WARNING -  Doc file 'guides/install-python.md' contains a link '#using-an-existing-python-installation', but there is no such anchor on this page.
WARNING -  Doc file 'guides/install-python.md' contains a link '#automatic-python-downloads', but there is no such anchor on this page.
WARNING -  Doc file 'guides/install-python.md' contains a link '../concepts/python-versions.md#managed-python-distributions', but the doc 'concepts/python-versions.md' does not contain an
           anchor '#managed-python-distributions'.
WARNING -  Doc file 'guides/install-python.md' contains a link '../concepts/python-versions.md#installing-a-python-version', but the doc 'concepts/python-versions.md' does not contain an
           anchor '#installing-a-python-version'.
WARNING -  Doc file 'guides/install-python.md' contains a link '../concepts/python-versions.md#viewing-available-python-versions', but the doc 'concepts/python-versions.md' does not contain an
           anchor '#viewing-available-python-versions'.
WARNING -  Doc file 'guides/install-python.md' contains a link '../concepts/python-versions.md#disabling-automatic-python-downloads', but the doc 'concepts/python-versions.md' does not contain
           an anchor '#disabling-automatic-python-downloads'.
WARNING -  Doc file 'guides/install-python.md' contains a link '../concepts/python-versions.md#discovery-of-python-versions', but the doc 'concepts/python-versions.md' does not contain an
           anchor '#discovery-of-python-versions'.
WARNING -  Doc file 'guides/install-python.md' contains a link '../concepts/python-versions.md#adjusting-python-version-preferences', but the doc 'concepts/python-versions.md' does not contain
           an anchor '#adjusting-python-version-preferences'.
WARNING -  Doc file 'guides/install-python.md' contains a link '../guides/scripts.md#using-different-python-versions', but the doc 'guides/scripts.md' does not contain an anchor
           '#using-different-python-versions'.
WARNING -  Doc file 'guides/projects.md' contains a link '../concepts/projects.md#project-environments', but the doc 'concepts/projects.md' does not contain an anchor '#project-environments'.
WARNING -  Doc file 'guides/projects.md' contains a link '../concepts/projects.md#project-lockfile', but the doc 'concepts/projects.md' does not contain an anchor '#project-lockfile'.
WARNING -  Doc file 'guides/projects.md' contains a link '../concepts/projects.md#managing-dependencies', but the doc 'concepts/projects.md' does not contain an anchor
           '#managing-dependencies'.
WARNING -  Doc file 'guides/projects.md' contains a link '../concepts/projects.md#running-commands', but the doc 'concepts/projects.md' does not contain an anchor '#running-commands'.
WARNING -  Doc file 'guides/projects.md' contains a link '../concepts/projects.md#running-scripts', but the doc 'concepts/projects.md' does not contain an anchor '#running-scripts'.
WARNING -  Doc file 'guides/projects.md' contains a link '../concepts/projects.md#building-projects', but the doc 'concepts/projects.md' does not contain an anchor '#building-projects'.
WARNING -  Doc file 'guides/publish.md' contains a link '../concepts/projects.md#build-systems', but the doc 'concepts/projects.md' does not contain an anchor '#build-systems'.
WARNING -  Doc file 'guides/scripts.md' contains a link '#declaring-script-dependencies', but there is no such anchor on this page.
WARNING -  Doc file 'guides/scripts.md' contains a link '../concepts/projects.md#running-scripts', but the doc 'concepts/projects.md' does not contain an anchor '#running-scripts'.
WARNING -  Doc file 'guides/scripts.md' contains a link '../concepts/python-versions.md#requesting-a-version', but the doc 'concepts/python-versions.md' does not contain an anchor
           '#requesting-a-version'.
WARNING -  Doc file 'guides/tools.md' contains a link './projects.md#running-commands', but the doc 'guides/projects.md' does not contain an anchor '#running-commands'.
WARNING -  Doc file 'guides/integration/alternative-indexes.md' contains a link '../../pip/compatibility.md#packages-that-exist-on-multiple-indexes', but the doc 'pip/compatibility.md' does
           not contain an anchor '#packages-that-exist-on-multiple-indexes'.
WARNING -  Doc file 'guides/integration/dependency-bots.md' contains a link '../../concepts/dependencies.md#project-dependencies', but the doc 'concepts/dependencies.md' does not contain an
           anchor '#project-dependencies'.
WARNING -  Doc file 'guides/integration/dependency-bots.md' contains a link '../../concepts/dependencies.md#optional-dependencies', but the doc 'concepts/dependencies.md' does not contain an
           anchor '#optional-dependencies'.
WARNING -  Doc file 'guides/integration/dependency-bots.md' contains a link '../../concepts/dependencies.md#development-dependencies', but the doc 'concepts/dependencies.md' does not contain
           an anchor '#development-dependencies'.
WARNING -  Doc file 'guides/integration/dependency-bots.md' contains a link '../scripts.md/#declaring-script-dependencies', but the doc 'guides/scripts.md' does not contain an anchor
           '#declaring-script-dependencies'.
WARNING -  Doc file 'guides/integration/docker.md' contains a link '#intermediate-layers', but there is no such anchor on this page.
WARNING -  Doc file 'guides/integration/docker.md' contains a link '../../concepts/projects.md#configuring-the-project-environment-path', but the doc 'concepts/projects.md' does not contain an
           anchor '#configuring-the-project-environment-path'.
WARNING -  Doc file 'guides/integration/docker.md' contains a link '../../concepts/tools.md#the-bin-directory', but the doc 'concepts/tools.md' does not contain an anchor '#the-bin-directory'.
WARNING -  Doc file 'guides/integration/fastapi.md' contains a link '../../concepts/projects.md#applications', but the doc 'concepts/projects.md' does not contain an anchor '#applications'.
WARNING -  Doc file 'guides/integration/github.md' contains a link '../../concepts/projects.md#configuring-the-project-environment-path', but the doc 'concepts/projects.md' does not contain an
           anchor '#configuring-the-project-environment-path'.
WARNING -  Doc file 'guides/integration/gitlab.md' contains a link 'docker.md#available-images', but the doc 'guides/integration/docker.md' does not contain an anchor '#available-images'.
WARNING -  Doc file 'guides/integration/gitlab.md' contains a link '../../concepts/cache.md#caching-in-continuous-integration', but the doc 'concepts/cache.md' does not contain an anchor
           '#caching-in-continuous-integration'.
WARNING -  Doc file 'pip/compatibility.md' contains a link '../configuration/indexes.md#pinning-a-package-to-an-index', but the doc 'configuration/indexes.md' does not contain an anchor
           '#pinning-a-package-to-an-index'.
WARNING -  Doc file 'pip/compatibility.md' contains a link './environments.md#using-arbitrary-python-environments', but the doc 'pip/environments.md' does not contain an anchor
           '#using-arbitrary-python-environments'.
WARNING -  Doc file 'pip/compile.md' contains a link 'packages.md#installing-packages-from-files', but the doc 'pip/packages.md' does not contain an anchor '#installing-packages-from-files'.
WARNING -  Doc file 'pip/dependencies.md' contains a link './packages.md#installing-packages-from-files', but the doc 'pip/packages.md' does not contain an anchor
           '#installing-packages-from-files'.
WARNING -  Doc file 'pip/environments.md' contains a link '../concepts/python-versions.md#discovery-of-python-versions', but the doc 'concepts/python-versions.md' does not contain an anchor
           '#discovery-of-python-versions'.
WARNING -  Doc file 'pip/packages.md' contains a link '../configuration/authentication.md#git-authentication', but the doc 'configuration/authentication.md' does not contain an anchor
           '#git-authentication'.
WARNING -  Doc file 'reference/resolver-internals.md' contains a link '../concepts/resolution.md#resolution-strategy', but the doc 'concepts/resolution.md' does not contain an anchor
           '#resolution-strategy'.
```

---

_Comment by @MtkN1 on 2024-10-18 14:38_

This is a mistake 🙇‍♂️  I actually meant to send a pull request to my Fork project. Sorry for any inconvenience caused.

---

_Closed by @MtkN1 on 2024-10-18 14:38_

---
