---
title: General
description: General information on how to get started developing
published: true
date: 2020-03-14T21:31:38.050Z
tags: getting started, tutorial, help, dev
---

# General Info

Welcome new developer!

We hope you will have a good time developing on TOS.
This page will list everything you need to know about tos before you get started.

All TOS source code is hosted on [github](https://github.com/ODEX-TOS)

Here are a few tips you should know before starting:

- Each repo is one specific part of the operating system
- Most repo's currently have bad documentation (feel free to add them)
- Most repo's have a PKGBUILD. They are useful to see what the dependencies are and how to install the software
- It is recommended that you run an arch based distro as some things depend on that

We obviously expect that the developers know how to use `git` and `github`

In case you don't here is a small list to get you started

- [Learn git](https://www.atlassian.com/git)
- [Fork on github](https://help.github.com/en/github/getting-started-with-github/fork-a-repo)
- [Pull Request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests)
- [GPG Key](https://help.github.com/en/github/authenticating-to-github/generating-a-new-gpg-key)

> You should be familiar with all of the above before contributing code

Lets quickly cover the flow that is expected of a developer.

1. You should for the `TOS repo`
2. Create at least one commit
3. Make sure the commit is signed with `gpg` (more info below)
4. Create a pull request (auto build if applicable shoudl be succesfull)
5. Wait for a merge or comment

## Signing commits

Each commit that is made by someone that is not in the official team **MUST** sign their commits with a
[GPG Key](https://help.github.com/en/github/authenticating-to-github/generating-a-new-gpg-key).
This is done in case some bug happens. That we can always trace it back to you as an individual.

GPG Keys also provided an extra level of trust and it is always recommended to sign of your commits.

## Add Yourself as a contributor to the readme

Every time you make a pull request don't forget to add yourself to the bottom of the readme.
There is a tab called `Acknowledgements`. You should definitely add yourself their.

usually the markdown looks as followed

```md
<!-- ACKNOWLEDGEMENTS -->
## Acknowledgements

* [F0xedb](https://github.com/F0xedb/main-server)
```

Add an extra entry there

Good luck and happy bug hunting :heart:
