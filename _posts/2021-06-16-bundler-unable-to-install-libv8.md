---
layout: default
title: Unable to install libv8, therubyracer or mini_racer with bundler
published: true
tags: ruby bundler
---

This blog posts explains how to get rid of the installation error when
installing therubyracer or mini_racer with bundler 2.2

<!--more-->

# Installation of therubyracer or mini_racer hanging

If you have recently upgraded bundler from 2.1.x to 2.2.x, you might have seen
that the installation of the v8 runtime, such as therubyracer or mini_racer
fails on Ubuntu computers. This happens because of a bundler incompatibility.
From 2.2.x onwards, supported platforms must be listed in the lockfile.

You can fix the error by adding your platform to the lockfile:

```bash
bundle lock --add-platform x86_64-linux
```
