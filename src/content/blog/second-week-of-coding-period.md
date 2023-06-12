---
author: Satyam Bansal
pubDatetime: 2023-06-12T18:00:00
title: Second Week of Coding Period | GSoC 2023
postSlug: second-week-of-coding-period-gsoc-2023
featured: true
draft: false
tags:
  - gsoc
  - zulip
  - open source
  - coding period
ogImage: ""
description: "Work I did during the second week of the coding period for GSoC 2023."
---

## Introduction

During the second week of the coding period, I accomplished the
following tasks:

1.  Implemented a feature to add a link to an audio call to the chat.
1.  Modified the design of the Bulk Delete Drafts UI.
1.  Fixing the script that transfer assets from Zulip Python Bots
    [Repository](https://github.com/zulip/python-zulip-api) to the
    production tarball of Zulip.
1.  Made contributions to GitLab.

I believe my contribution to GitLab deserves a dedicated blog post,
which I will write separately. In this blog post, I will discuss the
remaining three things.

## Audio call feature for Zulip

The feature is about adding an option to add a link to an audio call,
similar to how the users can [add a link to a video
call](https://zulip.com/help/start-a-call). Jitsi Meet, BigBlueButton,
and Zoom are the three video call providers in Zulip.

Here an audio call is not actually an audio call, it's a video call
but when the user opens the link the camera is turned off by default.
So, the user can use it as an audio call.

The change UI is very minimal -- just a 📞 call icon had to be added
and the functionality is pretty much similar but I had adjust how the
link is created for the audio call as some extra parameters had to be
added to the URL.

| <center>Before </center>                 | <center>After </center>                |
| ---------------------------------------- | -------------------------------------- |
| ![Before](/assets/audio_call_before.png) | ![After](/assets/audio_call_after.png) |

For Jitsi Meet, the URL creation is pretty simple. All one has to do
is to simply append a meeting name to the URL and you have a new video
call link. So, in Zulip the implementation is also done pretty much
the same way. A 15 digit random number is generated and appended to
the URL and we have a new video call link. Pretty simple, right?

You can also have a look at the code that does this
[here](https://github.com/zulip/zulip/blob/85681546ce5046f9663bf23358a5caae27d04493/web/src/compose.js#L691-L693).
Although there are other things in the code that might not be very
straightforward. But I think you can get the idea.

A sample URL for Jitsi Meet can be:

```
https://meet.jit.si/123456789012345
```

After the link is generated all that is left is to add
`#config.startWithVideoMuted=true` to the end of the URL and then we
have a new audio call link. The documentation for this can be found on
this
[link](https://community.jitsi.org/t/url-parameters-to-join-jitsi-meet-with-camera-and-or-mic-disabled/32341/2).

So the final URL for Jitsi Meet with the camera turned off by default
will be:

```
https://meet.jit.si/123456789012345#config.startWithVideoMuted=true
```

The [Pull Request](https://github.com/zulip/zulip/pull/25922) is in a
very early stage and requires manual testing. I also need to create
node-tests for this feature. I will keep updating about this in my
further posts.

## Design of the Bulk Delete Drafts UI

Right now, if a user has to delete multiple drafts they have to delete
them one by one which is a very tedious task. There is an
[issue](https://github.com/zulip/zulip/issues/19360) open to track the
feature that will make it possible to delete multiple drafts at once.

The [groundwork](https://github.com/zulip/zulip/pull/19943) was
already done by another contributor for this feature. So I took that
PR, rebased it -- had to resolve some conflicts -- and started working
on polishing the UI.

The main change I made was making use of flexboxes to make the UI more
clean and responsive. Discussion around how things should appear on
smaller screens was also done on
[chat.zulip.org](https://chat.zulip.org/#narrow/stream/101-design/topic/bulk.20delete.20drafts.20.2319943/near/1568814).

This UI had a very interesting point to it. I had to make the `Select
all drafts` button have a checkbox in it, which can be easily
implemented by using a checkbox inside a button. But the problem was
that this has done accessibility issues -- when using keyboard to
press the button it was not working properly -- which were [pointed
out](https://chat.zulip.org/#narrow/stream/101-design/topic/bulk.20delete.20drafts.20.2319943/near/1585514)
by my fellow contributor at Zulip.

![Bulk Delete Drafts UI](/assets/bulk_delete_drafts_ui.png)

The solution that came out of the discussion was to not use a checkbox
instead use an icon that looks like a checkbox. (Modern Problems,
Modern Solutions 😌) This way the button will work properly and the
user will also get an impression that there is a checkbox inside the
button. To make sure that the screen reader also reads the button as a
checkbox, I was suggested to add `aria-checked` attribute to the
button.

The [Pull Request](https://github.com/zulip/zulip/pull/25610) has
still some work left to be done -- some minor changes to the UI are
required and then it will be ready to be merged.

## Fixing script that transfer assets from Zulip Python Bots Repository to the production tarball of Zulip

Zulip supports [interactive bots](https://zulip.com/api/running-bots)
that can be used to interact with the user and perform some tasks.
These bots are written in Python and are hosted in a separate
[repository](https://github.com/zulip/python-zulip-api). When the
Zulip server is built, the assets from this repository are transferred
to the production tarball of Zulip.

To perform the transfer,
[this](https://github.com/zulip/zulip/blob/main/tools/setup/generate_zulip_bots_static_files.py)
script is used. The main issue was that a user
[reported](https://chat.zulip.org/#narrow/stream/9-issues/topic/http.20500.20in.20xkcd.20bot.20docs/near/1579260)
that the documentation page for the `xkcd` bot was not opening. After
inspection of the code, it was
[found](https://chat.zulip.org/#narrow/stream/9-issues/topic/http.20500.20in.20xkcd.20bot.20docs/near/1579735)
that the script was not transferring the contents of `assets`
directory for all the bots. So the fix was to add some code to the
script that will enable that.

To solve this issue I had to look upon various methods that are used
for file handling in Python, viz.

- `os.path.basename`
- `os.path.dirname`
- `os.path.join`
- `shutil.copy`
- `shutil.copy2`
- `shutil.copyfile`
- `shutil.copytree`

I used ChatGPT a lot to understand how these methods work, it also
churned out some very good examples that cleared out things for me.
Also, it was certainly a good experience as compared to doing file
handling in C/C++. 😉

The [Pull Request](https://github.com/zulip/zulip/pull/25888) for this
is under review and the issue should be fixed soon. I am having some
Django trouble here which I need to work on and then the PR will be
ready to be merged.

## Conclusion

I think the week was pretty productive. I was able to do a good amount
of work and also learned a lot of new things. Still I think I can do
better and I will try to do that in the coming weeks.

Feel free to drop any suggestions either on my
[Twitter](https://twitter.com/sbansal1999) or you can do it the school
way by sending me an email at
[sbansal1999@gmail.com](mailto:sbansal1999@gmail.com)
