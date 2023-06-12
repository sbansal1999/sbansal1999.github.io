---
author: Satyam Bansal
pubDatetime: 2023-06-02T19:00:00
title: My Journey as a Google Summer of Code (GSoC) Contributor for Zulip
postSlug: journey-as-gsoc-contributor-for-zulip
featured: true
draft: false
tags:
  - gsoc
  - zulip
  - open source
ogImage: ""
description: From first PR to the final evaluation, I'll talk about my journey to become a GSoC contributor for Zulip.
---

<!-- ## Table of contents -->

## Introduction

In this blog post, I will share my exciting experience as a
contributor for Zulip in the [Google Summer of
Code](https://summerofcode.withgoogle.com/) (GSoC) 2023 program. I'll
talk about the following things in this blog post:

- Selection process of GSoC
- The task of migrating Zulip's codebase from JavaScript to TypeScript
- Challenges encountered during the migration
- Valuable lessons I learned along the way

Join me as I recount my journey in this blog post.

## Selecting the organization

Selecting the organization was a tough task for me. I had to go
through the list of all the organizations that were participating and
pick an organization whose tech stack was somewhat familiar to me. I
entered `javascript` in the search bar and searched from the results.
I found Zulip and started exploring it. I discovered that there was a
project proposal for migrating the codebase from JavaScript to
TypeScript. I started exploring the codebase and found that it was
something I could work on.

## My first PR

The next stage was about setting up the development environment. This
setup was made very easy by the superb documentation provided by
Zulip. My [first PR](https://github.com/zulip/zulip/pull/24484) simply
renamed a couple of files from `*.js` to `*.ts`. 😅

I would say the first PR was more like a lesson on Git and GitHub. It
made me go through the [Commit
Discipline](https://zulip.readthedocs.io/en/latest/contributing/commit-discipline.html)
that Zulip follows. I also learned about various Git commands that I
think have helped me a lot.

## After the first PR

After the first PR, I started working on migrating other modules to
TypeScript. It was during this process that I realized the presence of
cyclic dependencies in the codebase. I started reading about what they
are and how they can be resolved. Understanding the concept of cyclic
dependencies is not very difficult, but resolving them is a non-trivial
task. Sometimes [dependency
injection](https://medium.com/software-ascending/circular-dependencies-in-dependency-injection-403b790daebb#:~:text=Dependency%20Injection%20is%20a%20way,be%20provided%20to%20your%20class.)
can be used to resolve them, sometimes the code needs to be refactored
to resolve them.

After
[discussions](https://chat.zulip.org/#narrow/stream/6-frontend/topic/typescript.20migration/near/1514527)
with other contributors, it was decided that we should first focus on
migrating modules that do not have any cyclic dependencies and address
the cyclic dependencies later. After working on the migration for a
month or so, I felt that working on just one direction was a bit
boring and also it's always good to have multiple things to work on
because every PR will go through a review cycle which can take some
time so you certainly don't want to just sit around during that time.
Thus, I started focusing on UI and Integrations.

I found a bug related to the tooltips in the UI, which were not
displayed properly. The tooltips were created using the `tippy.js`
library. I had to explore the library and then fix the bug. After
fixing this one, I worked on bunch of issues related to UI and
Integrations. All of this happened before the GSoC results were
announced.

## GSoC Results

After all this I was selected as a GSoC contributor for Zulip. I was
very happy to see my name on the list of accepted students. There was
some glitch also which kind of announced the results a bit earlier. 🤫

![GSoC Acceptance](/assets/gsoc_accepted.png)

I will try to post my learnings and things I did weekly during the
GSoC coding period. I feel there is a need of blog on the proposal
writing process and what things matter in the proposal. I will try to
write that blog post soon.

Thanks for reading! 💓
