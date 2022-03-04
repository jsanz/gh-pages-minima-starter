---
permalink: /docmerge
layout: page
title: Docmerge App
---

I'm a core team member of the Cosmos Project, which you may or may not have heard about. Lately, we've had more PRs made which involve editing markdown files, which makes 'waiting for AppVeyor builds to complete' a waste of time. This is because the build status is going to be exactly the same as the last time the source was modified.  

To solve this, I have discovered a appveyor.yml script which exits the appveyor build cleanly. I cannot run this check until the source is cloned, and the multi-project nature of Cosmos is making appveyor freak out as to where the main Cosmos repository itself is cloned in the source tree. This has led me to create a GitHub App that uses the GitHub API to set the build status to 'succeeded' for commits where only .md files are edited. This will be for our pull requests, as 'master' is a protected branch, and only reviewers with write-access (myself and a team of others) can merge these PRs. As we want more up-to-date documentation for the Cosmos project, this app will solve our problem efficiently.

This page will serve as the App's homepage
