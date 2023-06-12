---
author: Satyam Bansal
pubDatetime: 2023-06-05T01:00:00
title: First Week of Coding Period | GSoC 2023
postSlug: first-week-of-coding-period-gsoc-2023
featured: true
draft: false
tags:
  - gsoc
  - zulip
  - open source
  - coding period
ogImage: ""
description: "Blog post about my first week of coding period for GSoC 2023."
---

<!-- ## Table of contents -->

## Introduction

The coding period for GSoC 2023 has officially started. Nowadays, I am
mostly working on improving the integrations provided by Zulip. This
blog post will be a summary of the work I have done in the first week
of the coding period. I will also talk about the challenges I faced
and the lessons I learned along the way.

## The first week

I think I have worked with Zulip for almost 3 months so the coding
period didn't felt like something different. I had some issues figured
out on which I could work. One of them was related to the [GitHub
Integration](https://zulip.com/integrations/doc/github) and another
was related to [GitLab
Integration](https://zulip.com/integrations/doc/gitlab). Apart from
these, I also started working on creating a highly requested but small
[feature](https://github.com/zulip/zulip/issues/12207) for Zulip which
is about adding an option to add a link to an audio call -- right now
through [Jitsi Meet](https://meet.jit.si/) -- to the chat.

## GitHub Integration

The issue was regarding changing the behavior of notifications sent by
the integration when an issue is labeled or unlabeled. Currently, the
integration sends a very vague notification that had no information
about the labels added or removed. So, I started working on this
issue.

I have been working on the GitHub integration for a while now. So, I
was familiar with the codebase. I started by adding the fixtures for
the events. Then, I had to modify the codebase to send a better
message that included the labels added or removed. After some
[discussion](https://chat.zulip.org/#narrow/stream/127-integrations/topic/.2325789/near/1583383)
with the community members it was decided to do something so that the
end-user can decide whether they want the notification for the issue
labeled events or not.

The way we provide the end-user with the choices is that they can add
query params to the URL on the webhook mentioning the events for which
they want to receive the notifications -- they can also exclude some
events if they want.

This is what the documentation says:

```
To filter the events that trigger the notifications, you can append either &only_events=["event_a","event_b"] or &exclude_events=["event_a","event_b"] (or both, with different events) to the URL with an arbitrary number of supported events.
```

On the flipside GitHub also provides filtering of events, but they do
not provide that granular controls over the events that was required
in this case. In their UI, the choice is to either get all events
related to Issues or none.

I have made the changes to [Pull
Request](https://github.com/zulip/zulip/pull/25831) and I think it is
ready for review. It should be merged soon! 🔥

## GitLab Integration

So, GitLab in its 16.0 release decide to deprecate the URL format that
they used earlier. This change kind of brokes the integration as till
now the integration would create various URLs by taking information
from different parts of the payload. But, that format is no longer
supported. So, I started working on this issue.

The [issue](https://github.com/zulip/zulip/issues/25643) mentioned
that we should use the link provided by GitLab in the payload. So, I
started working on this issue. I started by adding the fixtures for
the events. Then, I had to modify the codebase to use the link
provided by GitLab.

While I was working on this one, I realized that the payload that is
received when a `comment` is done a `snippet` didn't contain the link.
So, I had to modify the codebase to handle that case as well. So for
that, I created a post on the GitLab forum and left. Now when I told
my mentor about this, he told me why not go ahead and fix that by
myself as GitLab is an open-source project. So, right now I have
started working on that as well. But I think that experience required
a separate blog post. So, I will write about that later.

## Feature Request

Before explaining the feature request, I would like to give a brief
backstory about the current feature that is available in Zulip. So
when a user is composing a message they can add a link to a video call
by clicking on the video call icon. This will add a link to the
message which then can be used by other users to join the video call.
For [Jitsi Meet](https://meet.jit.si/), when the link is clicked, it
opens a new tab in the browser and the user's camera is turned on
automatically. This feature request is about adding an audio call
button in which the user's camera is not turned on automatically.

I started working on this issue. A similar feature of adding a Video
Call was already available in Zulip. So, I had to figure out how that
was implemented and then had to implement the same for Audio Call. I
have created an implementation of that but the design has to be
discussed in [chat.zulip.org](https://chat.zulip.org/). So, I am
waiting for the feedback from the community.

## Conclusion

So, this was a summary of the work I have done in the first week of
the coding period. I will be writing more blog posts in the coming
weeks. So, stay tuned! 🔥
