---
layout: post
title: Testing GitHub workflow 
categories: [automation]
---

If you want to test workflows on separate branch before merging on master make sure
to install Github CLI tool `gh`. And it is important to put the `workflow_dispatch`
as a way of triggering the workflow on Github, so the job trigger should look something
like this:

```yaml
name: To Test

on:
  workflow_dispatch:
  pull_request:
  ...
```

and then just execute: `gh workflow run -r <branch_name> <workflow_file>` and done.
Unfortunately it is not possible to run workflows from other branches via GitHub 
UI, but only using `gh` tool.

